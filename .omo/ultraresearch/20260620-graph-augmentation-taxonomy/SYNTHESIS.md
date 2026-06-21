# Ultraresearch Synthesis: Workflow Graph Augmentation Taxonomy

Date: 2026-06-20

## Executive Summary

ControlNet, LoRA, SAM, and IPAdapter are examples, not the fixed feature set. The corrected product language is: the harness imports a Civitai/Comfy workflow first, snapshots capabilities, then lets the LLM choose among validated graph-augmentation families. A concrete node or model is only an implementation instance of a family.

The canonical unit is:

```text
original workflow + capability snapshot + typed graph patch manifest + validation evidence + localhost review gallery + static result manifest + final reproducibility package on accept + opt-in project memory profile when selected
```

This matches ComfyUI's graph model: workflows are node graphs, node definitions can be discovered through `object_info`, API workflows have a dedicated executable JSON format, models are selected through loader nodes, and custom nodes are both powerful and security-sensitive.

## Source-Backed Taxonomy

| Family | Intent it satisfies | Example technologies or nodes | Patch shape | Validation gate |
| --- | --- | --- | --- | --- |
| `asset_model_binding` | Add or replace required model assets. | checkpoint, VAE, LoRA, LoCon, LyCORIS, Hypernetwork, Textual Inversion, ControlNet weights, upscaler weights, Civitai AIR, HF/Civitai model links | add/replace loader, set model file, bind AIR/import URL, verify hash/base model/license | Civitai/HF resolver, Comfy Cloud import support, model folder/type, hash and base-model compatibility |
| `prompt_text_conditioning` | Change semantic request without changing graph topology much. | positive/negative prompt nodes, CLIPTextEncode, trigger words, CLIP skip, regional prompts, prompt schedules | set widget value, add prompt schedule, add regional prompt subgraph | schema match, prompt provenance, no unsafe hidden prompt injection |
| `sampler_latent_policy` | Control variation, size, batch, reproducibility, and img2img strength. | KSampler seed/steps/CFG/scheduler/denoise, latent size, batch size, refiner pass | set widget value, insert second sampler/refiner, change latent source | allowed ranges from node definition, reproducibility status, budget/latency check |
| `structure_spatial_control` | Preserve pose, layout, edges, depth, sketch, segmentation, or composition. | ControlNet, T2I Adapter, control LoRA, canny/depth/openpose/lineart/normal preprocessors | insert control preprocessor, load control model, connect conditioning, set strength/start/end | input image exists, preprocessor supported, control model compatible with base model |
| `reference_identity_style` | Preserve style, character, face, product, object, or composition from references. | IPAdapter, FaceID/InsightFace routes, CLIP Vision encoders, style/composition reference nodes, identity LoRA | upload reference image, encode reference, connect adapter, set weight/layer schedule | likeness/product-rights policy, reference quality, multi-reference binding risk, supported node pack |
| `source_image_edit` | Transform or edit an existing image rather than regenerate from scratch. | image-to-image, inpaint, outpaint, FLUX fill/Kontext-like routes, Qwen image edit, VAE encode for inpainting | upload image/mask, route to img2img/inpaint graph, set denoise/edit strength | source lineage, mask availability, denoise range, edit intent confidence |
| `segmentation_mask_routing` | Create/select local regions for edits or detail passes. | SAM/SAM2 when supported, CLIPSeg, bbox detectors, SEGS, mask dilation/blur/boolean ops | add detector/segmenter, create mask/SEGS, connect to inpaint/detailer/control routes | detector availability, mask preview, fallback if segmentation fails |
| `detail_refine_enhance` | Fix faces/hands/textures or improve final fidelity after main generation. | FaceDetailer, DetailerForEach, ESRGAN/RealESRGAN, UltimateSDUpscale, tiled sampling, color/sharpen/background nodes | add post-process subgraph, insert detailer/upscaler, connect output chain | output image exists, enhancement model supported, no destructive overwrite |
| `custom_node_dependency` | Use a workflow's required third-party functionality without hallucinating JSON. | Impact Pack, IPAdapter Plus, WAS nodes, custom preprocessors, partner nodes | resolve missing node pack, confirm supported-node inventory, insert imported/custom node only if discovered | object_info/supported-node catalog, trust/license/security review, version compatibility |
| `external_provider_or_partner_route` | Hand off to hosted/closed-source generation or utility APIs when the workflow uses them. | Comfy Partner Nodes, Civitai generation jobs, API nodes | preserve provider node, attach credential reference, route job lifecycle | credential presence, cost/budget, provider terms, retry/cancel behavior |
| `evaluation_feedback_loop` | Judge result fit and decide next patch. | VLM/CLIP scoring, product consistency checks, visual diff, user feedback cards | non-destructive sidecar evaluator; may emit advisory next patch proposal | rendered artifact exists, metrics are advisory, saved creator action is required before any continuation |

