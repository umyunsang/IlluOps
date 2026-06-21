# Ultraresearch Synthesis: Reference Intent Routing

Date: 2026-06-20

## Executive Summary

The corrected architecture is **workflow-first graph augmentation**. If a Civitai/Comfy workflow is available, the harness must import the original workflow as the canonical base, validate it on the selected Comfy surface, and then let the LLM add or modify nodes according to the creator's intent. The reference-intent router is not the product's endpoint; it is the compiler that decides which graph patches are needed.

A Civitai/reference link can mean model asset import, exact reproduction, workflow import, source-image transformation, structure/layout control, identity/object consistency, or style/mood reference. The difference is that `workflow_import` now has priority: when workflow metadata or JSON exists, the harness preserves it and performs typed graph patches selected from a capability taxonomy. ControlNet, LoRA, SAM, and IPAdapter are representative examples, not the full set.

Default policy: workflow-bearing Civitai/Comfy inputs become `original_workflow + requested graph augmentation`. Bare model/version/download/AIR links remain asset bootstrap inputs. Bare image/post links without workflow metadata become reference/transform candidates, not exact reproduction requests. Exact reproduction is allowed only when the user explicitly asks for it and resolver evidence proves sufficient prompt, model/version, seed/settings, workflow, and resource coverage.

## Locked Product Decision

Replace the old Q6 option set with this rule:

```text
Q6 locked: Civitai/Comfy workflows are imported first, then patched by an LLM graph augmentation engine.

The reference-intent classifier selects graph patch lanes; it does not replace workflow import.

The router chooses among:
1. asset_import
2. exact_reproduction_candidate
3. workflow_import
4. source_transform_or_edit
5. structure_or_control
6. identity_or_object_consistency
7. style_or_mood_reference
8. reference_board_only

If the link alone is ambiguous, open the localhost creator questionnaire instead of asking a developer-facing terminal prompt. The question copy must use creator vocabulary, for example:
"이 이미지를 어디에 쓰려 하나요?", "참고 이미지에서 꼭 유지할 부분은 무엇인가요?", "분위기/구도/색감/제품 디테일 중 무엇이 가장 중요하나요?", "바뀌어도 되는 부분은 어디인가요?"
```

When `workflow_import` is available, the next action is not reference-board mode. The next action is:

```text
import original workflow -> snapshot object_info/capabilities -> validate original -> compile creator intent into graph patches -> validate patched workflow -> execute -> render localhost review gallery + static artifacts
```

## Intent Signals

| Signal | Route | Harness action |
| --- | --- | --- |
| Civitai model/version/download URL, AIR URN | `asset_import` | Resolve model/version/file/hash/base model/license; import to Comfy Cloud or use AIR with Civitai/Comfy route. |
| Comfy workflow JSON or generated image with workflow metadata | `workflow_import` | Load original workflow, preserve it, validate supported nodes/models, then patch it for the creator request. |
| User says "same", "exact", "recreate", "seed", "workflow", "settings" | `exact_reproduction_candidate` | Try only if metadata/resources/workflow coverage is sufficient; otherwise return partial/referenced reproduction with missing fields. |
| User says "based on this", "edit", "change only", "transform", "make this into..." | `source_transform_or_edit` | Upload input image, route to img2img/inpaint/Kontext-like recipe; preserve source lineage. |
| User says "pose", "layout", "composition", "outline", "depth", "sketch" | `structure_or_control` | Create depth/edge/pose/sketch/control assets and route to ControlNet or equivalent supported Comfy Cloud nodes. |
| User says "same character", "same product", "logo", "object", "clothing" | `identity_or_object_consistency` | Use IPAdapter/identity refs/product gate/LoRA when available; add consistency QA. |
| User says "vibe", "mood", "style", "palette", "lighting", "like this but..." | `style_or_mood_reference` | Use style reference, IPAdapter style/composition, LoRA/style recipe, or moodboard; do not claim exact object/person copying. |
| Civitai image/post has no useful metadata and no explicit edit intent | `reference_board_only` | Build visual reference board and open the localhost creator questionnaire before spending Cloud credits. |

## Evidence

### Creator tools split reference roles

- Midjourney separates style reference, character reference, and Omni reference; it documents weights/strength and warns that text prompts are still required for scene details.
- Adobe Firefly separates style matching from structure reference; structure reference uses outline/depth-like similarity and a strength value.
- Runway Gen-4 References supports styles, characters, objects, traits, sketches, and iterative reuse of reference assets.
- FLUX Kontext and ComfyUI docs frame image-conditioned generation as text+image editing or img2img workflows, with parameters controlling how far output deviates from the input.

Sources: Midjourney Style Reference, Character Reference, and Omni Reference docs; Adobe Firefly Structure Reference and Generative Match docs; Runway Gen-4 References docs; ComfyUI image-to-image and Flux Kontext docs.

### Workflow-first graph augmentation is the Cross-Agent fit

Cross-Agent LLM Execution Harness means the agent should operate the real production graph, not only choose a pre-baked recipe. The correct unit is:

```text
original workflow + typed patch set + validation evidence + localhost review gallery + static result artifacts
```

The LLM first selects an augmentation family, then selects a concrete supported node/model instance. The family set is open-ended, but currently grounded as:

