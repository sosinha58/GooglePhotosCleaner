# Detection and Scoring Specification

> Part of the PhotoTidy PRD set. Index: `README.md`. Rules for agents: `AGENTS.md`.

## 6. Detection and scoring specification

### 6.1 Pipeline
```
Source (adapter) → Item metadata → Thumbnail/pixel fetch → Normalise
   → [Exact hash] → [pHash bucket] → [Embedding + ANN] → Group builder
   → Keeper scorer → Quality flags → Result store → UI
```

### 6.2 Algorithms (initial choices; replaceable behind interfaces)

| Stage | Method | Notes |
|---|---|---|
| Normalise | Decode → resize to 256 px max edge (extension) / 512 px (Takeout) → sRGB | Apply EXIF orientation |
| Exact (Takeout) | SHA-256 of bytes | Identical bytes = `exact` |
| Perceptual hash | pHash 64-bit (DCT of 32×32 grayscale) | Hamming distance for bucketing; default candidate if distance ≤ 10 `[TUNABLE]` |
| Embedding | MobileNetV3-class or DINOv2-small/CLIP-class, ONNX Runtime Web (WASM, WebGPU if available) | Cosine similarity |
| ANN search | HNSW in-browser (or brute-force for < 5,000 items) | Limit k=20 neighbours per item |
| Grouping | Union-find over edges where `sim ≥ threshold`, with size cap and re-clustering | FR-033 |
| Burst | Time delta ≤ 3 s + sim ≥ 0.85 `[TUNABLE]` | FR-035 |

Similarity slider maps to cosine threshold: 95% ≈ 0.95 `[TUNABLE; calibrate on golden set]`.

### 6.3 Confidence levels
| Level | Rule |
|---|---|
| `exact` | Identical SHA-256 (Takeout only) |
| `high-visual` | pHash distance ≤ 2 **and** cosine ≥ 0.98 **and** identical dimensions |
| `high` | Cosine ≥ 0.96 and pHash distance ≤ 6 |
| `medium` | Cosine ≥ 0.92 |
| `low` | Anything below, shown only if user lowers the slider |

Only `exact` and `high-visual` groups may be **pre-selected**. `high` and below default to unselected, but the keeper recommendation still shows.

### 6.4 Keeper score (higher is better)
```
score = 0.35 * norm(megapixels)
      + 0.25 * norm(sharpness)          // Takeout; in extension mode weight redistributed
      + 0.15 * norm(file_size)          // proxy for quality when same dims
      + 0.10 * is_original_flag         // not a re-export; Takeout metadata heuristics
      + 0.10 * norm(exposure_quality)
      + 0.05 * earliest_capture_bonus   // tie-breaker: prefer earliest
```
- All weights `[TUNABLE]` in `thresholds.ts`.
- Ties broken by earliest capture time, then lexicographic item key (deterministic).
- Where a signal is unavailable, drop it and renormalise remaining weights; record `signalsUsed` on the group.

### 6.5 Bad-photo flags (defaults, all `[TUNABLE]`)
| Flag | Rule | Confidence in extension mode |
|---|---|---|
| `blurry` | Laplacian variance < 100 on 512 px grayscale | low (estimate) |
| `too_dark` | ≥ 90% pixels luminance < 30 | medium |
| `too_bright` | ≥ 90% pixels luminance > 225 | medium |
| `accidental` | Mean luminance < 10 or global colour stddev < 5 | medium |
| `screenshot` | Aspect ratio in device list and no camera EXIF (Takeout) | low in extension mode |
| `low_res` | < 0.5 MP | depends on dimension availability |
