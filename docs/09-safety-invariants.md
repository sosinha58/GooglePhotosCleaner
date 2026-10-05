# Safety Invariants (override everything)

> Part of the PhotoTidy PRD set. Index: `README.md`. Rules for agents: `AGENTS.md`.

## 10. Safety invariants (override everything)

| ID | Invariant |
|---|---|
| SAF-001 | No item is moved to trash without a user-confirmed summary dialog in the same session (FR-080) and the final Google Photos confirmation by the user (FR-081). |
| SAF-002 | The product MUST NOT permanently delete, empty trash, or trigger any irreversible Google Photos action. |
| SAF-003 | In any group, at least one member (the keeper) MUST remain. Enforced in core, UI and apply layers (three independent checks). |
| SAF-004 | Items with confidence below `high-visual` MUST default to unselected. |
| SAF-005 | Bad-photo flags MUST default to unselected. |
| SAF-006 | Items marked Protected are never selectable for removal. |
| SAF-007 | If the selector health check fails or DOM state is unexpected mid-apply, the apply operation MUST stop and report; it MUST NOT guess. |
| SAF-008 | A dry-run MUST be available before the first real apply and recommended on first use (FR-086). |
| SAF-009 | Hard cap per apply session (default 500 items `[TUNABLE]`), user may raise it with an explicit confirmation. |
