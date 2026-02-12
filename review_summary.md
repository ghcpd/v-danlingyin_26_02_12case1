Review summary — Event Bus documentation audit

Findings:

- The repository originally had an empty `GUIDE.md` (title + "Coming soon...") and no structured API reference. Public symbols exported from `eventBus.ts` were undocumented.
- Public API surface includes `EventBus` (class), `EventBusOptions` (interface), and `EventHandler` (type). Methods on `EventBus` (subscribe, once, unsubscribe, publish, clear, getSubscriberCount, listEvents) were not described.

Actions taken:

- Created an exact backup of the original `GUIDE.md` as `GUIDE_backup.md`.
- Rewrote `GUIDE.md` to include a clear overview, descriptions of all public APIs and options (including `maxListeners` and `debug`), usage examples with code blocks, and references to `eventBus.ts`.
- Generated `api_reference.json` that lists exported symbols and concise descriptions.

Notes and recommendations:

- Add unit tests for common usage patterns (especially edge cases such as hitting `maxListeners`, unsubscribing non-existent handlers, and the behavior of `once`).
- Consider adding examples showing typed handlers (generic `EventHandler<T>`) and how to use the bus in async flows if needed.
- Keep `GUIDE.md` and `api_reference.json` maintained when the public API changes.

Files added/updated:

- `GUIDE_backup.md` (backup of original)
- `GUIDE.md` (expanded documentation)
- `api_reference.json` (structured API reference)
- `review_summary.md` (this file)

All changes made are documentation-only; source code (`eventBus.ts`) was not modified.