| Family | What it means | Example instances |
| --- | --- | --- |
| `asset_model_binding` | Add, replace, or bind required model assets. | checkpoint, VAE, LoRA, LoCon, LyCORIS, Hypernetwork, Textual Inversion, ControlNet weights, upscaler weights, Civitai AIR, Hugging Face/Civitai model links |
| `prompt_text_conditioning` | Change semantic conditioning without large topology changes. | positive/negative prompt nodes, CLIPTextEncode, trigger words, CLIP skip, prompt schedule, regional prompt |
| `sampler_latent_policy` | Control reproducibility, variation, image size, batch, and denoise behavior. | KSampler seed/steps/CFG/scheduler/denoise, latent size, refiner pass, second sampler |
| `structure_spatial_control` | Preserve pose, composition, depth, edges, segmentation, sketch, or layout. | ControlNet, T2I Adapter, control LoRA, canny/depth/openpose/lineart/normal preprocessors |
| `reference_identity_style` | Preserve style, face, character, product, object, or composition from references. | IPAdapter, FaceID/InsightFace route, CLIP Vision encoder, style/composition reference node, identity LoRA |
| `source_image_edit` | Edit or transform an existing image instead of regenerating from scratch. | img2img, inpaint, outpaint, FLUX fill/Kontext-like route, Qwen image edit, VAE encode for inpainting |
| `segmentation_mask_routing` | Create/select local edit regions and masks. | SAM/SAM2 where supported, CLIPSeg, detectors, SEGS, mask dilation/blur/boolean ops |
| `detail_refine_enhance` | Improve faces, hands, texture, resolution, or final output fidelity. | FaceDetailer, DetailerForEach, ESRGAN/RealESRGAN, UltimateSDUpscale, tiled sampling, color/sharpen/background nodes |
| `custom_node_dependency` | Resolve required custom-node functionality without hallucinating JSON. | Impact Pack, IPAdapter Plus, WAS nodes, custom preprocessors, partner nodes |
| `external_provider_or_partner_route` | Preserve or add hosted provider/API nodes when the workflow requires them. | Comfy Partner Nodes, Civitai generation jobs, API nodes |
| `evaluation_feedback_loop` | Evaluate fit and propose the next patch. | VLM/CLIP scoring, product consistency checks, visual diff, rendered proposal cards |

Concrete examples: ControlNet is one `structure_spatial_control` instance; LoRA is one `asset_model_binding` instance; SAM is one `segmentation_mask_routing` instance; IPAdapter is one `reference_identity_style` instance. They must not be treated as the closed feature list.

Every graph mutation remains typed and evidence-bound:

- discover nodes through Comfy Cloud `object_info`, supported-node inventory, templates, subgraphs, or imported workflow schema;
- apply graph patch operations instead of free-form JSON hallucination;
- validate the patched workflow before spending Cloud credits;
- show a workflow diff explaining which nodes were added, why, and what user intent each patch satisfies.

Source: `.omo/ultraresearch/20260620-graph-augmentation-taxonomy/SYNTHESIS.md`.

### 2026 research says reference binding is hard

- UniCustom identifies multi-reference grounding/binding as a core failure mode: a model may understand which subject is referred to while still mixing appearance details across references.
- UniRef-Image-Edit and MACRO both treat multi-reference editing/composition as an active research problem requiring dedicated representations, training data, and evaluation.
- ProductConsistency and ServImage show that product/commercial creator tasks need identity-preservation and business-relevant evaluation, not only generic prompt adherence.

Sources: UniCustom (arXiv:2605.12088), UniRef-Image-Edit (arXiv:2602.14186), MACRO (arXiv:2603.25319), ProductConsistency (arXiv:2606.19103), ServImage (arXiv:2604.24023).

### Civitai image metadata is useful but not trustworthy enough for exact default

Live probe on 2026-06-20:

- `GET /api/v1/images?limit=50&nsfw=false&withMeta=true` returned 48 public items.
- 34 had metadata present; 14 had null generation metadata.
- Top-level `resources` and `modelVersionId` were absent in that sample.
- Single-image queries showed the actual generation metadata can be nested as `meta.meta`, and another image can return `meta: null` while still exposing `modelVersionIds`.

Source: `direct-verification.md`.

### Civitai model/version links are strong asset inputs

Live probe of `GET /api/v1/model-versions/290640` returned a model version with an AIR URN, base model, SafeTensor model file, VAE file, download URLs, and multiple hash types. That is enough to create a high-confidence model asset manifest.

Source: `direct-verification.md`.

### Comfy Cloud can support the routes, with limits

- Comfy Cloud import-models is Cloud-only and requires Creator tier or higher.
- The Cloud API can retrieve node definitions, upload images/masks, submit workflows, poll jobs, receive WebSocket progress, and download outputs.
- The API is experimental, so the harness must snapshot capabilities and record endpoint assumptions.
- The supported-node page lists relevant packs for creator workflows, including Impact Pack with SAM/segmentation/detailing/control nodes and IPAdapter Plus for reference conditioning.
- Workflow templates can embed Hugging Face and Civitai model links under `properties.models`.

Sources: Comfy Cloud import models, Cloud API, supported nodes, image-to-image, workflow, and template docs.

## Adversarial Conclusions

