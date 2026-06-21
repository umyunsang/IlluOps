# Synthesis: 2026 ComfyUI Agent Skill Image Harness Plan Inputs

Date: 2026-06-20

## Executive Conclusion

The strongest plan is a two-tier ComfyUI harness:

1. Universal ComfyUI inventory/execution lane.
   - Discover installed nodes through `/object_info`.
   - Parse Workflow JSON and Node Definition JSON.
   - Import arbitrary API-format workflows.
   - Extract required custom nodes/models.
   - Install or verify dependencies through ComfyUI-Manager/registry/comfy-cli-compatible mechanisms where available.
   - Execute workflows, stream status, collect outputs, logs, history, and errors.
   - Mark outputs as "uncertified" unless the workflow is bound to a tested recipe manifest.

2. Certified creator recipe lane.
   - Provide small, validated recipes with pinned workflow JSON, parameter schema, model manifests, custom-node manifests, smoke tests, and quality gates.
   - Let the LLM control these recipes in detail without exposing unstable raw node internals by default.
   - Allow expert mode to inspect/edit node graphs, but require validation before execution.

This reconciles the user's intent: all custom nodes should be usable, but only verified recipe families should be promised as semantically reliable.

## Why Not "All Custom Nodes Fully Certified"

ComfyUI's APIs make syntax-level discovery and execution possible. They do not make arbitrary custom-node semantics safe or reliable.

Evidence:

- `/object_info` and Node Definition JSON can describe node inputs/outputs.
- Workflow JSON can represent arbitrary graph structure.
- Comfy Registry and ComfyUI-Manager help with custom-node identity/version/install.
- ComfyUI-Manager explicitly does not guarantee every custom node functions correctly.
- Custom node dependency/import failures are common enough that Manager has missing-node install, snapshots, and conflict surfaces.

Therefore the plan should support every node through inventory/execution, but certify only pinned recipes.

## Existing Work Answer

There are already ComfyUI agent/harness attempts:

- `artokun/comfyui-mcp`: closest current full control plane; `npx -y comfyui-mcp`, MCP tools, live graph editing, model/custom-node management, workflow authoring, dependency installation, verification, Claude Code plugin.
- `HuangYuChuh/ComfyUI_Skills_OpenClaw`: closest Skill-style wrapper; explicit Codex/Claude Code support, `SKILL.md`, CLI workflow import/execution/schema mapping/dependency checks/validation; graph-opaque and clone/pip installed.
- `heshengtao/comfyui_LLM_party`: LLM nodes inside ComfyUI, not a general external control plane.
- `apppps/comfyui-agent`: early/weak evidence for full harness requirements.
- Official Comfy Cloud MCP and Partner MCP exist, but current docs scope them to cloud/private/provider surfaces rather than the local custom-node ecosystem target.

The opportunity is contribution-shaped: combine `comfyui-mcp`-level control, `ComfyUI_Skills_OpenClaw`-style agent-safe skill UX, and our own certified recipe/evaluation manifests into a Codex/Claude installable package.

## 2026 Trend Summary

The trend is not "just text-to-image." It is controllable image generation/editing under constraints:

- Unified generation and editing model families.
- Multi-reference conditioning for character/product/style/layout consistency.
- Localized and selection-guided editing.
- Stronger text rendering and OCR-sensitive image generation.
- High-resolution editing/refinement as a downstream production pass.
- Physical realism/effect-aware editing as a specialized research frontier.
- Safety and visual-prompt attack surfaces in large image editing models.

Model families to track:

- Qwen-Image / Qwen-Image-Edit / Qwen-Image-2.0.
- FLUX.2.
- Z-Image.
- HunyuanImage 3.0.

Benchmark/evaluation anchors:

- MultiBanana for multi-reference.
- Inter-Edit and FineEdit for interactive/localized editing.
- GenColorBench and ProductConsistency for color/product constraints.
- OCRGenBench / OmniDoc-TokenBench for text rendering.
- PICABench / PhyEdit for physical realism, roadmap-only.

## Recommended Recipe Set For Planning

### MVP Core

1. `controlled_t2i`
   - Purpose: text-to-image with creator-level control.
   - Slots: base model, prompt/negative prompt, LoRA stack, optional ControlNet preprocessor/model, optional IPAdapter/reference image, sampler, seed, CFG, steps, size, optional SAG/PAG.
   - Why: directly matches user intent around text2img + LoRA + ControlNet + IPAdapter.

