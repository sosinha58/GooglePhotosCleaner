# Data Model

> Part of the PhotoTidy PRD set. Index: `README.md`. Rules for agents: `AGENTS.md`.

## 7. Data model (TypeScript, normative)

```ts
type Source = "google-photos-web" | "takeout" | "local-folder";

interface MediaItem {
  key: string;                 // stable per source
  source: Source;
  mediaType: "photo" | "video";
  thumbUrl?: string;           // extension mode
  fileRef?: FileSystemFileHandle; // takeout mode
  capturedAt?: string;         // ISO-8601
  capturedAtSource?: "exif" | "takeout-json" | "ui" | "upload" | "unknown";
  width?: number;
  height?: number;
  bytes?: number;
  bytesIsEstimate?: boolean;
  sha256?: string;
  phash?: string;              // 16 hex chars
  embedding?: Float32Array;    // stored locally only
  flags: QualityFlag[];
  status: "indexed" | "analysed" | "unanalysable" | "protected" | "removed";
}

interface QualityFlag {
  type: "blurry" | "too_dark" | "too_bright" | "accidental" | "screenshot" | "low_res";
  confidence: "low" | "medium" | "high";
  reason: string;
  value?: number;
}

interface Group {
  id: string;
  type: "exact" | "near" | "burst";
  confidence: "exact" | "high-visual" | "high" | "medium" | "low";
  memberKeys: string[];
  keeperKey: string;
  userKeeperKey?: string;
  reclaimableBytes?: number;
  reclaimableIsEstimate: boolean;
  signalsUsed: string[];
  decision: "pending" | "protected" | "queued" | "applied";
}

interface ActionLogEntry {
  at: string;
  action: "select-for-trash" | "protect" | "unprotect" | "dry-run";
  itemKey: string;
  groupId?: string;
  outcome: "ok" | "skipped" | "failed";
  detail?: string;
}
```

Persistence: IndexedDB stores `items`, `groups`, `decisions`, `actionLog`, `settings`. Schema versioned with migrations.
