# Risks and Mitigations

> Part of the PhotoTidy PRD set. Index: `README.md`. Rules for agents: `AGENTS.md`.

## 16. Risks and mitigations
| ID | Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|---|
| RK-01 | Google DOM changes break scanning | High | High | Isolated adapter, health check, fast release process, Takeout fallback |
| RK-02 | Terms-of-service / policy objection | Medium | High | Legal review, user-in-the-loop, minimal permissions, no credentials |
| RK-03 | Chrome Web Store rejection | Medium | High | Minimal permissions, clear disclosures, privacy policy, no remote code |
| RK-04 | False positives delete wanted photos | Low | Very high | Conservative defaults, SAF invariants, trash only, user confirmation |
| RK-05 | Thumbnail-only analysis too weak for quality scoring | High | Medium | Label estimates, Takeout mode, no pre-selection of weak signals |
| RK-06 | Performance on huge libraries | Medium | Medium | Workers, virtualisation, checkpointing, quantised embeddings |
| RK-07 | Existing competitors | High | Medium | Differentiate on safety, quality flags, explainability, reporting |
