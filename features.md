# PetApp — Feature List by Phase

---

## V1 — Foundation (Build First)

### Auth
- [ ] Social login: Google, Apple, Facebook via Supabase Auth
- [ ] JWT guard on all protected NestJS routes
- [ ] User profile creation on first login (display name, avatar)

### Pet Profiles
- [ ] Create pet profile (name, species, breed, birthday, bio, photo)
- [ ] Multi-pet support per user account
- [ ] Edit and soft-delete pet profiles
- [ ] Static breed-based game character (sprite, no animation yet)
- [ ] AI breed classification from photo (Google Vision API) pre-fills character creator
- [ ] Manual character refinement (body, ears, fur, colour, eyes)
- [ ] Polaroid shareable card per pet
- [ ] Unique QR code per pet linking to public profile

### Social Feed
- [ ] Photo feed (home screen, chronological + ranked)
- [ ] Post creation: photo + caption, tag pet(s)
- [ ] Paw reaction (single reaction type, V1)
- [ ] Comments on posts
- [ ] Follow other users / follow back
- [ ] User profile page showing their pets and posts

### Directory
- [ ] Vet listings (name, address, hours, phone, map pin)
- [ ] Groomer listings
- [ ] Boarder listings
- [ ] Filter by service type and area
- [ ] Listing detail page
- [ ] Team-curated data, admin-managed

### Adoption Board
- [ ] Adoption listings pre-populated via SPCA
- [ ] Listing detail: photo, description, contact info
- [ ] Filter by species and age

### Lost & Found
- [ ] Post a lost pet (photo, description, last seen location)
- [ ] Post a found pet
- [ ] Push notifications for new lost/found in user's area
- [ ] Mark as resolved

### Notifications
- [ ] Lost & found push notifications (Expo + FCM/APNs)
- [ ] Reaction and comment notifications (in-app)

### Infrastructure
- [ ] Presigned R2 upload flow for all media
- [ ] BullMQ queues: AI classification, push notifications, XP calculation
- [ ] Soft deletes on all user-owned data
- [ ] Indexes on all FK and high-query columns

---

## V2 — Engagement & Animation

### Character System (Animated)
- [ ] Rive animation integration for game characters
- [ ] XP and level system: walks, meals, vet visits update stats
- [ ] Activity logging: walk log, meal log, vet visit log
- [ ] Character stat display: health, happiness, stamina, hygiene
- [ ] Streak system for consistent logging
- [ ] Character accessories (unlockable via XP)

### Social
- [ ] Stories / 24h disappearing posts
- [ ] Multiple reaction types (beyond paw)
- [ ] Hashtag and breed filtering on feed
- [ ] Explore / discover page
- [ ] Saved posts

### Forum
- [ ] Reddit-style threaded forum
- [ ] Sub-communities by breed, area, topic
- [ ] Upvote/downvote on posts and comments
- [ ] Flair and tags

### Directory Enhancements
- [ ] User reviews and ratings on listings
- [ ] Bookmark favourite listings
- [ ] Claimed listings — service providers can manage their own page

---

## V3 — Regional Expansion

- [ ] Multi-country directory (Malaysia, Thailand, Philippines)
- [ ] Localised content and currency
- [ ] In-app vet appointment booking (marketplace layer)
- [ ] Pet insurance comparison widget
- [ ] Events board (pet meetups, adoption drives)
- [ ] Breed wiki with Singapore-specific care notes

---

## V4 — Monetisation & Scale

- [ ] Premium subscriptions (advanced character customisation, ad-free)
- [ ] Promoted listings for service providers
- [ ] Affiliate links on insurance and food products
- [ ] Pet ID digital passport (printable, shareable)
- [ ] API for vet clinic integrations
- [ ] Analytics dashboard for service providers