1. "Civitai image link means reproduce it" is unsafe. It may mean mood, style, identity, source edit, structure, or exact recreation; exact recreation is only one route.
2. "Reference router replaces workflow import" is wrong. If workflow metadata exists, import the original workflow first and use intent routing to decide graph patches.
3. "Metadata exists" is not enough. The resolver must prove prompt, negative prompt, seed, sampler, model/version/resource, dimensions, and workflow coverage before setting `reproducibility_confidence=exact_candidate`.
4. "Reference-only is failure" is wrong only when no workflow or executable metadata exists. If a workflow exists, falling back to reference-board mode before trying import/validation is a product bug.
5. "LLM can infer and mutate anything" is too risky when Cloud credits or Civitai Buzz may be spent. If clear and validation passes, mutate and execute automatically under Q7 option `a`; run a lightweight creator brief once at workspace start under Q8 option `a`; collect a workspace budget envelope under Q9 option `a`; resolve provider credentials from a `ppt-master`-style provider-specific env/user-config profile under Q10; then open targeted creator-facing localhost follow-ups only when ambiguity, predicted budget overrun, unknown cost, missing credits/Buzz, credential, rights, or unsupported-capability gates remain. After a batch renders, Q12 option `a` stops at the gallery: proposal cards are advisory until the creator saves accept/rerun/refine/branch/stop action. If that action is accept, Q13 option `a` writes a full reproducibility package rather than image-only output. Under Q14 option `a`, only creator-selected reuse dimensions become project memory. Under Q15 option `a`, saved project memory is shown as creator-visible cards in the next initial brief and only selected cards can influence that workspace. Under Q16 option `a`, continuation is exposed through a clean handoff manifest rather than chat transcript, raw secrets, or hidden browser state. Under Q17 option `a`, that handoff refreshes only at stable checkpoints. Under Q18 option `a`, a resumed session must show a "continue this work?" screen and wait for creator confirmation before spending budget or submitting a new job. Under Q19 option `a`, all of this image-specific behavior lives in a built-in reference domain pack behind `domain_pack_manifest.json`, not in harness core. Under Q20 option `a`, future non-image packs must also enter through manifest-first allowlist registration rather than arbitrary local-folder loading. Under Q21 option `a`, even registered packs can only resolve references, touch files, call providers, spend budget, use memory, or invoke external tools/agents within granted effective scopes. Under Q22 option `a`, external pack executable code must run through out-of-process JSON IPC/CLI workers rather than in-process imports. Under Q23 option `a`, worker side effects are typed requests and core brokers perform approved provider/network/file/budget/memory/tool/agent operations.
6. "Retrying a failed side effect is harmless" is unsafe for creator workflows because retries may duplicate provider jobs, file writes, memory writes, or paid spend. Under Q24 option `a`, brokered requests require idempotency key, explicit retry policy, timeout, cancel token, lifecycle events, and terminal outcome, and non-idempotent effects are not blindly retried.
7. "Worker polling is just an implementation detail" is wrong for cross-agent continuation. Under Q25 option `a`, provider job refs, resume handles, lease owner, polling cadence, cancel token, provider status, and terminal outcome belong in core scheduler manifests so another agent can resume without worker runtime state.
8. "Another agent can safely continue while the first one is polling" is unsafe unless the scheduler lease is transferred or recovered. Under Q26 option `a`, only the active single-writer workspace/job lease holder may submit, poll, cancel, continue, or finalize; other agents see read-only status/resume until lease acquisition or TTL recovery.
9. "The database or workflow engine can be the hidden source of truth" is not compatible with this cross-agent handoff goal. Under Q27 option `a`, reference resolution, planning, execution, review, memory, handoff, and resume must be inspectable from file-first manifests and append-only JSONL events with locks and checksums.
10. "Snapshotting current manifests is enough to resume" is incomplete. Under Q28 option `a`, snapshots accelerate replay but append-only events remain authoritative; schema changes require `migration_ledger.json`, and compaction cannot delete original events until final-package or handoff retention rules allow it.
11. "A localhost gallery action is automatically trusted" is unsafe. Under Q29 option `a`, intake, review, and resume mutations require loopback-only one-time-token sessions, CSRF, origin checks, action scope, and CLI/state lease revalidation before any reference plan, feedback action, or resume confirmation is persisted.
12. "A valid browser action or lease holder string is enough actor authority" is unsafe. Under Q30 option `a`, resolver and planning mutations require a trusted local `agent_id` plus current `agent_session_id`; unknown or revoked agents can inspect status/handoff but cannot acquire leases, spend budget, mutate memory, confirm resume, refresh handoff, or finalize.
13. "Imported reference intermediates can be deleted after rendering" is unsafe. Under Q31 option `a`, workflow imports, source images, masks, preprocessed controls, temp renders, and provider caches need retention classes and may be pruned only after final package or handoff reproducibility proofs exist; deletion requires tombstone evidence.
14. "LLM said add a patch" is not enough. Under Q32 option `a`, graph patch decisions require typed `decision_manifest.json` or `proposal_manifest.json` records with input hashes, selected action, actor/trust, validation status, trace, and evidence; any harness-owned model call must go through the broker as `llm_model_call` and write `llm_call_manifest.json`.
15. "Mocked provider success proves the workflow" is too weak after Q33 option `b`. When credentials exist, reference routing and graph patches should prove the live provider path after budget, actor trust, lease, idempotency, broker, and cleanup gates pass; mock/offline execution needs an explicit fallback reason.
16. "Workspace budget approved" is not enough for live provider spend under Q34 option `a`. Reference routing may recommend a live workflow only when the selected credential profile has hard caps for smoke spend, job count, wall-clock time, and cleanup requirements; missing or lower caps block before provider submission.
17. "CI passed" is not live-provider proof under Q35 option `a`. Default CI/PR reference-routing checks stay mock/offline; live provider drift checks need an explicit manual, scheduled, label, or env gate plus cap and cleanup evidence.
18. "Reference routing proves the harness is general" is false after Q36 option `b`. Reference/workflow routing proves image domain pack behavior; harness generality requires the Q37-selected second production domain pack with non-image artifacts.
19. "Style reference equals copying a style" needs rights and policy handling. Every external reference needs `source_rights`, `commercial_safe_required`, and `likeness_sensitive` fields.