2. `controlled_img2img`
   - Purpose: image-to-image variation/transformation.
   - Slots: source image, denoise strength, prompt, LoRA stack, optional ControlNet derived from source or separate control image, optional IPAdapter/reference, seed/sampler/settings.
   - Why: directly matches user intent around img2img plus detailed control.

3. `localized_edit`
   - Purpose: mask/box/selection-guided edit and inpaint/outpaint.
   - Slots: source image, user mask or SAM/SAM3/Grounded-SAM prompt, edit prompt, optional ControlNet/IPAdapter/LoRA, mask grow/blur, inpaint model, detailer.
   - Why: 2026 editing evidence says localized editing is operationally distinct and creator-critical.

4. `reference_composition`
   - Purpose: style, subject, character, product, and multi-reference composition.
   - Slots: 1-N reference images, role labels, weights, adapter/model route, prompt, optional spatial control.
   - Why: 2026 model and benchmark trend makes reference control central.

### MVP Plus / Optional Pack

5. `refine_upscale`
   - Purpose: production pass for high-resolution, details, faces/hands, and artifact repair.
   - Slots: source output, upscale model/method, target size, tiled decode/upscale, detailer/detector, strength, final save/export.
   - Why: important for creator usability, but downstream from core generation/editing.

### Roadmap Only

- Physical realism / effect-aware editing.
- Video generation/editing.
- Comic/storyboard/sequential panel consistency.
- Dedicated product/brand/OCR specialist recipes beyond shared quality gates.

## Certification Contract

Every certified recipe needs:

- Workflow JSON in API format.
- Human-readable recipe manifest.
- Node manifest with registry IDs, versions, required custom-node repos, and install method.
- Model manifest with filenames, paths, URLs, hashes when allowed, license notes, and VRAM estimates.
- Parameter schema with safe ranges and defaults.
- Compatibility matrix by model family: SD1.5, SDXL, FLUX, Qwen, Z-Image, and others as they become practical.
- Smoke test:
  - server health,
  - `/object_info` node availability,
  - model file presence,
  - minimal run or validation mode,
  - output nonblank and file saved,
  - logs/errors captured.
- Quality gate:
  - recipe-specific assertions based on reference, mask, text, color, or spatial-control failure modes.

## Control Slots

Use a common internal control schema:

- `base_model`: checkpoint or model family route.
- `prompt`: positive/negative text, style, composition, seed.
- `lora_stack`: one or more LoRA/LyCORIS/DoRA-like adapters with weights and compatibility checks.
- `reference_images`: IPAdapter/reference roles and weights.
- `spatial_control`: ControlNet/T2I-Adapter/ControlNeXt-style control maps and preprocessors.
- `mask`: user mask, SAM/SAM3/Grounded-SAM-generated mask, box, or segmentation prompt.
- `guidance`: CFG, SAG, PAG, and sampler-specific guidance controls.
- `refinement`: inpaint, outpaint, upscale, detailer, detector, tiled pass.
- `evaluation`: smoke and quality gates.

## Contribution Strategy

Recommended path:

1. Build an independent `npx skills add` compatible Skill package as the user-facing artifact.
2. Design it to interoperate with existing ComfyUI surfaces rather than replacing them.
3. Prefer optional adapters to `artokun/comfyui-mcp` where its MCP control plane is installed.
4. Borrow the useful abstraction from `ComfyUI_Skills_OpenClaw`: agent-safe parameter schemas over raw graph internals.
5. Contribute upstream once the manifest format and smoke-test behavior are proven:
   - Codex install docs.
   - recipe manifest conventions.
   - workflow dependency verification.
   - generated Skill wrappers for custom-node packs.

## Next Socratic Decision

The remaining ambiguity is not "should ComfyUI be used?" The evidence supports ComfyUI.

The next decision is the package architecture boundary:

- Option A: Skill package is a thin orchestrator over `comfyui-mcp` when available, plus recipe manifests.
- Option B: Skill package owns its own CLI/control layer, and treats `comfyui-mcp` as inspiration or optional interop.
- Option C: Contribute directly to `artokun/comfyui-mcp` / `ComfyUI_Skills_OpenClaw` first and delay a separate package.

Recommended default: Option A for fastest path and lowest duplication, with a fallback local executor so the Skill is not hard-dependent on one community MCP project.
