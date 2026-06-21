# Ultraresearch Synthesis: Agent Skill-Compatible Image Generation Harness

Date: 2026-06-20

Workers: 9 agents plus orchestrator checks. Waves: 2. Verification artifacts: `verify-comfyui-control.md`, Context7 ComfyUI docs queries, Hugging Face paper/model searches, web primary-source fetches, and `gh` repo/issue/PR checks.

## Executive Summary

The correct product architecture is a skill-first, ComfyUI-aware execution harness. Do not port ComfyUI into the harness. Keep ComfyUI as the graph runtime, model loader, sampler host, and custom-node ecosystem. Build an Agent Skill-compatible operating layer above it: creator intent graph -> capability index -> workflow compiler -> validator/policy gate -> ComfyUI Server API or Cloud API -> WebSocket/history observer -> visual evaluator -> export/provenance bundle.

The current 2026 image-generation stack is no longer "prompt -> one model -> image." The leading product and research direction is generation plus editing, multi-reference conditioning, text/OCR-aware design output, flow/DiT-based open models, cheap adapter personalization, CV-derived masks/controls, and downstream image-to-video/comic continuity. A useful harness must expose typed primitives for LoRA, reference images/IP-Adapter, ControlNet/T2I-Adapter/ControlNeXt/OminiControl, SAM/Grounded-SAM masks, inpaint/outpaint, matting, upscaling, guidance policies such as CFG/SAG/PAG, evaluation, and provenance.

The feasibility answer is strong but bounded. An LLM harness can control ComfyUI's practical execution lifecycle and discover installed nodes, models, templates, and subgraphs. It cannot guarantee universal semantic correctness or safety for arbitrary custom nodes. Custom nodes are executable Python supply-chain surface. ComfyUI metadata tells the harness what a node declares, not what opaque code will do. Therefore the harness must compile from an intermediate typed graph, validate against live `/object_info` and route policy, pin/allowlist custom-node installs, and treat every workflow/model-card/node-doc field as untrusted data.

## Core Findings

### 1. Agent Skill is the right first surface

Agent Skills are a portable package shape: `SKILL.md` plus optional `scripts/`, `references/`, and `assets/`, loaded progressively by compatible agents [S1, S2]. Codex docs explicitly describe skills as reusable workflow packages that can also be distributed via plugins [S2]. For this harness, the skill should be the stable contract, while scripts own deterministic indexing, compilation, validation, execution, evaluation, and export.

MCP should come second, not first. MCP is the structured tool/resource transport and supports model-discovered tools, schemas, structured content, and human-in-the-loop expectations [S3]. A2A should come later for peer-agent interoperability, not for local creator workflows.

### 2. ComfyUI is already agentable, but not formally complete

ComfyUI's official docs now expose Agent Tools / MCP for image, video, audio, and 3D generation [S4]. Cloud MCP is closed beta and supports generation, search, and workflow execution for Claude Code/Desktop [S5]. Partner MCP is private preview and unifies generation across 30+ partner providers, but it is not arbitrary custom local workflow execution [S6].

The local ComfyUI server exposes the necessary control-plane primitives: `/prompt`, `/ws`, `/object_info`, `/models`, `/history`, `/queue`, `/interrupt`, `/free`, upload/view routes, workflow templates, and subgraphs [S7, S8]. Direct `gh` verification pinned ComfyUI master at `dc3f8f314a987d23115ed278693e76cf6e72a5a0` on 2026-06-20. Context7 confirmed that `node_info` returns declared `INPUT_TYPES`, output types, names, display name, description, module, category, and related flags.

The hard gap is prompt/workflow schema and arbitrary custom-node semantics. ComfyUI issue #8899 and PR #13094 are both still open; they explicitly call out the lack of formal `/prompt` JSON Schema and workflow-vs-prompt confusion [S9, S10]. A harness should ship its own conservative schema and validate against the live capability index.

### 3. Existing attempts are partial, not a universal full-control harness

