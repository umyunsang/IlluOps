# Direct Verification Notes

Date: 2026-06-20

This file records claims directly checked by the lead agent, separate from subagent summaries.

## ComfyUI Integration Surface

- ComfyUI official docs list local server API routes including `/ws`, `/models`, `/models/{folder}`, `/workflow_templates`, `/upload/image`, `/upload/mask`, `/prompt`, `/object_info`, `/history`, `/queue`, `/interrupt`, and `/system_stats`.
  - Source: https://docs.comfy.org/development/comfyui-server/comms_routes
- ComfyUI API examples show submitting workflows to `/prompt` and require API-format workflow export.
  - Source: https://docs.comfy.org/development/comfyui-server/api-examples
- ComfyUI Workflow JSON is formally specified as JSON Schema; workflow nodes include IDs, types, links, inputs/outputs, widget values, and optional model metadata.
  - Source: https://docs.comfy.org/specs/workflow_json
- ComfyUI Node Definition JSON is formally specified. This supports inventorying installed node inputs/outputs for LLM-facing schema generation.
  - Source: https://docs.comfy.org/specs/nodedef_json
- Comfy Registry versions custom nodes; published versions are immutable, and registry names are globally unique. This is relevant for dependency manifests and reproducibility.
  - Source: https://docs.comfy.org/registry/overview
- ComfyUI-Manager supports install/update/disable/enable for custom nodes, snapshots, missing custom node detection, and registry-related metadata. It warns that it does not guarantee custom nodes function correctly.
  - Source: https://github.com/Comfy-Org/ComfyUI-Manager

## Existing Agent/Harness Projects

- `artokun/comfyui-mcp` is currently the closest full control-plane attempt. Its README says it can generate images/video, execute and author workflows, manage models/custom nodes, and edit the live ComfyUI graph. It is launched with `npx -y comfyui-mcp`, has 89 MCP tools and 22 AI skills, and ships Claude Code plugin commands.
  - Source: https://github.com/artokun/comfyui-mcp
- Its changelog reports manifest application, custom-node verification against `/object_info`, workflow dependency extraction/install, custom-node install/update/fix/sync, snapshots, health checks, and failure surfacing.
  - Source: https://github.com/artokun/comfyui-mcp/blob/main/CHANGELOG.md
- `HuangYuChuh/ComfyUI_Skills_OpenClaw` ships a `SKILL.md` that explicitly names Claude Code, OpenClaw, Codex, and Hermes. It exposes workflow import, schema-mapped parameters, dependency checks, image/mask upload, validation, history, server stats, and execution through `comfyui-skill`.
  - Source: https://github.com/HuangYuChuh/ComfyUI_Skills_OpenClaw
  - Source: https://github.com/HuangYuChuh/ComfyUI_Skills_OpenClaw/blob/main/SKILL.md
- `heshengtao/comfyui_LLM_party` is an LLM workflow/custom-node framework inside ComfyUI, not a general ComfyUI external control plane.
  - Source: https://github.com/heshengtao/comfyui_LLM_party
- Official Comfy Cloud MCP exists but is invite-only and cloud-scoped. Official Partner MCP is private preview and provider-oriented.
  - Source: https://docs.comfy.org/agent-tools/cloud
  - Source: https://docs.comfy.org/agent-tools/partner-mcp

## 2026 Model And Paper Trend Checks

- MultiBanana is a CVPR 2026 benchmark for multi-reference T2I generation/editing with reference count, domain mismatch, scale mismatch, rare concepts, and multilingual text-reference axes.
  - Source: https://openaccess.thecvf.com/content/CVPR2026/html/Oshima_MultiBanana_A_Challenging_Benchmark_for_Multi-Reference_Text-to-Image_Generation_CVPR_2026_paper.html
- FineEdit focuses on fine-grained image edit with bounding-box guidance and reports better instruction compliance/localization/background preservation than open-source baselines.
  - Source: https://arxiv.org/html/2604.10954v1
- A 2026 instruction-based image editing survey defines IIE as source image + textual instruction -> edited image and organizes tasks/evaluation around atomic and compositional editing.
  - Source: https://link.springer.com/article/10.1007/s44336-026-00034-3
- Qwen-Image repo reports Qwen-Image-2.0 launch, professional typography, 1k-token instructions, native 2K support, unified generation/editing, and native ComfyUI support for Qwen-Image.
  - Source: https://github.com/QwenLM/Qwen-Image
- Qwen-Image-Edit model card confirms semantic and appearance editing, precise bilingual text editing, and diffusers support.
  - Source: https://huggingface.co/Qwen/Qwen-Image-Edit
- FLUX.2 official blog reports multi-reference support up to 10 images, generation+editing in one architecture, up to 4MP outputs, better typography, prompt adherence, photorealism, and spatial relationships.
  - Source: https://bfl.ai/blog/flux-2
- Z-Image official sources describe a 6B image generation model family, Turbo generation, Omni/Base generation+editing, Edit specialized for image editing, strong bilingual text rendering, and consumer 16GB VRAM suitability.
  - Source: https://github.com/Tongyi-MAI/Z-Image
  - Source: https://tongyi-mai.github.io/Z-Image-blog/
- HunyuanImage 3.0 technical report describes an 80B total / 13B active MoE native multimodal image generation model with open assets.
  - Source: https://arxiv.org/html/2509.23951v1

## Control Primitive Checks

- ControlNet remains the classic spatial-conditioning primitive; newer work like ControlNeXt reduces overhead and broadens image/video generation control.
  - Source: https://arxiv.org/abs/2302.05543
  - Source: https://arxiv.org/abs/2408.06070
- IP-Adapter remains a key reference-image conditioning primitive compatible with text prompt and structural controls.
  - Source: https://arxiv.org/abs/2308.06721
  - Source: https://ip-adapter.github.io/
- ComfyUI has built-in SelfAttentionGuidance and PerturbedAttentionGuidance nodes; SAG is marked experimental/limited with chunked batches.
  - Source: https://docs.comfy.org/built-in-nodes/SelfAttentionGuidance
  - Source: https://docs.comfy.org/built-in-nodes/PerturbedAttentionGuidance
- PAG also exists as a custom-node package with broader guidance variants for ComfyUI/SD WebUI.
  - Source: https://github.com/pamparamm/sd-perturbed-attention
- ComfyUI official docs include a SAM 3.1 segmentation workflow, text-driven segmentation, image/video masks, and mask output usable by other workflows such as inpainting/background removal.
  - Source: https://docs.comfy.org/tutorials/utility/video-segment-sam3
