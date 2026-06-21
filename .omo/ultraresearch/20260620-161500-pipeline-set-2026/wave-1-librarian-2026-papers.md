# Wave 1: 2026 Image Generation And Editing Papers

Worker: `019ee3f4-5b20-7cb1-993b-4102ff26349a`

## Key Findings

- 2025-2026 work shifts from generic T2I to controllability under constraints: instruction editing, localized editing, multi-reference conditioning, consistency, OCR/text rendering, high-resolution editing, and physical-effect preservation.
- The worker recommended eight candidate certified pipelines:
  1. prompt-only T2I with spatial/compositional gates
  2. instruction editing
  3. localized/region-guided edit
  4. reference-conditioned/multi-reference generation
  5. character/subject consistency
  6. product/brand fidelity
  7. text rendering/long-form text
  8. high-resolution generation/editing plus physical realism/effect-aware edit gates
- Several sources are strong enough for direction, but some are preprint/submission-stage and should not be overclaimed as mature implementation anchors.

## Sources Reported By Worker

- CoDi: https://openreview.net/forum?id=8pDcEIvmcP
- RePlan: https://openreview.net/forum?id=3SqfjeV0d3
- DIM: https://github.com/showlab/DIM
- EditReward: https://github.com/TIGER-AI-Lab/EditReward
- MACRO: https://arxiv.org/abs/2603.25319
- MultiRef: https://arxiv.org/abs/2508.06905
- STRICT: https://arxiv.org/html/2505.18985v1
- FineEdit: https://arxiv.org/html/2604.10954v1
- GeoEdit: https://github.com/PRIS-CV/GeoEdit
- Qwen-Image: https://github.com/QwenLM/Qwen-Image
- TextAtlas5M: https://openreview.net/forum?id=8yJyEKHkB8
- SpatialGenEval: https://openreview.net/forum?id=ddFN3lWpIr
- RectifiedHR: https://openreview.net/forum?id=v6ppFkj1e9
- TEXTS-Diff: https://arxiv.org/html/2601.17340v1
- EditCrafter: https://github.com/EditCrafter/EditCrafter
- PICABench: https://arxiv.org/abs/2510.17681
- PhyEdit: https://arxiv.org/html/2604.07230v2
- ProductConsistency: https://arxiv.org/abs/2606.19103
- ERNIE-Image: https://arxiv.org/html/2605.25347v1
- FreeText: https://arxiv.org/html/2601.00535v1
- Learning an Image Editing Model without Image Editing Pairs: https://openreview.net/forum?id=OHqZ61ZqNO

## Worker EXPAND Markers

LEAD:

- Qwen-Image
- DIM
- EditReward
- GeoEdit
- CoDi
- RePlan
- MACRO
- TextAtlas5M
- SpatialGenEval
- RectifiedHR

DEAD END:

- GenColorBench withdrawn.
- Preprint-only leads need follow-up before hard certification.
