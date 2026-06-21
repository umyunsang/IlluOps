# Recent Paper Watchlist 2026

Date: 2026-06-20

This list focuses on papers and benchmark directions that matter for an Agent Skill-compatible image generation execution harness.

## Image Editing And Control

| Paper or benchmark | Date/status | Main idea | Harness relevance |
|---|---|---|---|
| [FireRed-Image-Edit-1.0](https://arxiv.org/abs/2602.13344) | 2026, revised Jun 14 | DiT instruction editing with pretraining, SFT, RL, DPO, OCR rewards, and REDEdit-Bench. | Strong reference for edit-specific training data, reward design, and edit benchmarks. |
| [VIBE](https://arxiv.org/abs/2601.02242) | 2026 | Low-cost source-consistent editing: smaller VLM guides a diffusion model. | Useful for local or moderate hardware editing route design. |
| [FineEdit](https://arxiv.org/abs/2604.10954) | 2026 | Bounding-box guided fine-grained image editing with FineEdit-1.2M and FineEdit-Bench. | Supports the harness need for target regions, boxes, and background preservation gates. |
| [X2Edit](https://arxiv.org/abs/2508.07607) | AAAI 2026 | 3.7M dataset, 14 editing tasks, task-aware MoE-LoRA on FLUX.1. | Strong argument for task-specific LoRA/adapters instead of one generic edit model. |
| [ReasonEdit](https://arxiv.org/abs/2511.22625) | 2025/2026 window | Thinking-editing-reflection loop. | Maps well to a planner -> execute -> evaluate -> revise harness loop. |
| [Hypothetical Instruction-Based Image Editing](https://arxiv.org/abs/2507.01908) | Updated 2026 | Visual reasoning editing with Reason50K/ReasonBrain. | Useful when edit instructions require reasoning over scene state. |

## Architecture And Efficiency

| Paper | Date/status | Main idea | Harness relevance |
|---|---|---|---|
| [EdgeDiT](https://arxiv.org/abs/2603.28405) | CVPR 2026 Mobile AI Workshop | Hardware-aware DiT design for NPUs with parameter/FLOP/latency reductions. | Supports local creator workstation and on-device draft routes. |
| [TEASR](https://arxiv.org/abs/2606.16188) | Jun 2026 | Any-step diffusion transformer for real image super-resolution. | Candidate post-processing/upscale route and benchmark target. |
| [PixelDiT](https://arxiv.org/abs/2511.20645) | 2025/2026 | Pixel-space DiT without autoencoder, extending to high-resolution T2I. | Watch for future non-VAE workflow routes that may not fit old ComfyUI assumptions. |
| [Diffusion Transformers with Representation Autoencoders](https://arxiv.org/abs/2601.16208) | 2026 | Replaces VAE-style bottlenecks with richer representation autoencoders. | Track because control graphs may need new latent/encoder assumptions. |
| [SD3.5-Flash](https://arxiv.org/abs/2509.21318) | 2025/2026 | Few-step distillation for memory-efficient consumer-device generation. | Useful default fast route if licensing and quality match. |

## Guidance And Personalization

| Method | Source | Main idea | Harness relevance |
|---|---|---|---|
| LoRA | [LoRA paper](https://arxiv.org/abs/2106.09685) | Freeze base weights and inject low-rank trainable matrices. | Base mechanism for creator-specific style/character/product adapters. |
| ControlNet | [ControlNet repo](https://github.com/lllyasviel/ControlNet) and [paper](https://arxiv.org/abs/2302.05543) | Adds trainable conditional branch to pretrained diffusion model. | Foundation for edges, depth, pose, segmentation, and sketch controls. |
| IP-Adapter | [Paper](https://arxiv.org/abs/2308.06721) | Decoupled image prompt adapter. | Reference image and style/identity control route. |
| T2I-Adapter | [Paper](https://arxiv.org/abs/2302.08453) | Lightweight adapter for external conditions. | Alternative or complement to ControlNet. |
| SAG | [Self-Attention Guidance](https://arxiv.org/abs/2210.00939) | Training-free guidance from intermediate self-attention maps. | Candidate guidance mode when supported by route. |
| Self-Guidance for diffusion and flow matching | [Paper](https://arxiv.org/html/2412.05827v4) | Training-free guidance across newer diffusion/flow models. | Useful abstraction beyond CFG/SAG. |

## Evaluation

| Benchmark | Scope | Harness use |
|---|---|---|
| [T2I-CoReBench](https://t2i-corebench.github.io/) | Unverified lead: surfaced as a 2026 composition/reasoning benchmark candidate, but no stable primary paper/proceedings source was verified in this pass. | Keep as a watch item only; do not make it a hard gate until rechecked. |
| [GenEval 2](https://arxiv.org/html/2512.16853v1) | Studies benchmark drift and saturation. | Warning that old T2I benchmarks may no longer separate modern models. |
| [GenColorBench](https://openaccess.thecvf.com/content/CVPR2026/papers/Butt_GenColorBench_A_Color_Evaluation_Benchmark_for_Text-to-Image_Generation_CVPR_2026_paper.pdf) | CVPR 2026 color fidelity benchmark with RGB/hex/name prompts. | Brand color and palette QA. |
| [FineEdit-Bench](https://arxiv.org/abs/2604.10954) | Fine-grained edit benchmark. | Inpaint/edit region QA. |
| REDEdit-Bench | Introduced with FireRed-Image-Edit. | Instruction edit evaluation. |
| Story and comic consistency survey | [Survey](https://link.springer.com/article/10.1007/s10462-025-11482-6) | Character, style, space, time, event, and theme continuity for comics/story outputs. |

## Video And Sequential Creation

| Source | Main idea | Harness relevance |
|---|---|---|
| [StoryDiffusion](https://storydiffusion.github.io/) | Consistent self-attention for character-consistent comics and videos. | Reference design for character consistency across panels and generated video. |
| [Awesome Multi-Image Generation](https://github.com/AIDC-AI/Awesome-Multi-Image-Generation) | Curates multi-view, multi-character, temporal, semantic, and style consistency work. | Use as watchlist for comic and video roadmap. |
| Wan model family | Text-to-video, image-to-video, video editing, continuation, and reference-to-video routes. | Candidate second-stage creation route after still image generation. |

## What To Read First

1. FireRed-Image-Edit and FineEdit for edit workflow and evaluation.
2. GenColorBench and GenEval 2 for evaluation design; keep T2I-CoReBench as an unverified lead until a stable primary source is confirmed.
3. FLUX.2, Qwen-Image, Z-Image, and HunyuanImage technical docs/model cards for route registry.
4. SAM3 and Grounded-SAM docs for mask and region control.
5. ComfyUI workflow API and V3 custom node docs for runtime integration.

## 2026-06-20 Benchmark Gate Addendum

Core evaluation gates for the harness should be capability-specific:

| Gate | Candidate sources | Harness use |
|---|---|---|
| Text rendering fidelity | [STRICT](https://aclanthology.org/2025.emnlp-main.1070/), [OCRGenBench](https://huggingface.co/datasets/PeirongZhang/OCRGenBench) | OCR correctness, readable length, multilingual text, prompt-to-text alignment. |
| Dense text and layout | [TextAtlas5M](https://textatlas5m.github.io/) | Posters, infographics, document-like images, comic/dialogue text. |
| Multi-reference fusion | [MultiRef](https://multiref.github.io/) | Moodboards, character/style/product reference blending. |
| Edit correctness and preservation | [Co-EditBench](https://openreview.net/forum?id=tKz0XEaZXw), [GIE-Bench](https://machinelearning.apple.com/research/gie-bench), [Inter-Edit](https://openaccess.thecvf.com/content/CVPR2026/papers/Liu_Inter-Edit_First_Benchmark_for_Interactive_Instruction-Based_Image_Editing_CVPR_2026_paper.pdf) | Localized edits, grounded edits, iterative creator control. |
| Color/palette fidelity | [GenColorBench](https://openreview.net/forum?id=E9zStzWz6M) | Brand colors, palette constraints, hex/name/RGB prompts. |
| Compositional adherence | [GenEval 2](https://arxiv.org/html/2512.16853v1) | Counts, attributes, relations, multi-atom prompts. |
| Identity/personalization | [DreamBench++](https://dreambenchplus.github.io/) | Character/product/persona consistency. |
| Production utility | [ServImage](https://openreview.net/forum?id=bH2JgJdHp0) | Real commercial design usefulness, not just academic preference. |

Note: the exact `T2I-CoReBench` source was not verified in the second-wave pass. Keep it marked unverified until a primary benchmark page is found.
