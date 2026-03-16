# PetApp — Execution Notes

Technical decisions, architecture rationale, and implementation guidance.

---

## Auth

### Decision: Supabase Auth, social-only
No email/password. Google, Apple, and Facebook only. Supabase handles OAuth token exchange and issues a JWT. The NestJS backend validates that JWT on every protected request via a Guard. The frontend never sees raw credentials.

- Supabase JWT secret is used to verify tokens server-side via `@nestjs/passport` + `passport-jwt`
- User record is created in our own PostgreSQL `users` table on first login (triggered from the backend, not Supabase webhooks — keeps logic centralised)

---

## Media Uploads

### Decision: Presigned R2 uploads, server never receives file bytes
All media (pet photos, post images, avatars) is uploaded directly from the client to Cloudflare R2 using presigned PUT URLs. The backend generates the URL, returns it to the client, the client uploads, then the client notifies the backend with the final R2 key.

Flow:
1. Client calls `POST /media/presign` with `{ filename, contentType }`
2. Backend returns `{ uploadUrl, key }`
3. Client PUTs directly to R2
4. Client calls `POST /media/confirm` with `{ key }`
5. Backend marks the key as confirmed and attaches it to the resource

---

## Background Processing

### Decision: BullMQ for all heavy/async work
Nothing heavy runs synchronously in a request handler. Queue workers run in separate processes.

Queues:
- `ai-classification` — triggered after pet photo upload; calls Google Vision API; updates pet breed suggestion
- `push-notifications` — triggered by lost/found posts and social events
- `xp-calculation` — triggered after activity logs; updates character stats (V2)

Redis is required for BullMQ. Redis also used for caching hot directory listings and user session metadata.

---

## Database

### Decision: PostgreSQL + Drizzle ORM
- Drizzle is the ORM of choice — type-safe, lightweight, close to raw SQL
- Schema defined in `backend/src/db/schema.ts`
- Migrations via `drizzle-kit`

### Key schema decisions:
- `users` table: id (uuid), supabase_user_id, display_name, avatar_key, created_at, deleted_at
- `pets` table: id, user_id (FK), name, species, breed, birthday, bio, photo_key, character_config (JSONB), qr_code, deleted_at
- `character_config` stored as JSONB: `{ body, ears, fur, colour, eyes, accessories[] }`
- `posts` table: id, user_id, pet_ids (array FK), media_key, caption, created_at, deleted_at
- `reactions` table: id, post_id, user_id, type, created_at
- `directory_listings` table: id, type (vet/groomer/boarder), name, address, lat, lng, hours (JSONB), phone, deleted_at
- `lost_found` table: id, user_id, type (lost/found), pet_id, description, last_seen_lat, last_seen_lng, resolved_at, deleted_at

### Indexes (apply from day one):
- All FK columns indexed
- `posts(created_at DESC)` for feed queries
- `directory_listings(type, lat, lng)` for geo queries
- `lost_found(resolved_at, created_at DESC)` for active board

### Soft deletes:
All user-owned tables have a `deleted_at` timestamptz column. Queries always filter `WHERE deleted_at IS NULL`.

---

## NestJS Module Structure

```
backend/src/
  app.module.ts          # Root module
  db/                    # Drizzle schema + connection
  modules/
    auth/                # JWT guard, Supabase validation
    users/               # User profile management
    pets/                # Pet profiles, character config
    media/               # Presign + confirm upload flow
    feed/                # Posts, reactions, comments
    directory/           # Vet/groomer/boarder listings
    adoption/            # Adoption board
    lost-found/          # Lost & found board
    notifications/       # Push notification dispatch
    ai/                  # Breed classification worker
  queues/                # BullMQ queue definitions and processors
  common/                # Guards, interceptors, decorators, pipes
```

---

## Mobile App Structure (Expo)

```
mobile/
  app/
    _layout.tsx          # Root navigator
    (auth)/              # Login / onboarding screens
    (tabs)/              # Main tab navigator
      feed/              # Home feed
      explore/           # Explore / directory
      pets/              # Pet profiles
      profile/           # User profile
    lost-found/          # Lost & found board
    adoption/            # Adoption board
    pet/[id]/            # Pet public profile (QR scan target)
  components/
    feed/
    pets/
    character/
    directory/
    shared/
  hooks/
  stores/                # Zustand stores
  services/              # API client functions
  constants/
  assets/
```

---

## Realtime

Supabase Realtime is the first choice for feed updates and lost/found notifications, since Supabase Auth is already a dependency. Socket.io is the fallback if Supabase Realtime proves too limiting.

---

## Character System (V1 — Static)

V1 ships a static sprite-based character. No animation. The character is defined entirely by `character_config` JSONB. The frontend renders the character by compositing sprite layers in order: background > body > ears > fur > colour > eyes > accessories.

Sprite assets are stored in R2 and served via Cloudflare CDN. Layer keys in `character_config` map to asset filenames.

AI breed classification (Google Vision API) is triggered on first pet photo upload and returns a breed suggestion. The character creator is pre-filled with breed-matched defaults. Owner can override everything manually.

Rive animation is deferred to V2.

---

## QR Codes

Each pet gets a unique UUID-based QR code on creation, stored in the `pets.qr_code` column. The QR encodes a URL like `https://petapp.sg/p/{pet_uuid}`. The Polaroid card renders this QR. The public profile endpoint `GET /pets/:id/public` is unauthenticated.

---

## Hosting

- Fly.io Singapore region (`sin` or `sgp`) for lowest latency to target users
- Separate Fly apps for backend and any background workers
- Cloudflare in front of R2 for CDN
- Supabase project hosted in closest available region (ap-southeast-1)

---

## Security

- JWT validated on every protected route via `AuthGuard`
- R2 presigned URLs expire after 15 minutes
- No user-generated HTML rendered without sanitisation
- Rate limiting on auth and upload endpoints
- All IDs are UUIDs, never sequential integers exposed in URLs
