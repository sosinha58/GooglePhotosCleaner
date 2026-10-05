# Requirements — Takeout Import, Adapter Hooks, Settings (FR-110..FR-132)

> Part of the PhotoTidy PRD set. Index: `README.md`. Rules for agents: `AGENTS.md`.

### 5.9 Takeout import (M4)

| ID | Requirement | Priority |
|---|---|---|
| FR-110 | The user can open a Takeout **folder** (File System Access API) or ZIP; processing is local. | MUST |
| FR-111 | Parse Google's per-photo JSON sidecar metadata and merge with EXIF: capture time, dimensions, title, albums where present. | MUST |
| FR-112 | Compute SHA-256, pHash, embeddings and quality metrics on full-resolution images (downscaled in memory). | MUST |
| FR-113 | Takeout results MUST be exportable as a **removal list** (CSV/MD) of file names and capture dates, and, where possible, as a ready-to-use **search/selection guide** for performing deletion in Google Photos (because Takeout cannot delete in the cloud). | MUST |
| FR-114 | Takeout mode MUST process ≥ 100 GB without loading all files into memory (streaming, bounded concurrency). | MUST |
| FR-115 | Matching Takeout items to items visible in the live Google Photos view (to enable extension-assisted selection) SHOULD use capture time + dimensions + visual hash. | SHOULD |

---

### 5.10 Architecture hooks (M4)

| ID | Requirement | Priority |
|---|---|---|
| FR-120 | All storage access MUST go through the `StorageAdapter` interface (docs/07-architecture.md §8.3). Google Photos web-UI access and Takeout are the first two adapters. | MUST |
| FR-121 | Detection and scoring modules MUST be pure functions with no dependency on any adapter or DOM. | MUST |
| FR-125 | Adding a new adapter MUST NOT require changes in detection, scoring or review UI code. | MUST |

---

### 5.11 Settings and privacy (M1/M3)

| ID | Requirement | Priority |
|---|---|---|
| FR-130 | Settings: similarity threshold, thresholds for blur/exposure/low-res, batch size, concurrency, telemetry opt-in, clear all local data. | MUST |
| FR-131 | **Clear all local data** button MUST delete IndexedDB, caches, and settings. | MUST |
| FR-132 | First-run screen explains in plain language what is read, that nothing leaves the browser, and what the extension will never do (permanent delete). | MUST |