## Manifest Addendum

Add this shape to the resolver output:

```json
{
  "references": [
    {
      "id": "ref_001",
      "source_url": "https://civitai.com/images/...",
      "source_kind": "civitai_image|civitai_model|air|workflow_json|image_file|comfy_cloud_share|hf_file",
      "intent_role": "asset_import|exact_reproduction_candidate|workflow_import|source_transform_or_edit|structure_or_control|identity_or_object_consistency|style_or_mood_reference|reference_board_only",
      "intent_confidence": "high|medium|low",
      "reproducibility_confidence": "exact_candidate|partial|reference_only|blocked",
      "evidence": {
        "has_prompt": true,
        "has_negative_prompt": true,
        "has_seed": true,
        "has_model_version": false,
        "has_workflow_json": false,
        "has_downloadable_model_asset": false
      },
      "controls": {
        "style_strength": null,
        "identity_strength": null,
        "structure_strength": null,
        "img2img_denoise": null
      },
      "policy": {
        "source_rights": "unknown|user_confirmed|public_domain|licensed|blocked",
        "likeness_sensitive": false,
        "commercial_safe_required": true
      },
      "creator_intake": {
        "needed": true,
        "surface": "localhost_browser_questionnaire",
        "language_mode": "creator",
        "timing_policy": "initial_brief_once_then_targeted_followups",
        "budget_policy": "workspace_budget_envelope_then_autopilot",
        "credential_policy": "ppt_master_style_provider_specific_profile_with_workspace_refs_only",
        "project_memory_application_policy": "show_eligible_memory_cards_in_initial_brief_apply_only_selected",
        "technical_mode_opt_in": false,
        "must_avoid_default_terms": ["object_info", "graph patch", "node", "ControlNet", "LoRA", "SAM", "IPAdapter", "sampler", "denoise"],
        "questions": [
          {
            "id": "usage_context",
            "copy": "이 이미지는 어디에 쓰려 하나요?",
            "examples": ["제품 상세 페이지", "SNS 광고", "앨범 커버", "캐릭터 시안", "프레젠테이션 이미지"]
          },
          {
            "id": "reference_role",
            "copy": "참고 이미지에서 꼭 유지해야 할 부분은 무엇인가요?",
            "examples": ["구도", "인물/제품 정체성", "색감", "분위기", "의상/소품", "배경"]
          },
          {
            "id": "design_elements",
            "copy": "원하는 디자인 요소를 골라주세요.",
            "examples": ["미니멀", "프리미엄", "키치", "시네마틱", "편집샵 느낌", "게임 일러스트 느낌"]
          },
          {
            "id": "change_tolerance",
            "copy": "바뀌어도 되는 부분과 바뀌면 안 되는 부분을 알려주세요.",
            "examples": ["배경은 바뀌어도 됨", "제품 형태는 유지", "인물 얼굴은 유지", "색감은 더 밝게"]
          },
          {
            "id": "budget_envelope",
            "copy": "이번 작업에서 자동으로 써도 되는 최대 시도 횟수나 예산은 어느 정도인가요?",
            "examples": ["최대 3회", "최대 30분", "최대 $5", "최대 Buzz 100", "무료/mock만"]
          }
        ]
      }
    }
  ],
  "review_surface": {
    "policy": "localhost_gallery_plus_static_artifacts",
    "post_result_policy": "creator_action_required_after_each_batch",
    "allowed_creator_actions": ["accept", "rerun", "refine", "branch", "stop"],
    "localhost_route": "/review",
    "static_html_path": "review/index.html",
    "result_manifest_path": "result_manifest.json",
    "review_gallery_session_path": "review_gallery_session.json",
    "feedback_path": "feedback.json",
    "required_capabilities": ["compare_images", "select_candidate", "show_proposal_cards", "save_feedback"]
  },
  "final_package": {
    "policy": "full_reproducibility_package_on_accept",
    "manifest_path": "final_package_manifest.json",
    "required_assets": ["accepted_images", "static_review_page", "original_workflow", "patched_workflow", "graph_patch_diff", "prompt_settings", "provenance", "budget_cost_summary", "rights_license_notes", "checksums"]
  },
  "project_memory": {
    "policy": "opt_in_managed_reuse_profiles",
    "application_policy": "creator_selects_eligible_memory_cards_in_next_initial_brief",
    "manifest_path": "project_memory_manifest.json",
    "source": "final_package_manifest.json",
    "allowed_dimensions": ["style", "brand_product", "workflow", "prompt_settings"],
    "required_fields": ["memory_profile_id", "creator_visible_label", "source_final_package_id", "source_hash", "allowed_reuse_scope", "rights_policy", "forgettable"],
    "application_fields": ["eligible_cards_shown", "selected_memory_profile_ids", "selected_reuse_dimensions", "blocked_profile_reasons"]
  },
  "handoff": {
    "policy": "clean_manifest_artifact_status_package",
    "refresh_policy": "stable_checkpoints_only",
    "manifest_path": "handoff_manifest.json",
    "checkpoint_reasons": ["initial_brief_submitted", "review_ready", "feedback_action_saved", "final_package_created"],
    "include": ["current_status", "allowed_next_actions", "manifest_paths_and_hashes", "selected_review_or_final_artifacts", "selected_memory_profile_refs", "trace_and_evidence_pointers"],
    "exclude": ["chat_transcript", "raw_secrets", ".env", "authorization_headers", "browser_localStorage", "browser_sessionStorage", "hidden_browser_state", "unsaved_browser_only_selections", "unbounded_intermediate_artifacts"]
  },
  "actor_identity": {
    "policy": "local_agent_identity_with_workspace_trust_grants",
    "identity_path": "agent_identity.json",
    "trust_registry_path": "agent_trust_registry.json",
    "required_fields": ["agent_id", "agent_session_id", "creator_visible_label", "host_runtime", "runtime_version", "trust_state", "workspace_grant_status", "evidence_path"],
    "mutation_rule": "unknown_or_revoked_agents_are_read_only_until_explicitly_trusted",
    "record_on": ["event_log.jsonl", "lease_records", "side_effect_request.json", "side_effect_result.json", "ui_session.json", "handoff_manifest.json", "resume_confirmation.json"]
  },
  "retention_cleanup": {
    "policy": "retention_classes_with_creator_cleanup",
    "policy_path": "retention_policy.json",
    "cleanup_plan_path": "cleanup_plan.json",
    "tombstone_path": "event_log.jsonl",
    "classes": ["final_package", "project_memory", "handoff_status", "audit_event_log", "snapshot", "raw_intermediate", "temp_render", "provider_cache"],
    "proof_rule": "raw_intermediates_prunable_only_after_reproducibility_and_handoff_proofs",
    "must_keep_until_explicit_delete": ["final_package", "project_memory"],
    "must_tombstone": true
  },
  "llm_decision": {
    "policy": "host_agent_first_typed_manifests_with_optional_brokered_llm_calls",
    "decision_path": "decision_manifest.json",
    "proposal_path": "proposal_manifest.json",
    "llm_call_path": "llm_call_manifest.json",
    "authority_rule": "validated_decision_manifest_required_for_execution_free_form_prose_is_advisory_only",
    "optional_broker_type": "llm_model_call",
    "required_fields": ["decision_id", "decision_type", "input_refs", "input_hash", "selected_action", "host_agent_id", "agent_session_id", "actor_trust_state", "output_schema", "trace_id", "evidence_path"],
    "llm_call_fields": ["provider_profile", "model_id", "input_hash", "output_hash", "output_schema", "budget", "credential_ref", "trace_id", "idempotency_key", "actor_trust_state", "evidence_path"],
    "must_reject": ["hidden_core_llm_router", "free_form_prose_authority", "unbrokered_harness_model_call"]
  },
  "live_provider_execution": {
    "policy": "live_provider_first_when_credentials_exist",
    "run_mode_field": "live|mock|offline",
    "profile_cap_required": true,
    "live_default_when": ["credential_ref_resolved", "provider_profile_live_cap_configured", "budget_envelope_approved", "workspace_budget_within_profile_cap", "actor_trusted", "active_lease_held", "broker_approved", "idempotency_key_present"],
    "fallback_allowed_when": ["missing_credential", "forced_offline", "deterministic_unit_test", "failure_fixture"],
    "required_evidence": ["fallback_reason_when_not_live", "provider_job_ref_when_live", "provider_terminal_status", "spend_estimate", "spend_consumed", "cap_snapshot", "cap_check_result", "cleanup_proof_path", "trace_id"],
    "provider_profile_cap_fields": ["max_live_smoke_spend", "max_live_jobs", "max_live_wall_clock_seconds", "cleanup_requirements", "cap_evidence_path"],
    "ci_default_mode": "mock_offline",
    "live_ci_gate_required": true,
    "live_check_allowed_when": ["manual_trigger", "scheduled_trigger", "label_gate", "env_gate"],
    "must_block_when": ["missing_provider_profile_cap", "workspace_budget_exceeds_provider_cap", "missing_cleanup_requirements", "silent_cap_escalation_attempt"]
  },
  "workflow": {
    "original_workflow_path": "workflows/imported/original.workflow.json",
    "original_api_workflow_path": "workflows/imported/original.api.json",
    "validated_original": false,
    "patched_workflow_path": "workflows/patched/current.workflow.json",
    "patches": [
      {
        "family": "asset_model_binding|prompt_text_conditioning|sampler_latent_policy|structure_spatial_control|reference_identity_style|source_image_edit|segmentation_mask_routing|detail_refine_enhance|custom_node_dependency|external_provider_or_partner_route|evaluation_feedback_loop",
        "op": "add_node|replace_node|insert_subgraph|connect_edge|disconnect_edge|set_widget_value|bind_asset|upload_image|upload_mask|set_budget_gate|preserve_provider_node|add_evaluator",
        "reason": "creator intent this patch satisfies",
        "examples": ["ControlNet, LoRA, SAM, and IPAdapter are examples only, not a closed list"],
        "capability_source": "object_info|supported_nodes|template|subgraph|imported_workflow",
        "target_nodes": [],
        "inserted_nodes": [],
        "assets": [],
        "risk_gates": ["budget|credential|license|likeness|custom_node_trust|base_model_compatibility"],
        "credential_ref": "profile://default/comfy-cloud",
        "validation_status": "pending|pass|fail|blocked"
      }
    ]
  }
}
```