The closest public project is `artokun/comfyui-mcp`, verified on 2026-06-20 as a Claude Code plugin + MCP server with live graph editing, workflow execution/authoring, model/custom-node management, and a broad tool/skill surface [S11]. It proves feasibility of a broad agent layer, but it is still a ComfyUI-integrated control layer, not a formal guarantee for every custom-node workflow.

Other projects are useful but narrower:

- `LingyiChen-AI/comfyui-workflow-skill`: natural language to importable workflow JSON, template-first, not a live control plane [S12].
- `MieMieeeee/comfyui-agent-skill`: registered-workflow execution through local/self-hosted ComfyUI, structured JSON outputs, not arbitrary graph manipulation [S13].
- `HuangYuChuh/ComfyUI_Skills_OpenClaw` and `ComfyUI_Skill_CLI`: workflow-as-agent-skill packaging and execution CLIs, not universal graph editing [S14, S15].
- `AIDC-AI/Pixelle-MCP`: a broader ComfyUI + MCP + LLM multimodal AIGC solution; it strengthens the evidence that MCP-style agent control is active, but still does not prove universal semantic control for arbitrary custom nodes [S40].
- `joenorton/comfyui-mcp-server`: lightweight local Python MCP server for ComfyUI, and the project cited from ComfyUI issue #7780; it is a clear wrapper proof point rather than a full harness [S41].
- `21Pdontno/comfyui-workflow-skills`: workflow lifecycle automation skill for OpenClaw, Claude Code, and other skill-based agents; useful prior art for skill packaging, but low-signal on full custom-node semantics [S42].
- ComfyUI issue #7780 confirms user demand for MCP in core, while noting separate wrapper implementations already existed [S16].

No evidence was found for a maintained public project that fully owns all of these at once: complete workflow orchestration, arbitrary custom-node management, live graph editing, execution, skill packaging across Codex/Claude/etc., security policy, evaluation, and provenance. The ecosystem is moving toward agentability through bridges and wrappers.

### 4. 2026 model direction favors routing, not one default model

FLUX.2 positions itself for real creative workflows: multi-reference support, character/product/style consistency, structured prompts, text rendering, brand guideline adherence, and up to 4MP editing [S17]. Qwen-Image is a 20B MMDiT model focused on complex text rendering and precise editing, licensed Apache 2.0 [S18]. Qwen-Image-Edit combines Qwen2.5-VL semantic control with VAE appearance control [S19]. ERNIE-Image is an 8B single-stream DiT with prompt enhancement and author-reported strong open-weight performance [S20]. Z-Image is a 6B efficient family for generation/editing/turbo inference [S21]. HunyuanImage-3.0 is an 80B MoE native multimodal image model [S22].

The harness should route between fast local drafts, controlled ComfyUI graphs, premium hosted models, and training/personalization routes. It should store model license, architecture family, supported controls, VRAM/cost limits, quality profile, and eval packs.

### 5. CV control must be typed primitives, not a generic generate endpoint

The control stack should expose separate primitives:

- `personalization_lora.apply`
- `image_prompt_adapter.apply`
- `control_preprocess.{canny,depth,pose,lineart,seg}`
- `structural_control.apply`
- `segmentation.track`
- `guidance_policy.set`
- `image_edit.inpaint/outpaint`
- `matting.extract`
- `upscale.run`
- `evaluate.image`
- `provenance.attach/verify`

SAM3 is now a unified model for promptable segmentation in images/videos using text, exemplars, boxes, points, and masks [S23, S24]. ControlNet remains the canonical structural-control mechanism; ControlNeXt and newer universal control work reduce overhead and target modern backbones [S25]. IP-Adapter gives image-prompt conditioning through a lightweight decoupled-attention adapter [S26]. SAG and PAG are sampling-time guidance lines; they should be late-stage guidance policies, not replacements for spatial control [S27].

The recommended compiler order is synthesized from the evidence, not dictated by one paper: base model -> LoRA/adapters -> spatial controls -> SAM/Grounded-SAM masks as region gates -> IP/reference conditioning -> sampling guidance. Geometry should win over reference style when they conflict, unless the task is explicitly subject-centric.

