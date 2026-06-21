# Wave 1: Local Workspace Evidence

Worker: `019ee3f4-8136-7693-a291-7e4c239a5b8b`

## Key Findings

- The local workspace already converges on a skill-first deterministic execution harness, not a prompt wrapper.
- Runtime boundary is ComfyUI as graph engine, with the harness compiling creator intent into validated workflow graphs and controlling execution through ComfyUI APIs.
- Local docs currently favor template filling plus approved subgraph composition first, with arbitrary graph synthesis/custom-node install deferred.
- The remaining open decision is not architecture; it is which certified pipelines/presets come first.

## Local Evidence Cited By Worker

- `README.md:7-25`, `README.md:99-110`: top-level thesis and ComfyUI control model.
- `research/01-architecture-synthesis.md:21-35`, `research/01-architecture-synthesis.md:192-213`: architecture lock and template/subgraph first decision.
- `evidence/image-generation-findings.md:92-101`: ComfyUI-aware controller decision.
- `.omo/ultraresearch/20260620-154100-image-harness/SYNTHESIS.md:9-13`, `:29`, `:92`, `:140`, `:192-203`: synthesized architecture and remaining gaps.
- `research/08-comfyui-full-control-analysis.md:7-24`, `:123-145`, `:147-181`: staged control model and implementation phases.
- `research/07-computer-vision-control-stack.md:98-109`: practical presets already named.
- `research/09-creator-intent-graph-roadmap.md:67-79`: mode roadmap already named.
- `research/05-build-roadmap.md:152-172`: first implementation milestone.

## Worker EXPAND Markers

LEAD:

- `/Users/um-yunsang/image-harness-2026/research/07-computer-vision-control-stack.md:98-109`
- `/Users/um-yunsang/image-harness-2026/research/09-creator-intent-graph-roadmap.md:67-79`
- `/Users/um-yunsang/image-harness-2026/research/05-build-roadmap.md:152-172`

DEAD END:

- `/Users/um-yunsang/image-harness-2026/.omo/ultraresearch/20260620-154100-image-harness/SYNTHESIS.md:194-197`
- `/Users/um-yunsang/image-harness-2026/.omo/ultraresearch/20260620-161500-pipeline-set-2026/expansion-log.md:13-21`
- `/Users/um-yunsang/image-harness-2026/research/10-recent-paper-watchlist-2026.md:64-64`
