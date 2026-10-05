# Architecture

> Part of the PhotoTidy PRD set. Index: `README.md`. Rules for agents: `AGENTS.md`.

## 8. Architecture

### 8.1 Components
| Component | Responsibility | Tech |
|---|---|---|
| `content-script` | Runs on photos.google.com; DOM reader/selector via `GooglePhotosAdapter` | TS |
| `service-worker` | Messaging, lifecycle, storage broker | MV3 |
| `review-app` | Extension page UI | React + Vite |
| `analysis-workers` | Hashing, embeddings, grouping, scoring | Web Workers + ONNX Runtime Web |
| `core` | Pure detection/scoring libraries | TS, no DOM |
| `adapters/*` | StorageAdapter implementations | TS |

### 8.2 Repository layout (agents MUST follow)
```
/ (repo root)
├─ README.md            # index of the PRD set
├─ AGENTS.md            # rules and workflow for agents
├─ OPEN_QUESTIONS.md
├─ docs/
│  ├─ 01-...14-*.md     # the PRD set (this folder)
│  ├─ decisions/        # ADRs, one MD per decision (ADR-001...)
│  ├─ spikes/           # spike write-ups (S-01...)
│  └─ privacy-policy.md
├─ src/
│  ├─ config/thresholds.ts
│  ├─ core/             # hashing, embeddings, grouping, scoring (pure)
│  ├─ adapters/
│  │  ├─ googlephotos/  # dom/, selectors.ts, health-check.ts
│  │  └─ takeout/
│  ├─ workers/
│  ├─ review-app/
│  ├─ extension/        # manifest, service worker, content script
│  └─ store/            # IndexedDB access
├─ test/
│  ├─ fixtures/golden/  # labelled image sets (docs/11-testing-acceptance.md §13.2)
│  ├─ fake-google-photos/ # static page simulating the DOM
│  └─ e2e/
└─ .github/workflows/
```

### 8.3 Adapter interface (normative)
```ts
interface StorageAdapter {
  id: Source;
  capabilities: {
    listAll: boolean;          // can enumerate whole library
    readOriginals: boolean;
    applyDeletion: "none" | "select-only" | "trash";
  };
  healthCheck(): Promise<{ ok: boolean; problems: string[] }>;
  scan(opts: ScanOptions, onItems: (items: MediaItem[]) => void, signal: AbortSignal): Promise<ScanSummary>;
  getThumbnail(item: MediaItem, maxEdge: number): Promise<ImageBitmap>;
  getOriginal?(item: MediaItem): Promise<Blob>;
  selectForTrash?(keys: string[], opts: { dryRun: boolean }): Promise<ApplyResult>;
}
```

### 8.4 Extension permissions (minimal, MUST)
- `host_permissions`: Google Photos origin and the Google image hosts it uses for thumbnails (list verified during Spike S-01).
- `permissions`: `storage`, `unlimitedStorage`, `scripting`, `activeTab`.
- MUST NOT request: `<all_urls>`, `cookies`, `history`, `webRequest` (blocking), or remote code execution. All code and models bundled locally.
