# Platform Constraints and Decision Log

> Part of the PhotoTidy PRD set. Index: `README.md`. Rules for agents: `AGENTS.md`.

## 3. Platform facts and decisions

### 3.1 Constraints (verified from Google documentation)
- C-01: After 31 March 2025 the Google Photos Library API can only list, search and retrieve media items and albums **created by the calling app**.
- C-02: The Google Photos Picker API lets a user select items for an app, but does not offer library-wide scanning or deletion.
- C-03: Deletion of user items is not available via official APIs for non-app-created content.

### 3.2 Decision log
| ID | Decision | Rationale | Status |
|---|---|---|---|
| D-01 | Primary access method: **browser extension in the logged-in Google Photos web UI**, reading loaded media tiles and thumbnails | Only practical way to cover the whole library after C-01 | Accepted |
| D-02 | Final deletion action is performed **by the user** via Google Photos' own "Move to trash" confirmation, or via an extension-driven selection that the user confirms | Safety (SAF-001) and reduces terms-of-service exposure | Accepted |
| D-03 | All analysis runs **locally** (Web Workers + WASM/ONNX) | Privacy (G-04) | Accepted |
| D-04 | Do **not** depend on undocumented Google endpoints in v1 | Fragility and policy risk | Accepted `[SPIKE S-01 to confirm feasibility]` |
| D-05 | Secondary mode: **Google Takeout import** (user-supplied ZIP/folder, processed locally) | Full-resolution data enables accurate blur, exposure and pixel checks | Accepted (Milestone M4) |
| D-06 | Storage access sits behind a `StorageAdapter` interface | Allows Drive, Dropbox, OneDrive, local folders later | Accepted |
| D-07 | Language/stack: TypeScript, Vite, React, Web Workers, ONNX Runtime Web | Mainstream, agent-friendly | Accepted |

### 3.3 Known risks of D-01 (accepted)
- R-01: Google may change the web UI/DOM at any time. Mitigation: isolate all DOM access in one adapter (`src/adapters/googlephotos/dom/`), add contract tests and a "selector health check" (FR-016).
- R-02: Automating a consumer web interface may conflict with Google's terms. Mitigation: legal review before public launch (OQ-02), keep user-in-the-loop for destructive actions, minimal permissions, no credential handling.
- R-03: Chrome Web Store review. Mitigation: minimal host permissions, clear privacy policy, no remote code.
