# Image Generation And ComfyUI Findings

Date: 2026-06-20

## User Goal

Build an Agent Skill-compatible LLM execution harness for image generation. The harness should let creators control not only a generative model, but also LoRA, ControlNet, SAM, SAG, computer vision algorithms, image editing, and later video/comic creation.

## ComfyUI Feasibility Finding

ComfyUI can be the primary graph runtime for the harness. The practical pattern is:

```text
LLM -> intent graph -> workflow compiler -> ComfyUI API workflow JSON -> ComfyUI runtime -> output evaluator
```

The harness should not reimplement ComfyUI. It should discover, validate, and drive ComfyUI.

## Official ComfyUI Control Surface Captured

| Surface | Finding |
|---|---|
| Workflow API format | API workflows are JSON node graphs with node IDs, `class_type`, and `inputs`, exported via `File -> Export Workflow (API)`. |
| Local/cloud API | Official docs state both local server API and Comfy Cloud API use the same workflow format. |
| `/prompt` | Queues and validates workflows, returning prompt id or node errors. |
| `/ws` | Provides real-time execution progress, node status, errors, and queue updates. |
| `/object_info` | Provides node type details and is the key schema source for LLM graph validation. |
| `/models` | Lists model types and model files. |
| `/history` and `/view` | Retrieve completed outputs and image files. |
| Custom nodes V3 | Newer schema path with typed node definition concepts, async execution, progress reporting, and extension lifecycle. |
| Workflow templates and subgraphs | Custom nodes can expose example workflows and reusable subgraphs for composition. |
| Agent tools/MCP | Official docs describe Comfy Cloud MCP and Partner MCP for agent-driven media generation. |

## Repository Metadata Captured

| Repository | Metadata |
|---|---|
| `Comfy-Org/ComfyUI` | 117,630 stars, 13,759 forks, GPL-3.0, pushed 2026-06-20. Description: modular diffusion GUI, API, and backend with graph/nodes interface. |
| `Comfy-Org/ComfyUI-Manager` | 15,106 stars, 2,297 forks, GPL-3.0, pushed 2026-06-19. |
| `artokun/comfyui-mcp` | 166 stars, 33 forks, MIT, pushed 2026-06-20. Description: Claude Code plugin and MCP server for ComfyUI with 88/89 tools, AI skills, graph editing, model/custom node management. |
| `SaladTechnologies/comfyui-api` | 431 stars, 74 forks, MIT, pushed 2026-06-19. API server for horizontally scaling ComfyUI. |
| `ostris/ai-toolkit` | 10,927 stars, 1,362 forks, MIT, pushed 2026-06-19. Diffusion fine-tuning toolkit. |
| `kohya-ss/sd-scripts` | 7,130 stars, 1,198 forks, Apache-2.0, pushed 2026-06-18. Stable diffusion training scripts. |
| `modelscope/DiffSynth-Studio` | 12,603 stars, 1,232 forks, Apache-2.0, pushed 2026-06-18. Diffusion model training/inference studio. |

## Model Repositories Captured

| Repository | Metadata |
|---|---|
| `black-forest-labs/flux2` | 2,411 stars, 170 forks, Apache-2.0 repo, pushed 2026-03-12. |
| `QwenLM/Qwen-Image` | 8,021 stars, 506 forks, Apache-2.0, pushed 2026-02-10. |
| `Tencent-Hunyuan/HunyuanImage-3.0` | 3,138 stars, 165 forks, license other, pushed 2026-02-03. |
| `Tongyi-MAI/Z-Image` | 11,582 stars, 790 forks, Apache-2.0, pushed 2026-02-09. |
| `baidu/ERNIE-Image` | 477 stars, 34 forks, Apache-2.0, pushed 2026-04-17. |

## Important Source URLs

### ComfyUI

- https://docs.comfy.org/development/api-development/overview
- https://docs.comfy.org/development/api-development/workflow-api-format
- https://docs.comfy.org/development/comfyui-server/comms_routes
- https://docs.comfy.org/custom-nodes/v3_migration
- https://docs.comfy.org/custom-nodes/workflow_templates
- https://docs.comfy.org/custom-nodes/subgraph_blueprints
- https://docs.comfy.org/agent-tools
- https://github.com/Comfy-Org/ComfyUI
- https://github.com/artokun/comfyui-mcp

### Current image generation models and runtimes

- https://developers.openai.com/api/docs/guides/image-generation
- https://developers.openai.com/api/docs/models/gpt-image-2
- https://bfl.ai/models/flux-2
- https://github.com/black-forest-labs/flux2
- https://github.com/QwenLM/Qwen-Image
- https://github.com/Tongyi-MAI/Z-Image
- https://github.com/Tencent-Hunyuan/HunyuanImage-3.0
- https://github.com/baidu/ERNIE-Image
- https://github.com/Wan-Video/Wan2.1

### Control, CV, and personalization

- https://github.com/facebookresearch/sam3
- https://ai.meta.com/blog/segment-anything-model-3/
- https://github.com/IDEA-Research/Grounded-SAM-2
- https://github.com/DepthAnything/Depth-Anything-V2
- https://github.com/lllyasviel/ControlNet
- https://arxiv.org/abs/2106.09685
- https://arxiv.org/abs/2210.00939

## Decision

Build the harness as a ComfyUI-aware controller:

1. Agent Skill package as the install and activation surface.
2. CLI scripts for capability indexing, graph compilation, validation, execution, evaluation, and export.
3. ComfyUI local server API as first runtime.
4. Optional MCP server after CLI contracts are stable.
5. Optional hosted model routes for premium generation and video.
6. Project state that preserves creator intent, assets, workflows, outputs, and evaluation evidence.

## 2026-06-20 ULW / Ultraresearch Addendum

Research session:

```text
.omo/ultraresearch/20260620-154100-image-harness/
```

Key artifacts:

```text
.omo/ultraresearch/20260620-154100-image-harness/SYNTHESIS.md
.omo/ultraresearch/20260620-154100-image-harness/verify-comfyui-control.md
```

Direct verification:

- `gh repo view Comfy-Org/ComfyUI` observed 117,631 stars, updated 2026-06-20T06:39:00Z, default branch `master`.
- `gh api repos/Comfy-Org/ComfyUI/commits/master --jq .sha` returned `dc3f8f314a987d23115ed278693e76cf6e72a5a0`.
- `gh issue view 8899 --repo Comfy-Org/ComfyUI` confirmed `/prompt` JSON Schema issue remains open.
- `gh pr view 13094 --repo Comfy-Org/ComfyUI` confirmed the schema PR remains open.
- `gh issue view 7780 --repo Comfy-Org/ComfyUI` confirmed direct MCP support demand in core.
- `gh repo view artokun/comfyui-mcp` observed 166 stars and updated 2026-06-20.
- `gh repo view LingyiChen-AI/comfyui-workflow-skill` observed 312 stars and updated 2026-06-20.
- `gh repo view MieMieeeee/comfyui-agent-skill` observed 86 stars and updated 2026-06-16.

Final feasibility statement:

An LLM harness can control ComfyUI's execution lifecycle and installed node graph surface, but not prove arbitrary custom-node semantics. The correct plan is a skill-first ComfyUI controller with a typed intermediate intent graph, capability index, conservative compiler/validator, route policy, visual evaluation, and provenance export.