## Design Rules

1. The LLM chooses a patch family first, then a concrete node/model instance second.
2. The patch family is selected from creator intent, reference evidence, imported workflow topology, and available capabilities.
3. Concrete node selection must be backed by one of: imported workflow, Comfy Cloud `/api/object_info`, supported-node catalog, workflow template, reusable subgraph, or an approved recipe.
4. Workflow-bearing inputs must not be replaced by a generic template unless import or validation fails.
5. A patch cannot spend Cloud credits or Buzz until the original workflow and patched workflow have passed validation, the workspace `budget_envelope` exists, predicted spend fits inside that envelope, and the selected provider resolves through a local `credential_ref`.
6. Missing custom nodes are not an invitation to hallucinate equivalent JSON. They become `unsupported_node`, `install_required`, `alternative_patch`, or `blocked`.
7. Multi-reference or identity/product tasks require an explicit binding/evaluation note because current research still reports subject confusion, attribute leakage, and product-identity preservation failures.
8. Each workspace starts with one lightweight localhost creator brief. If a risk or ambiguity gate still needs creator input later, it must open a targeted localhost follow-up and ask in creative-brief vocabulary. Default creator mode must not ask developer-facing questions or mention node names, graph patches, samplers, denoise, `object_info`, or model-control implementation details.
9. The evaluation/feedback loop can propose only after a localhost review gallery, static HTML snapshot, `review_gallery_session.json`, `result_manifest.json`, sidecars, and saved `feedback.json` path exist; it cannot continue, rerun, refine, branch, or accept after generated images until a creator action is saved from the gallery.
10. If the saved action is accept, the loop must write `final_package_manifest.json` with accepted images, static review page, original/patched workflows, graph patch diff, prompt/settings, provenance, cost/budget summary, rights/license notes, and checksums. Final image files alone are not enough.
11. If the creator opts into future reuse at accept/finalize time, write `project_memory_manifest.json` as a managed reference to the final package with selected reuse dimensions, rights/reuse scope, creator-visible label, source hash, and deletion support. A final image, browser localStorage entry, or chat transcript is not project memory.
12. In a later workspace, saved memory can influence graph patch planning only after the creator selects its visible card in the `initial_brief`. Unselected, disabled, deleted, rights-blocked, or out-of-scope memory profiles must not become hidden prompt or graph context.
13. Cross-agent continuation must use a clean `handoff_manifest.json` with selected manifests/artifacts/status/memory refs/evidence pointers. It must not rely on chat transcript, hidden browser state, raw secrets, or unbounded intermediate artifact dumps, and it should refresh only at stable checkpoints.
14. A resumed agent must show a "continue this work?" confirmation screen before post-handoff spend or job submission. The graph-patch compiler may prepare advisory next actions from the handoff, but execution remains blocked until `resume_confirmation.json` records human confirmation.
15. The graph-patch compiler is an image domain-pack implementation, not a harness-core primitive. Under Q19, core can request planning/patching through the domain-pack interface but cannot directly import Civitai/Comfy/image graph-patch modules.
16. Future non-image packs that bring their own planner, patch compiler, renderer, memory policy, or adapters must pass Q20 manifest-first allowlist registration before core can load them; a local folder with a valid-looking manifest is not enough.
17. Registered packs still need Q21 least-privilege grants before a graph planner, patch compiler, adapter, renderer, memory policy, or finalizer can touch provider/network, file roots, localhost UI, project memory, budget, tools, or external agents.
18. External pack graph planners, patch compilers, adapters, renderers, memory policies, and finalizers must run through Q22 out-of-process JSON IPC/CLI workers; built-in image-pack code may be bundled but must satisfy the same worker interface.
19. Under Q23, graph planners and adapters do not perform provider calls, file writes, budget spend, project-memory writes, or external tool/agent calls directly. They emit `side_effect_request.json`, and core brokers perform approved side effects and write `side_effect_result.json`.
20. Under Q24, any graph-planning or adapter side-effect request must carry `idempotency_key`, `retry_policy`, `timeout`, and `cancel_token`; broker lifecycle events and terminal outcomes decide whether the operation may be replayed, cancelled, timed out, or retried.
21. Under Q25, graph adapters may receive a `pending` provider submission result, but long-running provider job refs, resume handles, polling cadence, cancel tokens, lease owner, and terminal outcomes are owned by the core scheduler and stored in manifests.
22. Under Q26, graph/domain workers and adapters cannot submit, poll, cancel, continue, or finalize unless the core scheduler grants the active workspace/job lease; non-holders receive read-only status/resume data until they acquire a valid lease.
23. Under Q27, graph/domain planning and execution state is persisted through file-first manifests and append-only JSONL events with atomic writes, file locks, and checksums; database/workflow-service adapters cannot become the only state store.
24. Under Q28, graph/domain snapshots are replay accelerators, not audit sources. `snapshot_manifest.json` must cover schema version, event range, checksum chain, and included manifest hashes; schema changes require `migration_ledger.json`; compaction cannot delete original events until retention rules allow it.
25. Under Q29, graph/domain browser surfaces cannot treat localhost as ambient authority. Intake, review, and resume actions must originate from loopback-only one-time-token sessions, pass CSRF and origin checks, and still pass CLI/state lease validation before `creator_intake.json`, `feedback.json`, `loop_state.json`, or `resume_confirmation.json` is mutated.
26. Under Q30, graph/domain workers cannot rely on process names or chat identity for mutation authority. Every graph-planning, provider, file, budget, memory, handoff, resume, or finalization mutation carries a stable `agent_id`, fresh `agent_session_id`, and workspace trust state; unknown or revoked agents receive read-only status/handoff data until explicitly trusted.
27. Under Q31, graph/domain outputs and intermediates need retention classes. Final packages and opt-in memory persist until explicit deletion; handoff/status persists until superseded or finalized; event logs/snapshots persist until retention permits compaction; raw workflow imports, temp renders, masks, preprocessed controls, provider caches, and other intermediates can be pruned only after reproducibility and handoff proofs exist, with tombstone events.
28. Under Q32, graph/domain patch planning decisions are host-agent-authored `decision_manifest.json` / `proposal_manifest.json` records by default. Optional model scoring, evaluator, or planner-assist calls are brokered `llm_model_call` side effects with `llm_call_manifest.json`; free-form LLM prose is never graph mutation authority.
29. Under Q33, graph/domain execution is live-provider-first when credentials exist. After graph validation, budget envelope, actor trust, lease, idempotency, broker, and cleanup gates pass, the image pack should exercise the real provider route by default; mock/offline routes are explicit fallback lanes with recorded reasons.
30. Under Q34, validated graph runs also require a provider-profile live cap before live execution. The workspace `budget_envelope` is necessary but not sufficient: `max_live_smoke_spend`, `max_live_jobs`, wall-clock cap, cleanup requirements, and `cap_snapshot` evidence must exist, and the workspace budget cannot exceed the provider-profile cap.
31. Under Q35, default CI/PR graph tests stay mock/offline even when credentials and caps exist. Live graph/provider checks require manual, scheduled, label-gated, or env-gated triggers plus cap and cleanup evidence; local executable demos remain live-first.
32. Under Q36, this graph taxonomy remains evidence for image domain pack 1 only. It cannot be the generality proof; v1 must also include the Q37-selected real second non-image production domain pack with domain-native artifacts.