## Plan Impact

Update downstream spec/tasks so the link resolver owns `source_kind`, `intent_role`, and workflow extraction. Do not let execution start from a Civitai image/post link until one of these is true:

1. A workflow was extracted, preserved, validated, and selected as the original graph to patch.
2. The user explicitly selected exact reproduction and metadata/workflow coverage is sufficient.
3. The user selected source transformation/editing.
4. The user selected style/mood/reference-board mode because no executable workflow is available.
5. The router inferred a high-confidence role from explicit user language.

Add a required `graph_patch_engine` component to the plan. It must compile creator intent into typed, family-labeled patches over the imported workflow, not create a parallel workflow from scratch unless import fails. The schema axis should be the augmentation family and validation gate, while named technologies such as ControlNet, LoRA, SAM, and IPAdapter remain examples of concrete supported instances.

Add a required localhost creator questionnaire surface. Under the selected Q7 policy, validated patches execute automatically within approved budget; under the selected Q8 policy, every workspace starts with one lightweight creator brief and later questionnaire sessions appear only as targeted follow-ups for unresolved creative intent, cost, credential, rights, unsupported capability, or safety gates. Under the selected Q9 policy, the initial brief must collect a workspace `budget_envelope`; live Comfy Cloud, Civitai Buzz, or other paid-provider execution can proceed automatically only while predicted and actual spend remain inside that envelope. Under the selected Q10 policy, credentials follow the `ppt-master` pattern: process environment first, provider-specific keys in persistent user config next, explicit active profile, and workspace artifacts store only `credential_ref` plus non-secret metadata. Under the selected Q15 policy, the initial brief must show eligible saved project-memory cards and persist only the cards selected by the creator; unselected memory cannot become hidden planner context. Its default wording must be creator-facing and gather usage examples, reference role, design elements, constraints, output format, priorities, memory-card choices, and budget before mapping answers back into graph patch planning. Persist questionnaire answers as `creator_intake.json` and browser/session state as `questionnaire_session.json`, with session type recorded as `initial_brief` or `targeted_followup`.

