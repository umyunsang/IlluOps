# Ultraresearch Expansion Log

Core question: How should we design an Agent Skill-compatible LLM execution harness for creator-controlled image generation, current to June 20, 2026, with LoRA, ControlNet, SAM, SAG, ComfyUI, custom nodes, and later video/comic expansion?

Mode: ULTRAWORK + ULTRARESEARCH.

Tier: HEAVY. Reason: new architecture/domain model plus external integrations, security/evaluation concerns, ComfyUI runtime/API control, and explicit deep/aggressive research request.

Skills selected:
- `omo:ulw-loop`: user explicitly requested it; use durable goals, criteria, ledger, and evidence.
- `omo:ultraresearch`: user explicitly requested aggressive deep research; use saturation waves, expansion leads, cited synthesis.

Skills not selected:
- `omo:programming`: no source-code implementation requested in this turn.
- `omo:frontend`: no user-facing UI build requested in this turn.
- `github:github`: GitHub research will use web/gh/agent lanes directly; no PR/issue mutation requested.

Axes:
1. 2026 image model and paper trends: diffusion/rectified flow/DiT, multimodal editing, personalization, reference conditioning, text rendering, image-to-video bridge.
2. Creator control stack: LoRA and variants, ControlNet/control images, SAM/Grounded-SAM, SAG/attention guidance, depth/pose/segmentation/matting/upscaling/evaluation.
3. ComfyUI control surface: official API, workflow JSON, `/object_info`, `/prompt`, `/ws`, custom nodes, subgraphs, node schema, security and execution bounds.
4. Existing attempts: ComfyUI MCP, Claude/Codex skills, GitHub issues/PRs, tool servers, agent wrappers, graph editors, gaps for "all pipelines/all custom nodes".
5. Harness architecture: Agent Skill package shape, CLI/compiler, typed intermediate graph, MCP/A2A later, evaluation/security gates.

Session directory: `.omo/ultraresearch/20260620-154100-image-harness`

## Wave 0

Opened journal and reconciled local workspace:
- Existing files: `README.md`, `research/00-source-index.md`, `research/01-architecture-synthesis.md` through `research/10-recent-paper-watchlist-2026.md`, `evidence/*`, `scaffold/*`.
- Local workspace is not a git repository, so commit/PR instructions are not applicable for this research pass.
- `omo ulw-loop create-goals` created durable ULW state at `.omo/ulw-loop/019ee3bd-cc58-7d40-9c0f-1886d00759a0/`.

## Wave 1 Plan

Spawn independent lanes:
- A: 2026 image-generation models and recent papers.
- B: CV control stack for creator intent.
- C: ComfyUI API/custom-node/full-control feasibility.
- D: GitHub issue/PR/repo evidence for ComfyUI agent/MCP/Skill attempts.
- E: Harness architecture/security/evaluation plan.

## Wave 1 Returns

Workers returned:

- A: Model/paper lane found generation+editing convergence, text rendering as a core benchmark axis, multi-reference conditioning, flow/DiT backbones, adapter personalization, fragmented benchmarks, and image-to-video as downstream bridge.
- B: CV-control lane mapped LoRA, IP-Adapter, ControlNet/T2I-Adapter/ControlNeXt/OminiControl, SAM/SAM2/SAM3/Grounded-SAM, SAG/PAG/CFG, inpainting, matting, upscaling, evaluation, and provenance into typed harness primitives.
- C: ComfyUI feasibility lane confirmed control-plane API coverage and the metadata-vs-semantics boundary. It pinned ComfyUI source examples around `/object_info`, `/prompt`, queue/history, upload/view, subgraphs, and workflow templates.
- D: Existing-attempts lane found official Comfy MCP direction plus community MCP/Skill/CLI projects, but no single universal full-control Agent Skill-compatible harness.
- E: Architecture lane recommended skill-first packaging, deterministic CLI/runtime, MCP optional, A2A later, and layered security/evaluation/provenance.

New leads:

- OpenAI and current hosted model route.
- FLUX.2 / Qwen / Hunyuan / ERNIE / Z-Image model family details.
- Benchmark cluster for text, layout, color, edit, reference, identity, and production utility.
- Close public ComfyUI projects and remaining gaps.
- Multi-control composition policy.
- MCP/custom-node/provenance security threat model.

## Wave 2 Returns

Workers returned:

- Benchmark expansion: core gates are text/OCR, dense layout, compositional adherence, reference fusion, edit preservation, color fidelity, personalization/identity, plus creator utility. `T2I-CoReBench` remained unverified from a primary source.
- Closest-projects expansion: `artokun/comfyui-mcp` is the broadest public ComfyUI agent-control project; `LingyiChen-AI/comfyui-workflow-skill`, `MieMieeeee/comfyui-agent-skill`, and HuangYuChuh's CLI/skills are useful but narrower. Official Comfy MCP is authoritative but scoped/preview.
- Multi-control expansion: recommended ordering is base model -> LoRA -> spatial controls -> SAM/Grounded-SAM masks -> IP/reference conditioning -> SAG/PAG/CFG guidance.
- Security expansion: MCP metadata/output and ComfyUI custom nodes are untrusted; C2PA/SynthID are evidence signals, not hard authenticity proofs.

Convergence:

- No evidence found for a maintained public universal harness that controls every ComfyUI pipeline/custom node with formal semantic guarantees.
- Enough evidence collected to plan a skill-first ComfyUI controller with a typed intent graph, compiler, validation, evaluation, and provenance.
- Remaining open implementation-time requirement: run against the user's actual installed ComfyUI to capture live `/object_info`, installed custom nodes, model inventory, and runtime constraints.