## Example Reframing

Old wording:

```text
The LLM may add ControlNet, LoRA, SAM, IPAdapter, detailer, upscaler, or custom nodes.
```

Correct wording:

```text
The LLM may add any validated graph patch required by creator intent. ControlNet is one instance of structure/spatial control; LoRA is one instance of asset/model adaptation; SAM is one instance of segmentation/mask routing; IPAdapter is one instance of reference identity/style conditioning. The implementation must remain open to other supported nodes, custom-node packs, partner nodes, and future model families.
```

## Source Map

- Comfy workflow/API format and Cloud execution: https://docs.comfy.org/development/cloud/overview, https://docs.comfy.org/development/cloud/api-reference, https://docs.comfy.org/development/api-development/workflow-api-format
- Comfy graph/node/custom-node model: https://docs.comfy.org/development/core-concepts/nodes, https://docs.comfy.org/installation/install_custom_node
- Comfy model and template metadata: https://docs.comfy.org/development/core-concepts/models, https://docs.comfy.org/cloud/import-models, https://docs.comfy.org/interface/features/template
- Comfy conditioning/edit/refine tutorials: https://docs.comfy.org/tutorials/basic/lora, https://docs.comfy.org/tutorials/controlnet/controlnet, https://docs.comfy.org/tutorials/basic/image-to-image, https://docs.comfy.org/tutorials/basic/inpaint, https://docs.comfy.org/tutorials/basic/upscale
- Civitai SDK and API evidence: https://github.com/civitai/civitai-javascript, https://github.com/civitai/civitai-python, `.omo/ultraresearch/20260620-graph-augmentation-taxonomy/direct-verification.md`
- Research papers: https://arxiv.org/abs/2302.05543, https://arxiv.org/abs/2106.09685, https://arxiv.org/abs/2304.02643, https://arxiv.org/abs/2308.06721, https://arxiv.org/abs/2605.12088, https://arxiv.org/abs/2606.19103, https://arxiv.org/abs/2604.24023

