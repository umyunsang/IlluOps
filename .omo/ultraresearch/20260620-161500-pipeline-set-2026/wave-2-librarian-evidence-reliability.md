# Wave 2: Evidence Reliability Audit

Worker: `019ee3fb-34b9-7182-b2c5-521bc90d6133`

## Evidence Tiers

### Strong Primary Evidence

- MultiBanana: strong benchmark anchor for multi-reference T2I and editing.
  - https://arxiv.org/abs/2511.22989
  - https://huggingface.co/datasets/kohsei/MultiBanana-Benchmark
- Inter-Edit: strong CVPR 2026 benchmark/source for interactive instruction-based image editing.
  - https://openaccess.thecvf.com/content/CVPR2026/papers/Liu_Inter-Edit_First_Benchmark_for_Interactive_Instruction-Based_Image_Editing_CVPR_2026_paper.pdf
- GenColorBench: strong CVPR 2026 color benchmark anchor.
  - https://openaccess.thecvf.com/content/CVPR2026/papers/Butt_GenColorBench_A_Color_Evaluation_Benchmark_for_Text-to-Image_Generation_CVPR_2026_paper.pdf
- T2I-ReasonBench: useful/strong-ish OpenReview benchmark source, but not yet an independently settled standard.
  - https://openreview.net/forum?id=1hYITxAwdz
- PICABench: strong physical-realism benchmark source for roadmap/scoping.
  - https://arxiv.org/abs/2510.17681
  - https://openreview.net/forum?id=AWxI5xnuZB
- PhyEdit: strong source for physical manipulation/editing being specialized.
  - https://arxiv.org/abs/2604.07230
- Qwen-Image-2.0 / Qwen-Image-Edit: strong model-family evidence, not independent benchmark evidence.
  - https://arxiv.org/abs/2605.10730
  - https://arxiv.org/abs/2508.02324
  - https://huggingface.co/Qwen/Qwen-Image-Edit
- HunyuanImage 3.0: strong model report.
  - https://arxiv.org/abs/2509.23951
  - https://github.com/Tencent-Hunyuan/HunyuanImage-3.0
- Z-Image: strong model report and official model-family evidence.
  - https://arxiv.org/abs/2511.22699
  - https://huggingface.co/Tongyi-MAI/Z-Image
  - https://huggingface.co/Tongyi-MAI/Z-Image-Turbo

### Useful But Immature

- FineEdit: useful but new/self-contained.
  - https://arxiv.org/abs/2604.10954
- ProductConsistency: useful for product/brand consistency but new/self-evaluated.
  - https://arxiv.org/abs/2606.19103
- OCRGenBench / OmniDoc-TokenBench: useful for text-rendering gates but early.
  - https://arxiv.org/html/2507.15085v4
  - https://arxiv.org/abs/2605.13565
- FLUX.2: important model-family/product evidence but not a benchmark standard.
  - https://github.com/black-forest-labs/flux2
  - https://huggingface.co/black-forest-labs/FLUX.2-dev
  - https://huggingface.co/blog/flux-2

### Weak Or Contested For Certification

- Treat vendor/model launch pages as capability signals, not proof of reliability.
- Do not use FLUX.2 as a benchmark anchor; use it as a model-family integration target.
- Do not claim any model report alone proves frontier reliability. Use it to decide supported model routes and then certify our own recipes.
- The earlier "GenColorBench withdrawn" lead conflicts with the CVPR 2026 open-access paper evidence. For this session, GenColorBench should be treated as valid primary evidence unless a stronger withdrawal record is later found.

## Audit Recommendation

Use benchmark papers to define quality gates, but use ComfyUI runtime smoke tests and recipe-level visual checks as the actual certification mechanism.

Benchmarks should inform failure modes:

- Multi-reference/reference composition: MultiBanana.
- Interactive/localized editing: Inter-Edit, FineEdit, ImgEdit-style suites.
- Color/product/text: GenColorBench, ProductConsistency, OCRGenBench.
- Physical realism: PICABench, PhyEdit, roadmap-only gates.

They should not be overused as proof that a local ComfyUI pipeline is production reliable.