Add a required result review surface. Under the selected Q11 policy, every executed batch renders into a localhost review gallery for image comparison, candidate selection, feedback entry, and proposal cards, while also saving static HTML, `result_manifest.json`, `review_gallery_session.json`, per-image sidecars, and `feedback.json` so another agent can continue without browser memory. Under the selected Q12 policy, the scheduler must pause after each rendered batch in `creator_action_required`; proposal cards may recommend accept/rerun/refine/branch/stop, but continuation requires a saved creator action in `feedback.json` / `loop_state.json`. Under the selected Q13 policy, accepting a result must create a `final_package_manifest.json` reproducibility package with accepted images, static review page, original/patched workflows, graph patch diff, prompt/settings, provenance, budget/cost summary, rights/license notes, and checksums. Under the selected Q14 policy, accept-time creator opt-in can create `project_memory_manifest.json` for selected style, brand/product, workflow, or prompt/settings reuse; the memory profile must reference the final package by id/hash and carry rights/reuse scope. Under the selected Q15 policy, those profiles become future initial-brief cards, not automatic context. Under the selected Q16 policy, another agent resumes from `handoff_manifest.json` and referenced artifacts, not the chat transcript or hidden browser state. Under the selected Q17 policy, `handoff_manifest.json` refreshes only after initial brief submission, review-ready pause, saved feedback/action, and final package creation. Do not treat terminal summaries, raw image paths, image-only final export, browser localStorage, chat transcript, unselected project memory, unbounded intermediate dumps, every provider poll, or low evaluator confidence as sufficient review/finalization/handoff evidence.

Add Q18 resume confirmation to the handoff section: another agent can read `handoff_manifest.json`, but before it spends budget, submits a provider job, reruns, refines, branches, or finalizes, it must show a short "continue this work?" screen with current status, allowed/recommended next action, budget remaining, selected memory refs, and blockers, then persist `resume_confirmation.json`.

Add Q19 boundary language to the image workflow section: the Civitai/Comfy/image resolver, graph patch compiler, review gallery renderer, and provider adapters are built-in reference domain-pack implementations. Harness core may only call them through `domain_pack_manifest.json` and generic domain-pack interfaces; direct core dependencies on Civitai/Comfy/image modules are a scope violation.

Add Q20 domain-pack registration language to the harness section: future non-image packs require explicit install/register, manifest schema validation, version/source metadata, security inventory, allowlist state, and workspace/user-config enablement before core can load them. A valid-looking local `domain_pack_manifest.json` alone is not a trust boundary.

