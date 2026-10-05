# Testing and Acceptance Criteria

> Part of the PhotoTidy PRD set. Index: `README.md`. Rules for agents: `AGENTS.md`.

## 13. Testing and acceptance

### 13.1 Test layers
| Layer | Tooling | Required |
|---|---|---|
| Unit (core) | Vitest | ≥ 90% line coverage on `src/core` |
| Contract (adapter) | Vitest + fake page | All `StorageAdapter` methods |
| E2E | Playwright with Chromium + extension loaded against `test/fake-google-photos` | Scan → review → dry-run → apply-select flows |
| Performance | Scripted benchmark on synthetic 10k/50k item sets | NFR-001..004 |
| Safety | Dedicated suite for SAF-001..009 | MUST pass in CI |

### 13.2 Golden dataset (agents MUST create and maintain)
`test/fixtures/golden/` with labelled groups (`labels.json`):
- Exact copies; resized (50%, 25%); re-compressed (JPEG q 60/80); cropped (5–10%); brightness ±15%; rotated 90°; burst sequences; unrelated look-alikes (same scene, different moment); screenshots; blurry/dark/overexposed; tiny images.
- Use only licensed/synthetic or generated images; no personal photos.
- Targets: **exact precision 100%**, **near-duplicate precision ≥ 95%**, **near-duplicate recall ≥ 85%**, **bad-photo precision ≥ 90% (Takeout mode)** `[TUNABLE]`.

### 13.3 Acceptance criteria (Given/When/Then)

**AC-1 (FR-003, FR-004)** Given a Google Photos view with 500 items, when the user clicks Scan, then all 500 items are indexed, progress updates at least once per 2 s, and no item is opened or modified.

**AC-2 (FR-022)** Given indexed items with known dimensions, when the user sets "< 2 MP", then only items under 2 MP are listed and the total updates within 200 ms.

**AC-3 (FR-021, FR-024)** Given items with mixed date sources, when filtering Jan–Mar 2020, then membership matches the displayed date source and the source label is visible.

**AC-4 (FR-044, SAF-003)** Given any group, when the user selects every member for removal, then the UI blocks the action and explains that one item must be kept.

**AC-5 (SAF-004, SAF-005)** Given groups with confidence `medium` and bad-photo flags, when the user taps "Select all suggestions", then those items remain unselected unless the user changes the threshold explicitly.

**AC-6 (FR-084, SAF-002)** Given any apply run, then the automated test spy asserts that no click event targets "Delete permanently" or "Empty trash".

**AC-7 (FR-016, SAF-007)** Given the fake page with a broken selector, when starting a scan, then the extension shows the "Google Photos changed" message and offers Takeout mode; no partial apply occurs.

**AC-8 (NFR-010)** Given a network recorder, during a full scan and analysis, then no request carries image data or embeddings to any non-Google host.

**AC-9 (FR-086)** Given dry-run mode, when the user confirms, then the action log shows `dry-run` entries and no selection occurs in the page.

**AC-10 (FR-113)** Given a Takeout folder, when analysis finishes, then an exportable removal list is produced with file names, capture dates, group IDs and reasons.