## Plan Impact

- Replace example-specific language with the taxonomy above.
- Rename `add_lora_stack`, `wrap_with_controlnet`, `add_sam_mask_route`, and `add_ipadapter_reference` from the primary schema axis to examples under broader `family` and `op` fields.
- Require every graph patch to carry `family`, `op`, `target_nodes`, `inserted_nodes`, `assets`, `capability_source`, `intent_evidence`, `risk_gates`, and `validation_status`.
- Add `creator_intake.json`, `questionnaire_session.json`, and `credential_profile.json` as first-class artifacts that translate creator-facing answers into `intent_evidence`, `risk_gates`, `budget_envelope`, local `credential_ref`, and feedback-loop decisions, with `initial_brief` and `targeted_followup` session types.
- Match the `ppt-master` credential pattern: provider-specific key names, process environment precedence, persistent user config fallback, explicit active provider/profile, and no raw secrets in workspace artifacts.
- Add `review_gallery_session.json`, `result_manifest.json`, static HTML review output, image sidecars, advisory proposal cards, and `feedback.json` as first-class artifacts for result review. Under Q12, `feedback.json` / `loop_state.json` must record `creator_action_required` and saved actions such as accept, rerun, refine, branch, or stop; terminal summaries, raw file paths, and browser-only state are insufficient, and automatic post-result rerun is prohibited.
- Add `final_package_manifest.json` as the acceptance artifact. Under Q13, accepted output must bundle final images, static review page, original and patched workflow files, graph patch diff, prompt/settings, provenance chain, budget/cost summary, rights/license notes, and checksums for reproducibility and cross-agent handoff.
- Add `project_memory_manifest.json` as an optional accept-time artifact. Under Q14, accepted output becomes reusable project memory only when the creator opts in, and the memory record must reference `final_package_manifest.json` by id/hash with selected reuse dimensions, allowed reuse scope, rights policy, creator-visible label, and deletion support.
- Add Q15 memory application handling to intake and planning: eligible memory profiles appear as initial-brief cards, selected `memory_profile_id` values become explicit `creator_intake.json` / `workflow_plan.json` inputs, and unselected profiles are ignored.
- Add `handoff_manifest.json` for clean cross-agent continuation under Q16: include only current status, allowed next actions, selected manifests/artifacts, selected memory refs, and trace/evidence pointers; exclude chat transcript, raw secrets, hidden browser state, and unbounded intermediate dumps.
- Under Q17, refresh `handoff_manifest.json` at stable checkpoints only: initial brief submitted, review ready, feedback/action saved, and final package created.
- Under Q18, add `resume_confirmation.json` and block post-handoff `run` / `continue` / `finalize` / provider submission until the resume screen is confirmed.
- Under Q19, mark graph augmentation taxonomy and Comfy-specific patch operations as image domain-pack contracts behind `domain_pack_manifest.json`; add static checks that fail on direct core imports of image graph-patch code.
- Under Q20, add `domain_pack_registry.json` and registration/enablement checks for future non-image packs; arbitrary local folders with only `domain_pack_manifest.json` must not be allowed to inject planner, patch, adapter, review, memory, or finalizer code into the core.
- Under Q21, add declared/granted/effective capability scopes to domain-pack registry and security fixtures; graph/planner/adapter attempts outside granted scopes must fail before execution.
- Under Q22, add `domain_pack_worker_manifest.json` and worker protocol fixtures; external graph/planner/adapter code runs through JSON IPC/CLI workers and direct imports into core fail tests.
- Under Q23, add `capability_broker_manifest.json`, `side_effect_request.json`, and `side_effect_result.json`; provider calls, file writes, budget spend, project-memory mutation, and external tool/agent calls must be brokered by core and fail when a worker attempts them directly.
- Under Q24, extend broker and side-effect fixtures with idempotency key, explicit retry policy, timeout, cancel token, lifecycle events, terminal outcome, duplicate-key replay behavior, and rejection of non-idempotent automatic retries.
- Under Q25, extend `job_manifest.json`, provider side-effect results, and execution QA with provider job refs, resume handles, lease owner, polling cadence, cancel token, provider status normalization, and terminal outcome owned by the core scheduler.
- Under Q26, add lease fields and QA for single-writer submit/poll/cancel/finalize, heartbeat, TTL recovery, denied non-holder mutation, and read-only resume/status behavior.
- Under Q27, require graph/domain fixtures to persist state through canonical file-first manifests, append-only JSONL events, atomic writes, file locks, and checksums; optional adapters must export equivalent manifests before handoff or finalization.
- Under Q28, add `snapshot_manifest.json`, `migration_ledger.json`, replay-equivalence checks, and retention-gated compaction checks to graph/domain fixtures. Snapshots must not replace append-only event logs as the audit source.
- Under Q29, add `ui_session.json` and browser-action security fixtures for intake, review, and resume sessions; saved gallery/intake/resume actions must fail on expired/reused token, missing CSRF, bad Origin, LAN bind, or failed lease check.
- Under Q30, add `agent_identity.json`, `agent_trust_registry.json`, and actor/trust fields to graph/domain fixtures; graph workers and adapters must fail before mutation when the actor is unknown, revoked, missing a workspace grant, or cannot be tied to the current `agent_session_id`.
- Under Q31, add `retention_policy.json`, `cleanup_plan.json`, and tombstone events to graph/domain fixtures; cleanup must preserve accepted package reproducibility and handoff evidence before deleting raw intermediates, temp renders, masks, preprocessed controls, or provider caches.
- Under Q32, add `decision_manifest.json`, `proposal_manifest.json`, and `llm_call_manifest.json` to graph/domain fixtures; patch-family selection and proposal cards must be validated typed records, optional model evaluator calls must go through the `llm_model_call` broker path, and free-form LLM prose must not authorize graph mutation or paid execution.
- Under Q33, add live-provider-first run-mode fixtures: credentialed graph runs default to live provider execution, missing-credential or forced-offline fixtures record mock/offline fallback reasons, and provider terminal status/spend/cleanup evidence becomes part of graph-domain QA.
- Under Q34, add provider-profile cap fixtures: credentialed graph runs include `cap_snapshot` and `cap_check_result`, missing-cap and over-cap fixtures block before provider submission, and cap escalation from creator intake or agent proposals is rejected.
- Under Q35, add CI/check-mode fixtures: default CI graph tests run mock/offline without credentials, gated live graph checks require a trigger marker plus cap/cleanup evidence, and mock CI success is not labeled as live-provider proof.
- Under Q36, keep graph-patch taxonomy scoped to image pack 1 and add tests that the selected second domain pack can run without graph-patch, gallery, Civitai, or Comfy requirements leaking into core schemas.
- Update demos and QA fixtures so at least one flow proves each major family, while ControlNet/LoRA/SAM/IPAdapter remain representative examples rather than a closed list.

