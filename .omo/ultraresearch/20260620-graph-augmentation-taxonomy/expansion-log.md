# Expansion Log: Graph Augmentation Taxonomy

Date: 2026-06-20

## Phase 0
Core question: Replace example-specific ControlNet/LoRA/SAM/IPAdapter language with a researched taxonomy for LLM graph augmentation over imported Civitai/Comfy workflows.
Axes: Comfy workflow/API constraints; model/asset augmentation; conditioning/editing/control primitives; custom-node capability and safety; plan/schema wording.
Codebase relevant: yes, documentation artifacts only. External: yes. Browsing: yes. Verification likely: content verification only; no runtime code.

## Wave 1

- Source retrieval covered Comfy official docs, Civitai official SDK/API evidence, primary papers for the example techniques, and Comfy Cloud supported-node/custom-node evidence.
- New artifact: `wave-1-primary-sources.md`.
- New artifact: `direct-verification.md`.
- New artifact: `SYNTHESIS.md`.

## Convergence

The open issue is not whether ControlNet/LoRA/SAM/IPAdapter matter; they do. The issue is taxonomy level. They are representative instances under broader patch families, and the plan artifacts should be updated accordingly.

## User Decision Addendum

- Q12 option `a`: evaluation/feedback graph patches may create advisory proposal cards after rendering, but the loop cannot continue, rerun, refine, branch, or accept until the creator saves an action in the localhost gallery.
- Q13 option `a`: accepted outputs become a full reproducibility package, not image-only export; `final_package_manifest.json` must carry final images, static review page, workflows, graph patch diff, prompt/settings, provenance, budget/cost, rights/license notes, and checksums.
- Q14 option `a`: accepted outputs become reusable project memory only through explicit creator opt-in; `project_memory_manifest.json` stores managed references to the final package with selected reuse dimensions, rights/reuse scope, source hash, creator-visible label, and deletion support.
- Q15 option `a`: saved project memory is shown as selectable cards in the next workspace initial brief; only selected cards influence graph planning.
- Q16 option `a`: cross-agent continuation uses a clean handoff manifest with selected manifests/artifacts/status/memory refs/evidence pointers, not chat transcript, raw secrets, or hidden browser state.
- Q17 option `a`: handoff manifest refreshes only at stable checkpoints: initial brief submitted, review ready, feedback/action saved, and final package created.
- Q18 option `a`: resumed handoff first shows a "continue this work?" confirmation screen; no resumed spend, provider job, rerun/refine/branch, or finalization before confirmation is saved.
- Q19 option `a`: image graph augmentation belongs to built-in reference domain pack 1 behind `domain_pack_manifest.json`; harness core cannot directly import or special-case it.
- Q20 option `a`: future non-image domain packs require manifest-first allowlist registration, schema validation, security inventory, version/source metadata, and workspace/user-config enablement before core can load their planners, adapters, review surfaces, memory policies, or finalizers.
- Q21 option `a`: registered packs still need least-privilege capability grants; graph planners, adapters, renderers, memory policies, and finalizers cannot use provider/network, file, UI, budget, memory, tool, or external-agent access outside granted effective scopes.
- Q22 option `a`: external graph planners, adapters, renderers, memory policies, and finalizers run out-of-process through JSON IPC/CLI workers; direct in-process import into core is a scope violation.
- Q23 option `a`: graph workers emit typed side-effect requests; core capability brokers perform approved provider/network/file/budget/memory/tool/agent side effects and write side-effect results.
- Q24 option `a`: graph worker side-effect requests require idempotency key, explicit retry policy, timeout, cancel token, lifecycle events, and terminal outcome; non-idempotent side effects are not blindly retried.
- Q25 option `a`: long-running provider jobs created by graph/domain adapters are owned by the core scheduler through manifests; workers stay stateless and do not hold polling loops.
- Q26 option `a`: graph/domain workers need the active single-writer workspace/job lease before mutating execution state; non-holders receive read-only status/resume data.
- Q27 option `a`: graph/domain state uses file-first manifests and append-only JSONL event logs with atomic writes, file locks, and checksums; optional durable adapters must export equivalent manifests.
- Q28 option `a`: graph/domain snapshots use `snapshot_manifest.json` with schema version, event range, and checksum chain, while schema changes use `migration_ledger.json`; append-only events remain authoritative and compaction is retention-gated.
- Q29 option `a`: graph/domain intake, review, and resume browser actions use loopback-only one-time-token UI sessions with TTL, CSRF, origin checks, read-only static artifacts by default, and CLI/state lease revalidation before mutation.
- Q30 option `a`: graph/domain mutations require trusted local actor identity. `agent_identity.json` and `agent_trust_registry.json` record stable `agent_id`, fresh `agent_session_id`, human-visible label, runtime metadata, optional fingerprint, workspace trust state, grants/revocations, and evidence. Unknown agents can inspect status/handoff but cannot acquire leases, spend budget, mutate memory, confirm resume, or finalize.
- Q31 option `a`: graph/domain artifacts use retention classes and creator-facing cleanup. Final packages and opt-in memory persist until explicit deletion; handoff/status persists until superseded/finalized; event logs/snapshots persist until retention allows compaction; raw graph intermediates, masks, temp renders, preprocessed controls, and provider caches are pruned only after reproducibility and handoff proofs; deletion writes tombstone evidence.
- Q32 option `a`: graph/domain LLM decisions use host-agent-first typed `decision_manifest.json` and `proposal_manifest.json` records. Optional graph evaluator, planner-assist, or scoring model calls are brokered `llm_model_call` side effects with provider profile, budget, input hash, model id, output schema, credential ref, trace, actor/trust, and `llm_call_manifest.json` evidence; hidden core LLM routing and free-form prose authority are rejected.
- Q33 option `b`: graph/domain execution and QA are live-provider-first when credentials exist. Validated graph patches should run against the real provider path after budget, actor-trust, lease, idempotency, broker, and cleanup gates pass; mock/offline runs are explicit fallback lanes for missing credentials, forced offline mode, deterministic tests, and failure fixtures.
- Q34 option `a`: live graph/domain execution requires provider-profile hard caps. Credential profiles declare max live smoke spend, max jobs, wall-clock cap, and cleanup requirements; missing or lower caps block live execution before provider submission.
- Q35 option `a`: default CI/PR graph checks stay mock/offline. Live provider graph checks require manual, scheduled, label, or env gates plus cap and cleanup evidence; local executable demos remain live-first.
- Q36 option `b`: image graph taxonomy is not enough to prove harness generality. A real second non-image production domain pack must ship in v1 and prove the lifecycle without graph/gallery/image assumptions.