Add Q21 domain-pack permission language to resolver and planning gates: registered packs declare capability groups, install/enable surfaces show those groups in human-readable form, `domain_pack_registry.json` records granted and effective scopes, workspace config can narrow grants, and reference resolution or provider/job planning outside those scopes is blocked before execution.

Add Q22 domain-pack worker language to resolver and planning gates: external pack executable code uses `domain_pack_worker_manifest.json` and JSON IPC/CLI request/response envelopes. The core projects only effective granted env/file/provider/budget/memory scopes into the worker and rejects in-process imports for external packs.

Add Q23 side-effect brokerage language to resolver and planning gates: workers emit `side_effect_request.json`; core capability brokers enforce effective scope, budget envelope, credential refs, output inspection, trace, and evidence before performing provider calls, file writes, network access, budget spend, project-memory writes, or external tool/agent calls. Broker outcomes are stored in `side_effect_result.json`.

Add Q24 broker lifecycle language to resolver and planning gates: side-effect requests require `idempotency_key`, explicit `retry_policy`, `timeout`, `cancel_token`, lifecycle events, and terminal outcome. Duplicate idempotency keys replay the recorded result, retry exhaustion/cancellation/timeout become evidenced terminal states, and non-idempotent provider/file/memory/budget/tool/agent side effects cannot be retried blindly.

Add Q25 provider-job ownership language to resolver and planning gates: brokered provider submissions may return `pending`, but `job_manifest.json`, `event_log.jsonl`, and `side_effect_result.json` must store provider job refs, resume handles, lease owner, polling cadence, cancel token, provider status, and terminal outcome. Domain-pack workers must remain restartable/stateless between polls.

Add Q26 scheduler lease language to resolver and planning gates: mutating actions such as submit, poll, cancel, continue, finalize, and mutating handoff refresh require the active workspace/job lease. Lease acquire, heartbeat, release, stale recovery, denied non-holder mutation, and conflict events are recorded in `event_log.jsonl`; non-holder agents get read-only status/resume output.

Add Q27 file-first state language to resolver and planning gates: the canonical state package is `event_log.jsonl` plus JSON manifests written with atomic updates, file locks, and checksum chains. Optional SQLite/Temporal adapters can assist execution only when they export the same canonical files before handoff, resume, final package creation, or project memory use.

Add Q28 snapshot/migration language to resolver and planning gates: `snapshot_manifest.json` can accelerate resume only after verifying `schema_version`, covered event range, and checksum chain against `event_log.jsonl`. Schema drift requires `migration_ledger.json` with source/target schema versions, before/after hashes, migration id, and evidence path; compaction is retention-gated and cannot silently delete original events.

Add Q29 localhost UI session language to resolver and planning gates: browser-originating intake, gallery feedback, and resume actions are requests, not authority. Persisted action files such as `creator_intake.json`, `feedback.json`, `loop_state.json`, and `resume_confirmation.json` require valid `ui_session.json`, one-time token hash, unexpired action scope, CSRF, allowed Origin, and successful CLI/state lease validation.

Add Q30 actor identity/trust language to resolver and planning gates: resolving the right reference role or receiving a valid browser action is not enough to mutate state. Lease acquisition, provider submission, budget spend, project-memory writes, handoff mutation, resume confirmation, and finalization require a trusted `agent_id` plus current `agent_session_id`; unknown or revoked agents can read status/handoff only, and actor identity/trust state must be recorded on events, leases, broker requests/results, UI confirmations, and handoff/resume manifests.

Add Q31 retention/cleanup language to resolver and planning gates: reference resolution and workflow import can create raw intermediates, downloaded metadata, masks, preprocessing outputs, temp renders, and provider caches, but cleanup cannot delete them until `final_package_manifest.json` or `handoff_manifest.json` proves reproducibility/continuation no longer depends on them. Final packages and opted-in memory persist until explicit deletion, and all cleanup writes tombstone events with artifact hashes and actor trust state.

Add Q32 LLM decision boundary language to resolver and planning gates: host agents write `decision_manifest.json` and `proposal_manifest.json` for reference-role selection, graph patch choices, proposal cards, and continuation decisions. Optional harness-owned evaluator or planner-assist model calls must use a brokered `llm_model_call` side effect and write `llm_call_manifest.json` with provider profile, model id, input/output hashes, output schema, budget, credential ref, trace, actor/trust, and evidence. Free-form prose cannot authorize workflow mutation, provider spend, memory mutation, handoff mutation, or finalization.

Add Q33 live-provider-first language to resolver and planning gates: when the selected provider credential resolves, validation-passed reference/workflow routes should execute against the real provider after budget, actor trust, lease, idempotency, broker, and cleanup gates pass. Mock/offline execution is still valid for missing credentials, forced offline mode, deterministic unit tests, and failure fixtures, but must record `run_mode` plus fallback reason.

Add Q34 provider-profile cap language to resolver and planning gates: live reference/workflow routes require a credential profile with `max_live_smoke_spend`, `max_live_jobs`, max wall-clock, cleanup requirements, and cap evidence. The workspace budget envelope must fit inside that provider-profile cap; missing caps, lower caps, or silent cap escalation attempts block before live provider submission.

Add Q35 CI/live-check language to resolver and planning gates: default CI and PR checks run mock/offline reference-routing and graph fixtures, even when credentials and caps exist. Live provider checks require an explicit manual/scheduled/label/env gate and must record cap snapshot, cleanup evidence, terminal provider status, and fallback/block reason.

