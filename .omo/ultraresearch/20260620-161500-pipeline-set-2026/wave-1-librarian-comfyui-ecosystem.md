# Wave 1: ComfyUI Ecosystem And Certified Recipe Feasibility

Worker: `019ee3f4-6b01-7a23-9242-97fcaab17bd6`

## Key Findings

- ComfyUI official docs support a certifiable core and a broad community extension layer.
- The right package model is not "certify arbitrary custom-node semantics"; it is pinned recipes with explicit dependency manifests, model lists, and smoke tests.
- Core-only workflows can have the highest certification confidence.
- Custom-node-backed workflows can be certified only as pinned recipes, not as generic semantic abstractions.
- ComfyUI-Manager can help install/discover dependencies, but the skill package must own an explicit allowlist and model/download contract.

## Feasibility By Pipeline

- `txt2img core`: feasible, core only, high certification bar.
- `img2img core`: feasible, core only, high certification bar.
- `ControlNet canny/depth/pose`: feasible if pinned, core + `comfyui_controlnet_aux` + exact ControlNet models.
- `FLUX control`: feasible if using official Flux workflows/templates.
- `IPAdapter reference/style`: feasible as pinned recipes, not as one generic reference feature.
- `detailer / detector upscale`: feasible as pinned recipes with `ComfyUI-Impact-Pack`.
- `mask-from-text -> inpaint`: feasible as a fixed chain using SAM/Grounded-SAM nodes plus inpaint nodes.
- `outpaint / crop-stitch repair`: feasible with strict recipe and image/mask bounds contract.
- `upscale`: feasible, high if core-only and medium if custom-node-backed.
- `workflow-template execution via MCP`: feasible if named recipes map to pinned workflow JSON.

## Sources Reported By Worker

- ComfyUI workflow docs: https://docs.comfy.org/development/core-concepts/workflow
- ComfyUI custom nodes docs: https://docs.comfy.org/development/core-concepts/custom-nodes
- Install custom nodes: https://docs.comfy.org/installation/install_custom_node
- Nodes docs: https://docs.comfy.org/development/core-concepts/nodes
- Custom-node troubleshooting: https://docs.comfy.org/troubleshooting/custom-node-issues
- Workflow templates: https://docs.comfy.org/interface/features/template
- Custom-node workflow templates: https://docs.comfy.org/custom-nodes/workflow_templates
- ComfyUI Agent Tools / MCP: https://docs.comfy.org/agent-tools
- Comfy Cloud MCP: https://docs.comfy.org/agent-tools/cloud
- ComfyUI-Manager: https://github.com/Comfy-Org/ComfyUI-Manager
- ControlNet preprocessors: https://github.com/Fannovel16/comfyui_controlnet_aux
- IPAdapter: https://github.com/cubiq/ComfyUI_IPAdapter_plus
- Impact Pack: https://github.com/ltdrdata/ComfyUI-Impact-Pack
- Inpaint/outpaint helpers: https://github.com/Acly/comfyui-inpaint-nodes
- SAM/Grounded-SAM segmentation: https://github.com/storyicon/comfyui_segment_anything
- SAM2 segmentation: https://github.com/kijai/ComfyUI-segment-anything-2
- MCP/skill wrappers: https://github.com/artokun/comfyui-mcp and https://github.com/LingyiChen-AI/comfyui-workflow-skill

## Worker EXPAND Markers

LEAD:

- ComfyUI core workflows and built-in templates
- ComfyUI-Manager as install/discovery helper
- ControlNet preprocessors
- IPAdapter
- Impact Pack
- Inpaint/outpaint helpers
- SAM / Grounded-SAM segmentation
- MCP / skill wrappers for fixed recipes

DEAD END:

- Universal support for arbitrary third-party ComfyUI custom nodes.
- "All workflows" certification without a node allowlist.
- Popularity-based certification without semantic pinning.
