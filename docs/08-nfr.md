# Non-Functional Requirements

> Part of the PhotoTidy PRD set. Index: `README.md`. Rules for agents: `AGENTS.md`.

## 9. Non-functional requirements

### 9.1 Performance
| ID | Requirement |
|---|---|
| NFR-001 | Thumbnail analysis throughput ≥ 20 items/s on a mid-range laptop (4 cores, no GPU) for hashing; ≥ 8 items/s with embeddings `[TUNABLE]`. |
| NFR-002 | UI remains responsive: main-thread tasks < 50 ms; scroll in Review at ≥ 50 fps with 10,000 rendered groups (virtualised list). |
| NFR-003 | Memory budget: ≤ 1.5 GB tab memory at 50,000 items; embeddings stored as float16 or quantised if needed. |
| NFR-004 | Scan of 10,000 items completes within 15 min on reference hardware (rate limits permitting). |

### 9.2 Reliability
| ID | Requirement |
|---|---|
| NFR-005 | Crashes or reloads MUST not lose scan progress (checkpoint every 500 items). |
| NFR-006 | All destructive-flow code paths MUST have automated tests (see docs/11-testing-acceptance.md §13). |

### 9.3 Privacy and security
| ID | Requirement |
|---|---|
| NFR-010 | **No photo pixels, thumbnails, hashes or embeddings are transmitted off-device.** |
| NFR-011 | No third-party scripts, analytics SDKs or remote fonts. |
| NFR-012 | Telemetry is **opt-in**, aggregate counts only (e.g., scan size bucket, errors), no item identifiers, no URLs. |
| NFR-013 | Content Security Policy forbids `unsafe-eval` and remote script sources. |
| NFR-014 | Clear-data function wipes all local stores (FR-131). |
| NFR-015 | The extension never reads, stores or transmits Google credentials, cookies or tokens. |

### 9.4 Accessibility and compatibility
| ID | Requirement |
|---|---|
| NFR-007 | WCAG 2.1 AA: contrast, focus order, labels, reduced motion. |
| NFR-008 | Supported: current and previous major Chrome and Edge (Chromium) on Windows, macOS, Linux. |
| NFR-009 | Firefox/Safari: out of scope for v1. |
