# PetApp — AI Assistance Rules

Rules for how Claude Code should assist on this project. These are standing instructions that apply to every session.

---

## Governance

1. **Read before writing.** Always read existing files before modifying them. Never assume file contents.
2. **Update session-state.md** after completing any meaningful unit of work (module, feature, migration).
3. **Update codemapcc.md** when new files or modules are created.
4. **Add to lessons.md** whenever a non-obvious problem is solved or a gotcha is encountered.
5. **Do not modify plan.md or features.md** without explicit user instruction. These are decision documents.

---

## Code Standards

6. **No over-engineering.** Only build what is needed for the current phase. Do not add features, abstractions, or error handling beyond the scope of the current task.
7. **No premature abstraction.** Three similar lines of code is better than a utility function used once.
8. **No backwards-compat shims.** If something is removed, delete it cleanly.
9. **No docstrings or comments** unless the logic is genuinely non-obvious.
10. **TypeScript strict mode.** No `any` unless absolutely unavoidable, and document why.

---

## Architecture

11. **Never route file bytes through the NestJS server.** All media goes via presigned R2 URLs.
12. **Never do heavy work synchronously in a request handler.** Breed classification, push notifications, and XP calculation always go via BullMQ.
13. **Soft deletes everywhere.** All user-owned tables have `deleted_at`. Always filter it in queries.
14. **Index early.** Add indexes for all FK columns and common query patterns at migration time, not later.
15. **Auth guard on every protected route.** No route is unprotected by accident. Unauthenticated public endpoints are explicitly marked.

---

## Security

16. **No SQL injection.** Always use Drizzle parameterised queries. Never interpolate user input into query strings.
17. **No XSS.** Sanitise any user-generated content before rendering.
18. **UUIDs only in URLs.** Never expose sequential integer IDs in public-facing endpoints.
19. **Short-lived presigned URLs.** R2 presign URLs expire in 15 minutes.

---

## Workflow

20. **Confirm before destructive actions.** Deleting files, dropping tables, force-pushing — always ask first.
21. **One module at a time.** Complete and test a module before moving to the next.
22. **Check features.md** at the start of each session to confirm scope before writing code.
23. **Do not invent product decisions.** If a product or design question is unclear, ask the user rather than guessing.

---

## V1 Scope Guard

The following are explicitly OUT of V1. Do not build them unless the user explicitly changes scope:
- Rive character animations
- XP / activity logging
- Forum threads
- Stories
- User reviews on listings
- Multi-country support
- Monetisation