## Expansion Trace

- Wave 1 primary sources: Comfy official docs, Civitai official SDK/API surfaces, primary papers, and Comfy Cloud supported-node catalog.
- Direct verification: Civitai model-version API confirms AIR, hashes, file metadata, companion VAE, and conditional image metadata.
- User Q7 decision: validation-pass autopilot is selected, with stop-and-ask gates rendered as a creator-facing localhost questionnaire.
- User Q8 decision: the questionnaire timing is initial lightweight creator brief plus targeted follow-ups, not ambiguity-only intake and not full intake before every execution.
- User Q9 decision: collect a workspace budget envelope in the initial brief and auto-execute inside it; predicted overrun or unknown paid cost becomes a targeted follow-up.
- User Q10 decision: credentials follow the `ppt-master` `gpt-image-2` setup style, using provider-specific env/user config and storing only `credential_ref` in workspace artifacts.
- User Q11 decision: result review uses a localhost gallery plus static HTML/result manifest artifacts for comparison, selection, feedback, proposal cards, and cross-agent continuation.
- User Q12 decision: after each generated batch, stop in the localhost review gallery; proposal cards are advisory, and no automatic post-result rerun/refine/branch/accept continuation can happen without saved creator action.
- User Q13 decision: after accept, write a full reproducibility package; final images alone are not the accepted handoff.
- User Q14 decision: after accept, save project memory only when the creator opts in; memory is a managed reference to the final package, not automatic learning from accepted images, localStorage, or chat transcript.
- User Q15 decision: after memory is saved, future workspaces show it as visible initial-brief cards and apply it only when selected.
- User Q16 decision: handoff exposes a clean manifest/artifact/status package and excludes chat transcript, raw secrets, and hidden browser state.
- User Q17 decision: handoff refreshes at stable checkpoints, not every transient state change.
- User Q18 decision: handoff resume first shows a confirmation screen; no resumed spend or job submission before confirmation.
- User Q19 decision: image graph augmentation ships in the built-in reference domain pack, not harness core.
- User Q20 decision: future non-image domain packs require manifest-first allowlist registration, validation, security inventory, and workspace/user enablement before core loads them.
- User Q21 decision: registered domain packs require least-privilege capability grants before using provider/network, file, UI, memory, budget, tool, or external-agent capabilities.
- User Q22 decision: external domain-pack executable code runs out-of-process through JSON IPC/CLI workers; built-in packs must satisfy the same interface.
- User Q23 decision: worker side effects go through core capability brokers rather than direct worker execution.
- User Q24 decision: brokered side effects require idempotency, explicit retry, timeout, cancel, lifecycle, and terminal outcome fields; non-idempotent effects are not blindly retried.
- User Q25 decision: core scheduler owns durable provider-job leases and polling after broker approval; graph/domain workers do not own hidden provider polling loops.
- User Q26 decision: core scheduler uses single-writer workspace/job leases with heartbeat and TTL recovery; graph/domain workers without the active lease are read-only.
- User Q27 decision: file-first workspace state is source of truth; graph/domain outputs must be recoverable from manifests, event log, locks, and checksums without SQLite/Temporal.
- User Q28 decision: event-log snapshots and schema migrations use `snapshot_manifest.json` plus `migration_ledger.json`; append-only events remain the audit source and compaction is retention-gated.
- User Q29 decision: localhost graph/domain review and intake actions require loopback-only one-time-token UI sessions with CSRF/origin checks and CLI/state lease revalidation before mutation.
- User Q30 decision: graph/domain mutations require a trusted local actor identity; unknown agents can inspect status/handoff but cannot acquire leases or spend budget until explicitly trusted.
- User Q31 decision: graph/domain artifacts use retention classes and creator-facing cleanup; intermediates are pruned only after reproducibility/handoff proofs, and every deletion writes a tombstone event.
- User Q32 decision: graph/domain LLM decisions are host-agent-first typed manifests; optional evaluator or planner-assist model calls are brokered side effects recorded in `llm_call_manifest.json`, not hidden core router decisions.
- User Q33 decision: graph/domain execution and QA are live-provider-first when credentials exist; mocks remain explicit fallback/test lanes with recorded reasons.
- User Q34 decision: live graph/domain execution needs provider-profile hard caps; workspace budget alone cannot authorize live spend.
- User Q35 decision: graph/domain CI defaults to mock/offline; live provider checks are explicit gated runs with cap and cleanup evidence.
- User Q36 decision: graph taxonomy is not the non-image proof; a real second production domain pack must prove harness generality outside image workflows.
- Convergence: the requested correction changes artifact wording and schema taxonomy; it does not require product code implementation in this non-git planning workspace.
