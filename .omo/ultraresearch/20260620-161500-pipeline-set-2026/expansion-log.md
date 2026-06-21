# Expansion Log: Pipeline Set 2026

Date: 2026-06-20

Core question: Which certified ComfyUI pipeline set should the installable Agent Skill-compatible image harness plan support first, given 2026 image-generation/control/editing trends and papers?

Known locked decisions:

- MVP target is an installable Agent Skill package reachable through `npx skills add`.
- Codex and Claude Code are both first-class install targets.
- Project positioning is hybrid: independent skill package first, with ComfyUI upstream contribution candidates split out.
- ComfyUI is chosen because its official/custom-node ecosystem is active for image-generation and computer-vision workflows.
- Pipeline choice is pending research; do not force a template-only answer without 2026 evidence.

Initial axes:

1. 2026 image generation/editing papers and benchmark trends.
2. Multi-control pipeline primitives: LoRA, ControlNet/T2I-Adapter/ControlNeXt, IP-Adapter/reference control, SAM/masks, inpaint/upscale.
3. ComfyUI node/custom-node ecosystem and realistic certified pipeline packaging.
4. Creator workflow roadmap: text2img, img2img, inpaint, upscale/fix, character/reference consistency, video/comic bridge.
5. Evaluation and acceptance gates per pipeline.

## Wave 1

Status: in progress.

Returned:

- Local workspace evidence worker: architecture already locked; certified pipeline order remains open.
- 2026 papers worker: recommended eight candidate pipeline families and surfaced paper/model leads.
- ComfyUI ecosystem worker: certification should be pinned recipe + dependency manifest + smoke test, not arbitrary custom-node semantics.
- Evaluation worker: certification gates must be pipeline-specific and tied to recent benchmark failure modes.
- Multi-control primitive worker: separate global adapters, reference conditioning, spatial control, masks, guidance, and refinement passes.

Open leads after first returns:

- Verify whether the eight paper-driven pipeline families should collapse into fewer MVP certified recipes.
- Verify which candidate papers are primary/reliable enough to drive plan decisions versus roadmap notes only.
- Cross-check ComfyUI ecosystem feasibility for those paper-driven pipelines.
- Collapse evaluation gates into MVP recipe acceptance criteria.
- Determine whether the first plan should encode recipes by creator task (`text2img`, `img2img`, `localized_edit`) or by control capability (`reference_conditioned`, `spatial_control`, `mask_refine`).

## Wave 2

Status: complete.

Returned:

- Recipe-collapse worker: paper-driven families collapse into general create/edit, localized edit, reference-conditioned composition, and optional high-res refine. Physical realism, video, and comic/storyboard remain roadmap-only.
- ComfyUI harness worker: existing attempts were verified. `artokun/comfyui-mcp` is closest to a full MCP control plane; `ComfyUI_Skills_OpenClaw` is closest to agent-safe Skill workflow execution; neither exactly matches the desired Codex/Claude `npx skills add` certified recipe harness.
- Evidence reliability worker: strongest benchmark anchors are MultiBanana, Inter-Edit, GenColorBench, PICABench, and PhyEdit. FineEdit/ProductConsistency/OCRGenBench are useful but immature. FLUX.2 is a model-family integration target, not a benchmark.
- Direct verification: ComfyUI local API, Workflow JSON, Node Definition JSON, registry immutability, ComfyUI-Manager behavior, existing agent projects, and 2026 model/paper sources were checked directly.

Resulting synthesis:

- Support every custom node through an uncategorized inventory/execution lane.
- Certify only pinned creator recipes with manifests, dependency/model contracts, smoke tests, and quality gates.
- Recommended planning recipe set:
  1. `controlled_t2i`
  2. `controlled_img2img`
  3. `localized_edit`
  4. `reference_composition`
  5. optional `refine_upscale`

Next Socratic decision:

- Decide whether the package is a thin orchestrator over `artokun/comfyui-mcp` when available, an owned CLI/control layer, or an upstream-first contribution.
