# PetApp — Code Map

Folder structure and module map. Update as new modules and files are added.

---

## Repository Root

```
PetApp/
  backend/               # NestJS API
  mobile/                # Expo React Native app
  plan.md
  features.md
  executionnotes.md
  session-state.md
  codemapcc.md           # This file
  lessons.md
  ai-rules.md
```

---

## Backend (`backend/`)

### Current (scaffolded)

```
backend/
  src/
    app.module.ts        # Root module
    app.controller.ts    # Health check
    app.service.ts
    main.ts              # Bootstrap, global pipes/guards
  test/
    app.e2e-spec.ts
  nest-cli.json
  package.json
  tsconfig.json
  tsconfig.build.json
  .prettierrc
  eslint.config.mjs
```

### Target Structure (V1)

```
backend/
  src/
    app.module.ts
    main.ts
    db/
      index.ts           # Drizzle client singleton
      schema.ts          # All table definitions
      migrations/        # Drizzle migration files
    modules/
      auth/
        auth.module.ts
        auth.guard.ts    # JWT validation guard
        auth.strategy.ts # passport-jwt strategy
        auth.types.ts    # Req user type
      users/
        users.module.ts
        users.controller.ts
        users.service.ts
        users.dto.ts
      pets/
        pets.module.ts
        pets.controller.ts
        pets.service.ts
        pets.dto.ts
      media/
        media.module.ts
        media.controller.ts
        media.service.ts  # R2 presign logic
      feed/
        feed.module.ts
        feed.controller.ts
        feed.service.ts
        feed.dto.ts
      directory/
        directory.module.ts
        directory.controller.ts
        directory.service.ts
      adoption/
        adoption.module.ts
        adoption.controller.ts
        adoption.service.ts
      lost-found/
        lost-found.module.ts
        lost-found.controller.ts
        lost-found.service.ts
      notifications/
        notifications.module.ts
        notifications.service.ts  # Expo push dispatch
      ai/
        ai.module.ts
        ai.processor.ts   # BullMQ processor for breed classification
    queues/
      queue.constants.ts  # Queue name constants
      queues.module.ts    # BullMQ module registration
    common/
      guards/
        auth.guard.ts
      decorators/
        current-user.decorator.ts
      interceptors/
        transform.interceptor.ts
      pipes/
        validation.pipe.ts
```

---

## Mobile (`mobile/`)

### Current (scaffolded)

```
mobile/
  app/
    _layout.tsx
    (tabs)/
      _layout.tsx
      index.tsx          # Home tab placeholder
      explore.tsx        # Explore tab placeholder
    modal.tsx
  components/
    haptic-tab.tsx
    themed-text.tsx
    themed-view.tsx
    external-link.tsx
    hello-wave.tsx
    parallax-scroll-view.tsx
    ui/
      collapsible.tsx
      icon-symbol.tsx
      icon-symbol.ios.tsx
  constants/
    theme.ts
  hooks/
    use-color-scheme.ts
    use-color-scheme.web.ts
    use-theme-color.ts
  scripts/
    reset-project.js
  assets/
    images/
  tsconfig.json
  eslint.config.js
```

### Target Structure (V1)

```
mobile/
  app/
    _layout.tsx           # Root: auth check, nav setup
    (auth)/
      _layout.tsx
      login.tsx           # Social login screen
      onboarding.tsx      # First pet creation prompt
    (tabs)/
      _layout.tsx
      index.tsx           # Feed (home)
      explore.tsx         # Directory + search
      pets.tsx            # My pets tab
      profile.tsx         # My profile
    lost-found/
      index.tsx           # Board
      new.tsx             # Post lost/found
      [id].tsx            # Detail
    adoption/
      index.tsx           # Board
      [id].tsx            # Detail
    pet/
      [id].tsx            # Public pet profile (QR scan target)
    post/
      [id].tsx            # Post detail + comments
  components/
    feed/
      PostCard.tsx
      PostComposer.tsx
      ReactionButton.tsx
    pets/
      PetCard.tsx
      PetCreationForm.tsx
      PolaroidCard.tsx
      QRCodeDisplay.tsx
    character/
      CharacterCanvas.tsx  # Layer compositor
      CharacterEditor.tsx
    directory/
      ListingCard.tsx
      ListingDetail.tsx
      DirectoryFilter.tsx
    shared/
      Avatar.tsx
      Button.tsx
      Input.tsx
      ScreenHeader.tsx
      LoadingSpinner.tsx
      EmptyState.tsx
  hooks/
    use-auth.ts
    use-pets.ts
    use-feed.ts
    use-directory.ts
    use-upload.ts         # Presign + upload flow
  stores/
    auth.store.ts         # Zustand: current user/session
    pets.store.ts
    feed.store.ts
  services/
    api.ts                # Axios/fetch base client
    auth.service.ts
    pets.service.ts
    feed.service.ts
    media.service.ts
    directory.service.ts
    lost-found.service.ts
    adoption.service.ts
  constants/
    theme.ts
    routes.ts
  assets/
    images/
    sprites/              # Character layer sprites (V1 static)
```

---

## Key Data Flows

### Pet Creation
`PetCreationForm` -> upload photo -> `media.service.presign` -> PUT R2 -> `media.service.confirm` -> POST `/pets` (with photo key + character_config) -> backend creates pet + enqueues `ai-classification` job -> worker calls Google Vision -> PATCH pet with breed suggestion

### Feed Post
`PostComposer` -> upload photo -> confirm -> POST `/feed/posts` -> backend stores post -> Supabase Realtime pushes to followers

### Lost & Found Alert
POST `/lost-found` -> backend stores record -> enqueues `push-notifications` job -> worker sends Expo push to nearby users
