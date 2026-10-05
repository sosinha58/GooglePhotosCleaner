# AGENTS.md — Instructions for Implementing Agents

> Project: **PhotoTidy** (working name) — Google Photos duplicate & bad-photo cleaner (Chrome MV3 extension).
> PRD set version: **v0.1 (draft)**. Index: `README.md`.

## 0. How agents must use this document set

1. The files in this repository's `README.md` index are the **source of truth**. If code and these docs disagree, stop and raise it in `OPEN_QUESTIONS.md`. Do not silently choose.
2. Every requirement has a stable ID (`FR-###`, `NFR-###`, `SAF-###`, `D-##`, `T-###`, `AC-#`, `RK-##`, `OQ-##`). Reference IDs in commits, PR titles, tests and code comments.
3. Keywords **MUST / MUST NOT / SHOULD / MAY** follow RFC 2119.
4. **Do not add scope.** Anything not in a milestone's scope (`docs/03-scope-milestones.md`) is out of scope. Propose additions in `OPEN_QUESTIONS.md`.
5. Numeric thresholds marked `[TUNABLE]` are initial values. Put them in a single config module (`src/config/thresholds.ts`) and never hardcode them elsewhere.
6. Items marked `[SPIKE]` require a short investigation with a written decision record in `docs/decisions/` before implementation.
7. A task is **done** only when it meets the Definition of Done in section 15.2 below.
8. Safety invariants in `docs/09-safety-invariants.md` override every other requirement, including performance and convenience.

## Where to read (load only what the task needs)

| Working on... | Read first |
|---|---|
| Any task | `AGENTS.md`, `docs/09-safety-invariants.md`, the task row in `docs/12-tasks.md` |
| Scanning, date/pixel filters | `docs/04a-req-scanning-filters.md`, `docs/07-architecture.md` |
| Duplicates, keeper, bad photos | `docs/04b-req-detection.md`, `docs/05-detection-spec.md`, `docs/06-data-model.md` |
| Review UI, trash flow, reports | `docs/04c-req-review-deletion.md`, `docs/10-ux-flows-telemetry.md` |
| Takeout, adapters, settings | `docs/04d-req-takeout-adapters-settings.md`, `docs/07-architecture.md` |
| Tests and acceptance | `docs/11-testing-acceptance.md` |
| Performance, privacy, a11y | `docs/08-nfr.md` |
| Why a decision was made | `docs/02-platform-decisions.md`, `docs/13-risks.md` |

## Workflow per task

1. Open the task in `docs/12-tasks.md`; confirm its dependencies are merged.
2. Read the requirement IDs it lists, plus the safety file.
3. Implement only those requirements. Write tests first where practical.
4. If something is ambiguous or missing, add an entry to `OPEN_QUESTIONS.md` and continue with the safest interpretation, noting it in the PR.
5. Open one PR per task using the commit format below.

## 15. Engineering conventions


### 15.1 Coding conventions
- TypeScript `strict: true`; no `any` without a justification comment.
- `src/core` MUST NOT import from `adapters`, `review-app`, DOM or `chrome.*` APIs.
- All thresholds from `src/config/thresholds.ts`.
- Commit format: `type(scope): summary [ID]` e.g., `feat(core): add pHash [FR-031]`.
- Each ADR: Context, Decision, Alternatives, Consequences (one MD file per decision).

### 15.2 Definition of Done (per task)
1. Implements the referenced requirement IDs completely and nothing more.
2. Unit/contract/E2E tests added and passing in CI.
3. Lint and type checks pass.
4. Docs updated (README/ADR/spike as relevant).
5. No new permissions or network calls without a PRD change.
6. Safety tests unchanged or strengthened.
7. PR description lists requirement IDs, test evidence and any deviations.