Add Q36 second-domain proof language to resolver and planning gates: image reference routing must not be treated as the entire non-image proof. The Q37-selected production domain pack must provide its own resolver/planner/validator/review/finalizer artifacts and prove generic handoff outside image workflows.

## Expansion Trace

- Wave 1 product docs: official creator tools split reference semantics into style, character/object, structure, source editing, and weights.
- Wave 1 papers: 2026 multi-reference generation/editing research treats grounding, binding, and consistency as unsolved enough to require structured reference slots.
- Direct verification: Civitai image metadata is inconsistent; Civitai model/version resources are stronger; Comfy Cloud has viable but bounded execution surfaces.
- User correction: Cross-Agent fit requires importing existing Civitai/Comfy workflows and augmenting them with LLM-selected graph patches when appropriate.
- Follow-up deep research: ControlNet, LoRA, SAM, and IPAdapter are examples under a broader augmentation taxonomy, not the product's closed node list.
- User Q7 decision: choose validation-pass autopilot, but ambiguity/risk questions must be creator-facing and rendered as a localhost survey-style questionnaire with application examples and design-element questions.
- User Q8 decision: choose initial lightweight creator brief plus targeted follow-ups, not ambiguity-only intake and not a full questionnaire before every execution.
- User Q9 decision: choose workspace budget envelope from the initial brief, then automatic execution inside that envelope; overrun or unknown cost opens a targeted follow-up.
- User Q10 decision: use the same API-key registration style as `ppt-master` for `gpt-image-2`: provider-specific env/user config, explicit active provider/profile, and workspace manifests with references only.
- User Q11 decision: choose localhost review gallery plus static HTML/result manifest storage for image comparison, selection, feedback, proposal cards, and cross-agent continuation.
- User Q12 decision: choose gallery-stop iteration; after each rendered batch, proposal cards are advisory and no automatic post-result rerun/refine/branch/accept continuation happens without saved creator gallery action.
- User Q13 decision: choose full reproducibility package on accept; final images alone are not enough for accepted handoff.
- User Q14 decision: choose opt-in project memory; only creator-selected reuse dimensions are stored as managed references to the final package.
- User Q15 decision: choose per-workspace memory application through visible cards in the next initial brief; only selected memory profiles apply.
- User Q16 decision: choose clean cross-agent handoff package; expose manifests/artifacts/status/memory refs/evidence pointers, not chat transcript, raw secrets, or hidden browser state.
- User Q17 decision: choose stable-checkpoint handoff refresh after initial brief submission, review-ready pause, saved feedback/action, and final package creation.
- User Q18 decision: choose a resume confirmation screen before post-handoff spend or job submission.
- User Q19 decision: choose built-in image reference domain pack with hard `domain_pack_manifest.json` boundary.
- User Q20 decision: choose manifest-first allowlist registration for future non-image domain packs.
- User Q21 decision: choose least-privilege capability grants for registered domain packs.
- User Q22 decision: choose out-of-process JSON IPC/CLI workers for external domain-pack executable code.
- User Q23 decision: choose core capability brokers for worker side effects.
- User Q24 decision: require broker idempotency, explicit retry policy, timeout, cancel token, lifecycle events, and terminal outcome for side effects.
- User Q25 decision: core scheduler owns long-running provider-job leases and polling; workers stay stateless and re-enter through manifests.
- User Q26 decision: use single-writer workspace/job leases with heartbeat and TTL recovery; non-holder agents are read-only until they acquire the lease.
- User Q27 decision: use file-first portable workspace state as source of truth; append-only JSONL events, JSON manifests, atomic writes, file locks, and checksums are required.
- User Q28 decision: use append-only event logs as the audit source with periodic snapshot manifests and migration ledgers; compaction cannot silently delete original events before retention rules allow it.
- User Q29 decision: use loopback-only one-time-token localhost UI sessions with CSRF/origin checks and CLI/state lease revalidation before intake, gallery feedback, or resume confirmation mutates state.
- User Q30 decision: use local actor identity/trust registry; unknown agents may inspect status/handoff but cannot acquire leases or perform budget-spending mutation until explicitly trusted, and actor trust state is recorded across events, leases, brokers, UI confirmations, and handoff/resume records.
- User Q31 decision: use retention classes and creator-facing cleanup; final packages/project memory persist until explicit deletion, proof-eligible intermediates can be pruned, and deletion writes tombstone events without retaining raw secrets.
- User Q32 decision: use host-agent-first typed LLM decision manifests, with optional brokered `llm_model_call` provider adapter for model calls and no hidden core LLM router or free-form prose authority.
- User Q33 decision: use live-provider-first execution and QA when credentials exist; mock/offline routes are explicit fallback lanes with recorded reasons.
- User Q34 decision: use provider-profile hard caps for live provider spend; workspace budget alone cannot authorize live reference/workflow execution.
- User Q35 decision: default CI/PR checks stay mock/offline; live provider checks are gated runs with cap and cleanup evidence, while local demos remain live-first.
- User Q36 decision: v1 must include a real second non-image production domain pack; image reference routing is not sufficient as the generality proof.
- Convergence: no unchecked lead changes the product decision. The open downstream work is implementation-spec work, not more research.