### 6. Evaluation must be capability-specific

A creator harness should not optimize one image-quality score. Core gates should map to failure modes:

- text rendering and OCR: STRICT, OCRGenBench [S28]
- dense text/layout: TextAtlas5M/TextAtlasEval [S29]
- multi-reference fusion: MultiRef-bench [S30]
- instruction edit correctness/preservation: Co-EditBench, GIE-Bench, Inter-Edit [S31]
- color/palette fidelity: GenColorBench [S32]
- compositional adherence: GenEval 2 or a verified successor [S33]
- identity/personalization: DreamBench++ [S34]
- creator utility: ServImage [S35]

One expansion lane could not verify a primary page for the exact name `T2I-CoReBench`; keep it as unverified until a primary benchmark page is found.

### 7. Security and provenance are release blockers

MCP tools are model-controlled, and the tools spec recommends clear user-visible tool exposure and human confirmation for risky operations [S3]. The NSA May 2026 MCP security report warns about arbitrary code execution, manipulated tool metadata, hidden instructions in outputs, cascading prompt injection, and the need to treat tools/model outputs as untrusted [S36].

ComfyUI custom nodes are Python code. Installing them is installing executable code into the image runtime [S37]. The harness must use pinned allowlists, isolated ComfyUI profiles, minimal filesystem/network privileges, provenance logs, and no LLM-driven arbitrary installs.

C2PA and SynthID are evidence signals, not hard proof. C2PA can be absent/stripped/redacted; validators can only report what is present and valid [S38]. SynthID detection is probabilistic and not designed to stop determined adversaries [S39].

## Recommended Architecture

```text
skills/image-harness/
  SKILL.md
  references/
    intent-graph.md
    comfyui-contract.md
    control-stack.md
    model-registry.md
    eval-rubric.md
    security-policy.md
  scripts/
    index-capabilities
    compile-intent
    validate-workflow
    execute-comfy
    evaluate-output
    export-project
  assets/
    fixtures/
    goldens/
projects/<slug>/
  intent.json
  assets/
  workflows/
  runs/
  exports/
```

Minimum CLI contracts:

```text
image-harness index-capabilities --comfy-url http://127.0.0.1:8188
image-harness compile-intent projects/<slug>/intent.json
image-harness validate-workflow projects/<slug>/workflows/compiled/*.json
image-harness execute-comfy --workflow ... --comfy-url ...
image-harness evaluate-output --run-id ...
image-harness export-project --run-id ...
```

Ship phases:

1. Template filling for known-good workflows.
2. Approved subgraph composition: LoRA, ControlNet/depth/pose/edge, SAM mask, inpaint, IP-Adapter, upscale.
3. Schema-guided graph patching from live `/object_info`.
4. Internal custom-node authoring only for stable missing operations.
5. Custom-node install only with allowlist, pin, hash, isolated profile, and review.
6. MCP wrapper after CLI contracts are stable.
7. A2A endpoint only when peer-agent collaboration is necessary.

## Sources

