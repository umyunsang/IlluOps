# Wave 1: Evaluation Gates

Worker: `019ee3f4-7648-7890-bee0-9fdcdca1acff`

## Key Findings

- 2025-2026 eval work supports pipeline-specific gates rather than a single generic quality score.
- Mandatory gates should be attached to the claimed capability of each certified recipe.
- Comic/video gates are useful roadmap signals, but many comic sources are understanding benchmarks rather than generation certification benchmarks.

## Pipeline To Gate Mapping From Worker

- Text/OCR-centric generation: OCR accuracy, layout fidelity, dense/small text, multilingual coverage.
- Multi-reference fusion: all-reference incorporation, consistency, copy-paste avoidance.
- Localized/interactive editing: localization, non-target preservation, multi-instance separation, over-editing resistance.
- Color/brand fidelity: exact color adherence and color-object binding.
- Identity/personalization: identity preservation, diversity tradeoff, local identity collapse, multi-subject separation.
- Compositional adherence: attributes, relationships, counting, comparison, logic/negation, implicit reasoning.
- Super-resolution: pixel fidelity, perceptual realism, fixed LR reproducibility.
- Creator usefulness: real-world prompt coverage and human preference calibration.
- Video consistency: temporal coherence, shot continuity, identity drift, motion rationality.
- Comic consistency: OCR, reading order, character persistence, dialogue grounding; weak for full generation certification.

## Sources Reported By Worker

- OCRGenBench: https://arxiv.org/html/2507.15085v4
- MultiBanana: https://github.com/matsuolab/multibanana
- MICON-Bench: https://github.com/Angusliuuu/MICON-Bench
- VTEdit-Bench: https://arxiv.org/abs/2603.11734
- Inter-Edit: https://openaccess.thecvf.com/content/CVPR2026/html/Liu_Inter-Edit_First_Benchmark_for_Interactive_Instruction-Based_Image_Editing_CVPR_2026_paper.html
- MIRAGE: https://arxiv.org/abs/2604.05180
- WiseEdit: https://openaccess.thecvf.com/content/CVPR2026/papers/Pan_WiseEdit_Benchmarking_Cognition-_and_Creativity-Informed_Image_Editing_CVPR_2026_paper.pdf
- GenColorBench: https://openaccess.thecvf.com/content/CVPR2026/papers/Butt_GenColorBench_A_Color_Evaluation_Benchmark_for_Text-to-Image_Generation_CVPR_2026_paper.pdf
- WithAnyone: https://openreview.net/forum?id=xFo13SaHQm
- When Identities Collapse: https://openaccess.thecvf.com/content/CVPR2026W/P13N/papers/Chen_When_Identities_Collapse_A_Stress-Test_Benchmark_for_Multi-Subject_Personalization_CVPRW_2026_paper.pdf
- PSR: https://openreview.net/forum?id=eBswLY0BSz
- GenAI-Bench: https://linzhiqiu.github.io/papers/genai_bench/
- T2I-ReasonBench: https://openreview.net/forum?id=1hYITxAwdz
- NTIRE 2026 SR report: https://arxiv.org/html/2604.14558v1
- ECHO: https://openreview.net/forum?id=nOcy5NvNI1
- MSVBench: https://arxiv.org/html/2602.23969v1
- VGBE 2026: https://openaccess.thecvf.com/content/CVPR2026W/VGBE/papers/Wu_VGBE_2026_Challenge_on_Image-to-Video_Consistent_Generation_Methods_and_Results_CVPRW_2026_paper.pdf
- AnimationBench: https://arxiv.org/html/2604.15299v1
- WBench: https://arxiv.org/html/2605.25874v1
- CoMix: https://arxiv.org/html/2407.03550v2
- MangaVQA/MangaOCR: https://arxiv.org/html/2505.20298v3

## Worker EXPAND Markers

LEAD:

- OCRGenBench
- MultiBanana
- MICON-Bench
- VTEdit-Bench
- Inter-Edit
- MIRAGE
- WiseEdit
- GenColorBench
- WithAnyone
- When Identities Collapse
- PSR
- GenAI-Bench
- T2I-ReasonBench
- NTIRE 2026 SR report
- ECHO
- MSVBench
- VGBE 2026
- AnimationBench
- WBench
- CoMix
- MangaVQA/MangaOCR

DEAD END:

- none; worker marked comic sources and some benchmarks as weak proxies rather than direct generation-certification sources.
