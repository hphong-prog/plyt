# Plyt mobile action reliability update

- Replaced eager action-map evaluation with lazy per-action callbacks, so a missing helper for one control cannot disable every `data-act` control.
- Added user-visible error feedback for synchronous and asynchronous action failures instead of silently doing nothing.
- Clarified that the app stays locked behind the Personal Space gate until the user opens an existing space or creates a new one on that device.
- Kept current desktop/mobile features, Personal Space, Join Plan, Budget member alignment, and Firebase Firestore integration.
- No Firestore schema or security rules changes are required for this patch.
