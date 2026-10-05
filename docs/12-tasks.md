# Milestone Task Breakdown

> Part of the PhotoTidy PRD set. Index: `README.md`. Rules for agents: `AGENTS.md`.

## 14. Milestone task breakdown (for agent planning)

Dependencies shown as `←`. Each task = one PR.

### M0 — Spikes & scaffolding
| ID | Task | Output |
|---|---|---|
| T-001 | Repo scaffolding (Vite, TS, lint, Vitest, Playwright, CI) | Green CI |
| T-002 `[SPIKE S-01]` | Investigate reading loaded media tiles and thumbnail URLs from the Google Photos DOM; list required host permissions; document selectors, scroll behaviour, virtualisation | `docs/spikes/S-01.md` + ADR-001 |
| T-003 `[SPIKE S-02]` | Evaluate embedding models (size, speed, accuracy) on golden set in browser (WASM vs WebGPU) | `docs/spikes/S-02.md` + ADR-002 |
| T-004 `[SPIKE S-03]` | Test multi-select handoff feasibility and verification after trash | `docs/spikes/S-03.md` + ADR-003 |
| T-005 | Build fake Google Photos page + fixtures generator | `test/fake-google-photos` |
| T-006 | Golden dataset v1 + `labels.json` | `test/fixtures/golden/` ← T-001 |

### M1 — Scan & index
| ID | Task | ← |
|---|---|---|
| T-010 | Manifest, service worker, popup shell | T-001 |
| T-011 | `GooglePhotosAdapter.scan` + health check (FR-003..FR-016) | T-002, T-005 |
| T-012 | IndexedDB store + checkpointing (FR-008, NFR-005) | T-001 |
| T-013 | Metadata index + date/pixel/size filters (FR-021..027) | T-012 |
| T-014 | Settings + first-run + clear data (FR-130..132) | T-010 |

### M2 — Duplicate detection
| ID | Task | ← |
|---|---|---|
| T-020 | Normalise + pHash in `core` (unit tests) | T-006 |
| T-021 | Embedding worker (ONNX) per ADR-002 | T-003 |
| T-022 | ANN + grouping + size cap + confidence (FR-030..034) | T-020, T-021 |
| T-023 | Burst detection (FR-035) | T-022 |
| T-024 | Keeper scorer + reasons (FR-040..045) | T-022 |
| T-025 | Benchmarks vs golden set; calibrate thresholds | T-022 |

### M3 — Review & trash
| ID | Task | ← |
|---|---|---|
| T-030 | Review app shell, tabs, virtualised list (FR-060..066) | T-013, T-022 |
| T-031 | Compare view (FR-062) | T-030 |
| T-032 | Protect / not-duplicate decisions (FR-071..072) | T-030 |
| T-033 | Summary dialog + dry-run (FR-080, FR-086) | T-030 |
| T-034 | `selectForTrash` batching + verification (FR-081..083) | T-004, T-011 |
| T-035 | Guided manual fallback (FR-082) | T-034 |
| T-036 | Action log + exports (FR-090, FR-093) | T-033 |
| T-037 | Safety test suite SAF-001..009 | T-034 |

### M4 — Quality + Takeout
| ID | Task | ← |
|---|---|---|
| T-040 | Blur/exposure/low-res/accidental/screenshot detectors (FR-100..104, 107..109) | T-020 |
| T-041 | "Individual issues" tab (FR-108) | T-030, T-040 |
| T-042 | Takeout adapter (folder + ZIP, sidecar JSON) (FR-110..112, 114) | T-005 |
| T-043 | Takeout removal list/export (FR-113) | T-042 |
| T-044 | Takeout ↔ live view matching (FR-115) | T-042, T-011 |
| T-045 | Extract `StorageAdapter` interface + contract tests (FR-120..125) | T-011, T-042 |

### M5 — Hardening & release
| ID | Task | ← |
|---|---|---|
| T-050 | Performance benchmarks + fixes (NFR-001..004) | M2–M4 |
| T-051 | Accessibility audit + fixes (NFR-007) | T-030 |
| T-052 | Privacy policy, store listing text/screenshots | M3 |
| T-053 | Network-audit test (AC-8) | M3 |
| T-054 | Legal review checkpoint (OQ-02) | M3 |
| T-055 | Release packaging + versioning | all |

### 14.1 Release criteria (v1.0)
All MUST requirements implemented; SAF suite and golden-set targets pass; AC-1..AC-10 pass; legal sign-off recorded; privacy policy published.
