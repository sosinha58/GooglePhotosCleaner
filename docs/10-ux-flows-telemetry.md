# UX Flows and Telemetry

> Part of the PhotoTidy PRD set. Index: `README.md`. Rules for agents: `AGENTS.md`.

## 11. UX flows (text wireframes)

### 11.1 First run
1. User installs → opens photos.google.com → popup shows **Welcome** (FR-132): what we read, what stays local, what we never do.
2. User clicks **Scan this view**.

### 11.2 Scan
`Popup → Scan this view → progress panel (found / analysed / ETA, Pause, Cancel) → "Open Review"`.

### 11.3 Review
```
┌ Top bar: [Scan scope] [Filters: Date ▾  Pixels ▾  Size ▾]   Selected: 128  Free ≈ 1.9 GB  [Review & move to trash] ┐
├ Tabs: Duplicates (42) | Similar (87) | Individual issues (310) | All scanned (12,450) ┤
│ Group #17  (near, high)  Free ≈ 14 MB                                       │
│ [ KEEP ✓ img A ] [ ☐ img B  "Lower resolution"] [ ☐ img C "Blurrier"]       │
│ [Compare] [Not duplicates] [Protect]                                        │
└──────────────────────────────────────────────────────────────────────────────┘
```

### 11.4 Apply
`Review & move to trash → Summary dialog (count, space, samples, dry-run toggle) → Confirm → extension selects items in Google Photos in batches → user clicks Google's Move to trash → verification report`.

### 11.5 Error states (all MUST have copy)
Selectors missing (FR-016), rate-limited, thumbnail failures, tab closed mid-scan (resume), storage quota exceeded.

---

## 12. Telemetry (optional, opt-in)
Aggregate only: scan size bucket, groups found bucket, apply success/fail counts, error codes, extension version. No identifiers beyond a random installation ID that the user can reset.
