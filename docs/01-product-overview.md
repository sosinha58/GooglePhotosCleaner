# Product Overview — Problem, Goals, Users

> Part of the PhotoTidy PRD set. Index: `README.md`. Rules for agents: `AGENTS.md`.

## 1. Summary

### 1.1 Problem
Users with large Google Photos libraries run low on storage. The library is full of exact duplicates, near duplicates (bursts, edits, re-saves), screenshots, blurry shots and accidental frames. Google Photos has no built-in duplicate detection, and since 2025 its official Library API no longer lets third-party apps read a user's whole library (it is limited to content the app created). Users must clean up by hand.

### 1.2 Solution
A **Chrome (Manifest V3) browser extension** that works inside the user's logged-in Google Photos web session. It scans the loaded view, analyses photos **locally in the browser**, groups exact duplicates, near duplicates and low-quality photos, recommends which to remove, and lets the user review and move selected items to Google Photos trash. A secondary **Takeout import mode** analyses full-resolution exports for accurate quality scoring.

### 1.3 Goals
| ID | Goal | Measure |
|---|---|---|
| G-01 | Help users reclaim storage safely | Median "reclaimable space" shown ≥ 5% of scanned library size on libraries with ≥ 1,000 items |
| G-02 | Zero accidental data loss | 0 permanent deletions; 0 deletions without explicit user confirmation |
| G-03 | Trustworthy detection | Exact-duplicate precision 100%; near-duplicate precision ≥ 95% on the golden set (docs/11-testing-acceptance.md §13.2) |
| G-04 | Privacy by design | No photo content leaves the browser (docs/08-nfr.md §9.3) |
| G-05 | Filter by date and pixel size | Users can scope scan and results by capture date range and resolution |

### 1.4 Non-goals (v1)
- NG-01: Not a backup, sync or photo-editing product.
- NG-02: No cloud processing of photo content.
- NG-03: No permanent deletion or "empty trash" automation.
- NG-04: No support for services other than Google Photos in v1 (architecture MUST allow adding them, see FR-120).
- NG-05: No face recognition or identity-based grouping.
- NG-06: No mobile apps in v1.
- NG-07: No use of the Google Photos Library API for whole-library access (it is not permitted for non-app-created content).

---

## 2. Users and jobs-to-be-done

| Persona | Description | Job |
|---|---|---|
| **Storage-squeezed Sam** | Hit the 15 GB free limit; thousands of phone photos | "Free up space fast without losing good photos" |
| **Archivist Priya** | 50k+ photos over 10+ years, multiple imports | "Remove duplicate copies from migrations, keep the best version" |
| **Parent Dev** | Many burst shots of kids | "Keep the best shot of each burst, drop the rest" |

JTBD statement: *When my Google Photos storage is nearly full, I want to find and remove duplicate and bad photos safely, so I can keep my best memories and avoid paying for more storage.*
