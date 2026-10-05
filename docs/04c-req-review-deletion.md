# Requirements — Review UI, Deletion Flow, Reporting (FR-060..FR-093)

> Part of the PhotoTidy PRD set. Index: `README.md`. Rules for agents: `AGENTS.md`.

### 5.6 Review UI (M3)

| ID | Requirement | Priority |
|---|---|---|
| FR-060 | A full-page **Review** app opens from the popup (extension page) with tabs: **Duplicates**, **Similar**, **Individual issues**, **All scanned**. | MUST |
| FR-061 | Each group is shown as a row/card with side-by-side thumbnails, keeper highlighted, metadata (date, pixels, size), and reason chips. | MUST |
| FR-062 | A group can be opened in a **compare view** with synchronized zoom and metadata diff. Zoom beyond thumbnail resolution MUST show a clear "low-res preview" notice. | SHOULD |
| FR-063 | Top bar MUST show: items selected, **space to be freed**, and a primary button **Review & move to trash**. | MUST |
| FR-064 | "Select all suggestions" and "Clear selection" MUST be available per tab and per group. | MUST |
| FR-065 | The UI MUST support keyboard navigation (arrow keys between groups, `K` set keeper, `Space` toggle remove). | SHOULD |
| FR-066 | Sorting: by reclaimable space (default), date, group size, confidence. | MUST |
| FR-067 | The UI MUST be accessible (WCAG 2.1 AA, NFR-007). | MUST |
| FR-071 | Users MUST be able to mark an item or whole group as **Protected / Not a duplicate**; this is remembered locally and respected in future scans (keyed by item key + visual hash). | MUST |
| FR-072 | A **Saved decisions** view lets the user undo "protected" and "not duplicate" markings. | SHOULD |

---

### 5.7 Deletion flow (M3)

| ID | Requirement | Priority |
|---|---|---|
| FR-080 | The user MUST confirm in a **summary dialog** listing count, space to free, and a sample of thumbnails before any selection is applied. | MUST |
| FR-081 | The extension MUST apply the selection in the Google Photos UI (multi-select) in batches (default 100 items `[TUNABLE]`) and then **hand off** to Google Photos' own "Move to trash" confirmation. The user performs the final click. | MUST (D-02) |
| FR-082 | If automatic selection is unreliable (DOM drift), the extension MUST fall back to **guided manual mode**: open each group's items in sequence with deep links and instructions. | SHOULD |
| FR-083 | After a batch, the extension MUST verify via re-scan that the targeted items are no longer in the view, and report successes/failures. | SHOULD |
| FR-084 | The extension MUST NOT click "Delete permanently", "Empty trash" or any equivalent control. | MUST (SAF-002) |
| FR-085 | The extension MUST show a reminder that Google Photos trash retains items for a limited time and that space is only freed after trash is emptied/expired; link to Google's help page. | MUST |
| FR-086 | A **dry-run mode** MUST exist that performs everything except the final selection/apply, and logs what would have happened. | MUST |
| FR-090 | A local **Action log** (item key, group, action, timestamp, outcome) MUST be kept and exportable as JSON/CSV. | MUST |

---

### 5.8 Reporting (M3)

| ID | Requirement | Priority |
|---|---|---|
| FR-091 | Show **reclaimable space** per group, per tab and total. In extension mode sizes may be estimates (from megapixels × compression factor `[TUNABLE]`); label estimates clearly. | MUST |
| FR-092 | After a clean-up show a summary: items moved to trash, estimated space freed, items skipped, errors. | MUST |
| FR-093 | Provide a one-click **export report** (Markdown and CSV) of groups, decisions and results. | SHOULD |
