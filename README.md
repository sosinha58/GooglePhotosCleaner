# PhotoTidy — PRD Set (v0.1 draft)

Find and safely remove duplicate, near-duplicate and bad photos in Google Photos using a local-first Chrome (MV3) extension, with an optional Google Takeout import mode. Built by AI agents; every document is Markdown.

**Start here:** `AGENTS.md` → `docs/09-safety-invariants.md` → `docs/12-tasks.md`.

## Document index

| File | Contents | Sections |
|---|---|---|
| `AGENTS.md` | Agent rules, reading guide, workflow, conventions, Definition of Done | 0, 15 |
| `OPEN_QUESTIONS.md` | Open questions (agents append here) | 17 |
| `docs/01-product-overview.md` | Problem, solution, goals, non-goals, personas | 1, 2 |
| `docs/02-platform-decisions.md` | Google Photos constraints, decision log D-01..D-07, platform risks | 3 |
| `docs/03-scope-milestones.md` | Milestones M0..M5 and scope | 4 |
| `docs/04a-req-scanning-filters.md` | FR-001..FR-027 scanning, date and pixel filters | 5.1, 5.2 |
| `docs/04b-req-detection.md` | FR-030..FR-109 duplicates, keeper selection, bad photos | 5.3, 5.4, 5.5 |
| `docs/04c-req-review-deletion.md` | FR-060..FR-093 review UI, deletion flow, reporting | 5.6, 5.7, 5.8 |
| `docs/04d-req-takeout-adapters-settings.md` | FR-110..FR-132 Takeout, adapter hooks, settings/privacy | 5.9, 5.10, 5.11 |
| `docs/05-detection-spec.md` | Pipeline, algorithms, confidence levels, keeper score, flag thresholds | 6 |
| `docs/06-data-model.md` | TypeScript data model, persistence | 7 |
| `docs/07-architecture.md` | Components, repo layout, `StorageAdapter`, permissions | 8 |
| `docs/08-nfr.md` | Performance, reliability, privacy, accessibility | 9 |
| `docs/09-safety-invariants.md` | SAF-001..SAF-009 (override everything) | 10 |
| `docs/10-ux-flows-telemetry.md` | Text wireframes, error states, telemetry | 11, 12 |
| `docs/11-testing-acceptance.md` | Test layers, golden dataset, AC-1..AC-10 | 13 |
| `docs/12-tasks.md` | Task breakdown T-001..T-055, release criteria | 14 |
| `docs/13-risks.md` | RK-01..RK-07 | 16 |
| `docs/14-glossary.md` | Terms | 18 |

Section numbers are stable across files (e.g., "section 6.4" always means the keeper score in `docs/05-detection-spec.md`). Cross-references show the file name next to the section number.

## Requirement ID prefixes
`G-` goals · `NG-` non-goals · `C-` platform constraints · `D-` decisions · `R-` platform risks · `FR-` functional · `NFR-` non-functional · `SAF-` safety · `AC-` acceptance · `T-` tasks · `RK-` risks · `OQ-` open questions.

## Key decisions at a glance
- Access Google Photos through a **browser extension in the logged-in web UI** (the official Library API no longer reads a user's whole library).
- All analysis runs **locally**; no photo data leaves the browser.
- The **user performs the final "Move to trash" confirmation**; the product never permanently deletes.
- Takeout import gives full-resolution quality analysis; a `StorageAdapter` interface keeps other services pluggable.

## Pre-launch blockers
Legal review of UI automation (OQ-02), product name (OQ-01), business model (OQ-03), and spike results (S-01..S-03) before building the scanner.
