# Requirements — Duplicate, Keeper and Bad-Photo Detection (FR-030..FR-109)

> Part of the PhotoTidy PRD set. Index: `README.md`. Rules for agents: `AGENTS.md`.

### 5.3 Duplicate detection (M2)

| ID | Requirement | Priority |
|---|---|---|
| FR-030 | **Exact duplicates:** items with identical content hash. In Takeout mode use SHA-256 of file bytes. In extension mode, where bytes are unavailable, use a high-threshold perceptual match plus identical dimensions and mark confidence `high-visual` (not `exact`). | MUST |
| FR-031 | **Near duplicates:** compute a 64-bit perceptual hash (pHash) per thumbnail for first-pass bucketing, then confirm with image embeddings (see docs/05-detection-spec.md §6.2). | MUST |
| FR-032 | Near-duplicate similarity MUST be user-adjustable (slider 80–100%, default 95% `[TUNABLE]`). | MUST |
| FR-033 | Groups MUST be disjoint (an item belongs to at most one group). Use connected components with a max group size cap (default 50 `[TUNABLE]`) to avoid chain-drift; oversize groups are split by re-clustering at a stricter threshold. | MUST |
| FR-034 | Each group MUST have: `groupId`, `type` (`exact` \| `near` \| `burst`), `confidence`, member list, recommended keeper, and reclaimable bytes (if sizes known). | MUST |
| FR-035 | **Burst/sequence detection:** items captured within 3 seconds `[TUNABLE]` and visually similar MUST be tagged `burst` when timestamp data allows. | SHOULD |
| FR-036 | Crops, resizes, re-compressions and minor edits (brightness/contrast) of the same photo SHOULD be grouped as near duplicates. Mirrored and rotated versions MAY be grouped. | SHOULD |
| FR-037 | Detection MUST run in Web Workers and MUST NOT block the UI thread. | MUST |

---

### 5.4 Keeper selection (M2)

| ID | Requirement | Priority |
|---|---|---|
| FR-040 | For every group the system MUST recommend exactly **one keeper**; the others are "suggested for removal". | MUST |
| FR-041 | Keeper score = weighted sum per docs/05-detection-spec.md §6.4. Weights live in `thresholds.ts`. | MUST |
| FR-042 | The UI MUST show **why** each member is suggested for removal (e.g., "Lower resolution", "Blurrier", "Smaller file"). | MUST |
| FR-043 | The user MUST be able to change the keeper in any group with one click. | MUST |
| FR-044 | The system MUST NOT recommend removing all members of a group (see SAF-003). | MUST |
| FR-045 | Items in user-marked "protected" state (FR-071) MUST never be suggested for removal. | MUST |

---

### 5.5 Bad-photo detection (M4)

| ID | Requirement | Priority |
|---|---|---|
| FR-100 | **Blur:** variance-of-Laplacian on a normalised grayscale image; below threshold → `blurry`. | MUST (Takeout), SHOULD (extension: low-confidence flag only) |
| FR-101 | **Exposure:** histogram analysis for under-exposed (≥ 90% pixels below luminance 30 `[TUNABLE]`) and over-exposed (≥ 90% above 225) → `too_dark` / `too_bright`. | MUST |
| FR-102 | **Obstructed/accidental:** near-black frames, uniform colour frames (low variance), finger-over-lens heuristic → `accidental`. | SHOULD |
| FR-103 | **Screenshots/low-value:** detect by aspect ratio matching common device screens, absence of camera EXIF (Takeout), and UI-like edge/colour statistics → `screenshot`. | SHOULD |
| FR-104 | **Low resolution:** below a user-set megapixel threshold (default 0.5 MP `[TUNABLE]`) → `low_res`. | MUST |
| FR-105 | **Learned quality score (aesthetic/technical):** optional ONNX model (e.g., NIMA/MUSIQ class) producing 1–10; used only to rank within groups and as a secondary filter, never as sole reason for removal. | MAY (v1.1) |
| FR-106 | **Closed eyes / eyes-shut:** MAY be added later; requires face landmark model; out of v1 scope. | MAY |
| FR-107 | Every bad-photo flag MUST include a **confidence** (`low`/`medium`/`high`) and a human-readable reason. | MUST |
| FR-108 | Bad-photo candidates MUST be shown in a separate **"Individual issues"** tab and MUST default to **unselected**. | MUST |
| FR-109 | In extension mode (thumbnails only) blur results MUST be labelled "estimate" and MUST NOT be pre-selected for removal. | MUST |
