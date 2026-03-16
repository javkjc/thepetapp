# PetApp — Session State

Tracks current build progress. Update this file at the end of every work session.

---

## Current Phase: V1

## Status: Setup complete — ready to build

---

## Done

- [x] NestJS backend scaffolded at `backend/`
- [x] Expo mobile app scaffolded at `mobile/`
- [x] Both verified running locally
- [x] Governance files created (plan, features, executionnotes, session-state, codemapcc, lessons, ai-rules)

---

## In Progress

- Nothing yet

---

## Up Next (V1 Build Order)

1. **Database schema** — Drizzle schema file, migration setup, all V1 tables
2. **Auth module** — Supabase JWT guard, user creation on first login
3. **Media module** — Presign + confirm R2 upload flow
4. **Pets module** — CRUD for pet profiles, character config
5. **AI queue** — Breed classification worker (Google Vision API)
6. **Feed module** — Posts, reactions, comments
7. **Directory module** — Listings CRUD, seed data
8. **Adoption module** — Board + SPCA seed data
9. **Lost & found module** — Board + push notification trigger
10. **Notifications module** — Expo push dispatch worker
11. **Mobile: Auth screens** — Login, onboarding
12. **Mobile: Pet creation flow** — Photo upload, AI pre-fill, character editor
13. **Mobile: Feed** — Home tab, post creation, reactions
14. **Mobile: Directory** — Listings, filter, detail
15. **Mobile: Adoption** — Board and detail
16. **Mobile: Lost & Found** — Board, post, resolve
17. **Mobile: Polaroid card + QR** — Shareable card screen

---

## Blockers

- None

---

## Last Updated

2026-03-16 — Initial governance setup session
