# PetApp — Product Plan

## Vision
A mobile-first pet community platform built for Singapore, with ambition to expand across Southeast Asia. PetApp brings together social connection, practical utility, and playful gamification into a single app for pet owners.

## Core Pillars

### 1. Social Community
Instagram-style photo feed and Reddit-style forum threads. Pet owners share moments, ask questions, and connect over shared breeds, areas, and interests.

### 2. Singapore Utility Hub
Pre-curated directory of vets, groomers, boarders, and pet services across Singapore. Each listing is team-verified. Filterable by location, pet type, and service.

### 3. Pet Gamification — The Character System
Every pet gets a game character that mirrors its real life. Logging walks, meals, vet visits, and activities updates the character's stats and XP. Characters are built from layered sprites (body, ears, fur, colour, eyes, accessories). AI classifies the breed from a photo and pre-fills the character creator; the owner refines manually.

### 4. Identity & Sharing
Each pet gets a Polaroid-style shareable card with a unique QR code. Scan to view the pet's public profile. Supports multi-pet accounts.

### 5. Lost & Found + Adoption
Community lost and found board with push notifications for nearby sightings. Adoption board pre-populated via SPCA partnership.

## Target Users
- Primary: Singapore-based pet owners aged 22–45
- Secondary: Vets, groomers, and pet service providers
- Tertiary: People looking to adopt

## Business Goals (V1)
- Validate social + utility loop in Singapore market
- Build directory inventory via manual curation + SPCA partnership
- Establish character system as the differentiating hook

## Regional Ambition (V3+)
Expand directory and community to Malaysia, Thailand, Philippines. Localise content and service listings.

## Tech Stack Summary
- Mobile: React Native + Expo
- Backend: NestJS + PostgreSQL + Drizzle ORM + Redis + BullMQ
- Auth: Supabase Auth (Google, Apple, Facebook)
- Storage: Cloudflare R2 (presigned uploads, never through server)
- Realtime: Supabase Realtime or Socket.io
- Animation: Rive (character animations, V2+)
- AI: Google Vision API (breed classification), pixel sampling (colour extraction)
- Hosting: Fly.io Singapore region
- Push: Expo + FCM/APNs

## Principles
- Mobile-first — all UX decisions start from the phone
- Social auth only — no email/password accounts
- Async by default — heavy work via BullMQ, never blocking the request cycle
- Singapore-first — localise before generalising
