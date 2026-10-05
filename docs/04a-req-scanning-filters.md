# Requirements — Scanning, Date and Pixel Filters (FR-001..FR-027)

> Part of the PhotoTidy PRD set. Index: `README.md`. Rules for agents: `AGENTS.md`.

### 5.1 Connection and scanning (M1)

| ID | Requirement | Priority |
|---|---|---|
| FR-001 | The extension MUST activate only on `https://photos.google.com/*` and show a toolbar popup with **Open PhotoTidy**. | MUST |
| FR-002 | The user MUST start a scan explicitly ("Scan this view"). No scan starts automatically. | MUST |
| FR-003 | A scan MUST progressively scroll the current view, collecting media tiles without opening or modifying any item. | MUST |
| FR-004 | The scan MUST show live progress (items found, items analysed, elapsed time) and MUST be pausable, resumable and cancellable. | MUST |
| FR-005 | For each item the scanner MUST capture: stable item key (from DOM/URL), thumbnail URL, media type (photo/video), displayed date if available, and any dimensions/size available without opening the item. | MUST |
| FR-006 | The scanner SHOULD fetch only **small thumbnails** (target max edge 64–256 px `[TUNABLE]`) and MUST NOT download originals in the extension mode. | MUST |
| FR-007 | The scanner MUST rate-limit requests (default max 4 concurrent, `[TUNABLE]`) and back off on errors (exponential, max 5 retries). | MUST |
| FR-008 | Scan results MUST persist locally (IndexedDB) so a closed tab can resume. | SHOULD |
| FR-009 | The user MUST be able to scope a scan to the currently open view (e.g., an album, a date range search, "Photos") and the UI MUST tell them what scope was scanned. | MUST |
| FR-016 | A **selector health check** MUST run at scan start; if required DOM landmarks are missing, show "Google Photos changed; scanning unavailable" and offer Takeout mode. | MUST |
| FR-017 | Videos MUST be indexed but excluded from duplicate/quality analysis in v1 (listed as "not analysed"). | MUST |
| FR-018 | The extension MUST handle large libraries (≥ 50,000 items) without exceeding the memory budget in NFR-003. | MUST |
| FR-019 | Items that fail to load a thumbnail MUST be marked `unanalysable` and excluded from groups. | MUST |
| FR-020 | The extension MUST work when Google Photos is in light or dark theme and in English UI; other locales are SHOULD (OQ-05). | SHOULD |

---

### 5.2 Filters by date and pixels (M1)

| ID | Requirement | Priority |
|---|---|---|
| FR-021 | The results view MUST support filtering by **capture date** range (from/to) and by presets (Last 30 days, This year, Before 2020, custom). | MUST |
| FR-022 | The results view MUST support filtering by **pixel dimensions**: min/max width, min/max height, and total megapixels (e.g., `< 2 MP`). | MUST |
| FR-023 | The results view SHOULD support filtering by file size when available. | SHOULD |
| FR-024 | Date source priority: EXIF `DateTimeOriginal` (Takeout mode) > date shown by Google Photos > upload date. The UI MUST display which source was used. | MUST |
| FR-025 | Where pixel dimensions are unavailable in extension mode, the item MUST show "unknown" and be excluded from pixel filters by default, with a toggle to include. | MUST |
| FR-026 | Filters MUST be combinable (AND) and the result count and reclaimable space MUST update live. | MUST |
| FR-027 | Filters MUST apply to groups: a group appears if **any** member matches, with non-matching members visibly dimmed `[confirm in OQ-06]`. | SHOULD |