- S1: Agent Skills specification, https://agentskills.io/specification
- S2: OpenAI Codex Skills, https://developers.openai.com/codex/skills
- S3: MCP tools spec 2025-11-25, https://modelcontextprotocol.io/specification/2025-11-25/server/tools
- S4: ComfyUI Agent Tools, https://docs.comfy.org/agent-tools
- S5: Comfy Cloud MCP, https://docs.comfy.org/agent-tools/cloud
- S6: Comfy Partner MCP, https://docs.comfy.org/agent-tools/partner-mcp
- S7: ComfyUI server routes, https://docs.comfy.org/development/comfyui-server/comms_routes
- S8: ComfyUI workflow API format, https://docs.comfy.org/development/api-development/workflow-api-format
- S9: ComfyUI issue #8899, https://github.com/Comfy-Org/ComfyUI/issues/8899
- S10: ComfyUI PR #13094, https://github.com/Comfy-Org/ComfyUI/pull/13094
- S11: artokun/comfyui-mcp, https://github.com/artokun/comfyui-mcp
- S12: LingyiChen-AI/comfyui-workflow-skill, https://github.com/LingyiChen-AI/comfyui-workflow-skill
- S13: MieMieeeee/comfyui-agent-skill, https://github.com/MieMieeeee/comfyui-agent-skill
- S14: HuangYuChuh/ComfyUI_Skills_OpenClaw, https://github.com/HuangYuChuh/ComfyUI_Skills_OpenClaw
- S15: HuangYuChuh/ComfyUI_Skill_CLI, https://github.com/HuangYuChuh/ComfyUI_Skill_CLI
- S16: ComfyUI issue #7780, https://github.com/Comfy-Org/ComfyUI/issues/7780
- S17: FLUX.2 blog, https://bfl.ai/blog/flux-2
- S18: Qwen-Image, https://github.com/QwenLM/Qwen-Image
- S19: Qwen-Image-Edit blog, https://qwenlm.github.io/blog/qwen-image-edit/
- S20: ERNIE-Image technical report, https://arxiv.org/html/2605.25347v1
- S21: Z-Image, https://github.com/Tongyi-MAI/Z-Image
- S22: HunyuanImage-3.0, https://github.com/Tencent-Hunyuan/HunyuanImage-3.0
- S23: SAM3 repository, https://github.com/facebookresearch/sam3
- S24: Meta SAM3 announcement, https://ai.meta.com/blog/segment-anything-model-3/
- S25: ControlNeXt, https://arxiv.org/html/2408.06070v3
- S26: IP-Adapter, https://arxiv.org/abs/2308.06721
- S27: Self-Attention Guidance, https://arxiv.org/abs/2210.00939
- S28: STRICT, https://aclanthology.org/2025.emnlp-main.1070/
- S29: TextAtlas5M, https://textatlas5m.github.io/
- S30: MultiRef, https://multiref.github.io/
- S31: Co-EditBench, https://openreview.net/forum?id=tKz0XEaZXw
- S32: GenColorBench, https://openreview.net/forum?id=E9zStzWz6M
- S33: GenEval 2, https://arxiv.org/html/2512.16853v1
- S34: DreamBench++, https://dreambenchplus.github.io/
- S35: ServImage, https://openreview.net/forum?id=bH2JgJdHp0
- S36: NSA MCP Security Design Considerations, https://www.nsa.gov/Portals/75/documents/Cybersecurity/CSI_MCP_SECURITY.pdf?ver=bmgiSbNQLP6Z_GiWtRt6bg%3D%3D
- S37: ComfyUI custom nodes docs, https://docs.comfy.org/custom-nodes/backend/server_overview
- S38: C2PA specification, https://spec.c2pa.org/
- S39: SynthID docs, https://ai.google.dev/responsible/docs/safeguards/synthid
- S40: AIDC-AI/Pixelle-MCP, https://github.com/AIDC-AI/Pixelle-MCP
- S41: joenorton/comfyui-mcp-server, https://github.com/joenorton/comfyui-mcp-server
- S42: 21Pdontno/comfyui-workflow-skills, https://github.com/21Pdontno/comfyui-workflow-skills

## Gaps

- No installed local ComfyUI server was started in this session, so live `/object_info` output for the user's own custom-node set was not captured.
- Several 2026 benchmarks are preprints/submissions or live sites; their status may change.
- `T2I-CoReBench` remains a weak lead: it was surfaced as an evaluation candidate, but wave 2 did not verify a stable primary paper/proceedings source, so it should not be treated as a hard gate until rechecked.
- Provider-hosted model claims and live leaderboards are volatile and should be rechecked during implementation.

## Expansion Trace

- Wave 1 covered image models/papers, CV control stack, ComfyUI control feasibility, public ComfyUI agent attempts, and architecture/security.
- Wave 2 expanded benchmark gates, closest public ComfyUI projects, multi-control composition rules, and security/provenance.
- Convergence reason: second wave produced refinements but no evidence of a public universal full-control harness or a blocker to the proposed skill-first ComfyUI controller architecture.
