# Expansion Log: Cross-Agent LLM Execution Harness Architecture 2026

Date: 2026-06-20

## Phase 0 Decomposition

Core question: What architecture should a 2026 Cross-Agent LLM Execution Harness use, including units, functions, modules, connections, lifecycle, security, observability, and extensibility?

Axes covered:
- Portable package surface: Agent Skills, `SKILL.md`, scripts/resources, host compatibility.
- Agent orchestration: handoffs, agents-as-tools, graph runtimes, state, memory, typed outputs.
- Inter-agent communication: A2A agent cards, tasks, messages, artifacts, streaming, push notifications, extensions.
- Tool/data protocol: MCP primitives, transports, authorization, security.
- Human/frontend protocol: AG-UI event streams, state sync, tools/handoff, user interaction.
- Durable execution: Temporal and graph runtime persistence, retries, long-running jobs, human waits.
- Framework architecture patterns: OpenAI Agents SDK, Google ADK, LangGraph, CrewAI, Pydantic AI, LlamaIndex Workflows.
- Observability: OpenAI tracing, OpenTelemetry GenAI/MCP semantic conventions, W3C Trace Context.
- Security/governance: OWASP Agentic AI Top 10 2026, MCP security best practices, NSA May 2026 MCP guidance.

Codebase relevant: yes, plan artifacts only. External: yes. Browsing: yes. Verification likely: no executable claim needed for this research addendum. Report requested: Markdown synthesis.

## Wave 1

Method: orchestrator-owned web research over official docs, primary specs, and major project documentation. Subagent swarm was not spawned because the active subagent tool policy only permits spawning when the user explicitly asks for subagents or delegation; this session still follows the ultraresearch journaling and synthesis shape.

Primary source families opened:
- OpenAI Agents SDK and Codex Skills.
- Agent Skills open standard.
- A2A latest specification and topic guides.
- MCP 2025-11-25 specification, authorization, transports, and security best practices.
- AG-UI introduction, architecture, and events.
- Google ADK agents, sessions, events, plugins, and workflows.
- LangGraph overview.
- CrewAI overview and flows.
- Pydantic AI agent and graph execution APIs.
- LlamaIndex Workflows.
- Temporal durable execution and AI agent posts.
- OpenTelemetry GenAI semantic conventions and W3C Trace Context.
- OWASP Agentic Applications Top 10 2026 and NSA MCP Security Design Considerations, May 2026.

## Leads Opened And Closed

- LEAD: A2A vs MCP vs AG-UI boundary. CLOSED: official A2A and AG-UI docs explicitly position A2A as agent-to-agent, MCP as agent-to-tools/data, and AG-UI as agent-to-user/front-end.
- LEAD: Durability shape for long-running image jobs. CLOSED: Temporal sources distinguish deterministic workflow orchestration from nondeterministic activities; LangGraph supplies graph persistence and human-in-loop primitives.
- LEAD: Portable package shape across agents. CLOSED: Codex Skills and Agent Skills standard define skill folders with `SKILL.md`, resources, and optional scripts; this supports the current Skill+CLI harness direction.
- LEAD: Observability standard. CLOSED: OpenAI Agents SDK tracing provides run-level trace events; OpenTelemetry GenAI conventions cover GenAI clients, MCP, and provider conventions; W3C Trace Context handles distributed trace propagation.
- LEAD: Security baseline. CLOSED: MCP spec/security docs, OWASP Top 10 2026, and NSA 2026 guidance all require identity, least privilege, session protection, tool output inspection, logging, and vulnerability inventory.

Convergence reason: all requested architecture axes have at least one primary/current source, and follow-up leads mapped to explicit architecture decisions for the current harness plan.

## User Decision Addendum

- Q12 option `a`: after each rendered batch, the harness stops at the localhost review gallery. LLM proposal cards are advisory; post-result accept, rerun, refine, branch, or stop requires saved creator action in `feedback.json` / `loop_state.json` before any continuation.
- Q13 option `a`: accepted results must write a full reproducibility package with `final_package_manifest.json`, final images, static review snapshot, workflows, graph patch diff, prompt/settings, provenance, cost/budget summary, rights/license notes, and checksums.
- Q14 option `a`: accepted results become reusable project memory only through explicit creator opt-in; `project_memory_manifest.json` stores managed references to final packages with selected reuse dimensions, source hash, rights/reuse scope, creator-visible label, and deletion support.
- Q15 option `a`: saved project memory profiles appear as creator-visible cards in the next workspace `initial_brief`; only selected cards apply to that workspace, and no memory is silently auto-applied.
- Q16 option `a`: cross-agent continuation uses a clean handoff package with manifests, selected artifacts, review/final pages, selected memory refs, current status, and evidence pointers; it excludes chat transcripts, raw secrets, and hidden browser state.
- Q17 option `a`: clean handoff refreshes only at stable checkpoints: initial brief submitted, review ready, feedback/action saved, and final package created.
- Q18 option `a`: a resumed agent/session first shows a "continue this work?" screen with current status, next action, budget remaining, selected memory refs, and blockers; no resumed spend or job submission until human confirmation is saved.
- Q19 option `a`: image generation ships as built-in reference domain pack 1, but `domain_pack_manifest.json` is a hard boundary and core may call Civitai/Comfy/image behavior only through generic domain-pack interfaces.
- Q20 option `a`: future non-image domain packs use manifest-first allowlist registration; built-in packs are bundled, and external packs load only after install/register, schema validation, version/source recording, security inventory, and workspace/user-config enablement.
- Q21 option `a`: registered packs use least-privilege capability grants; install/register and workspace enablement show human-readable permission groups, grants are stored in registry/config, workspace settings can narrow them, and undeclared capabilities are denied.
- Q22 option `a`: external domain-pack executable code runs out-of-process through a JSON IPC/CLI worker contract; built-in packs satisfy the same interface, and workers receive only effective granted env/file/provider/budget/memory scopes.
- Q23 option `a`: worker side effects are brokered through core capability brokers; workers emit typed side-effect requests and core performs approved provider/network/file/budget/memory/tool/agent operations with scope, budget, credential, trace, output-inspection, and evidence checks.
- Q24 option `a`: every brokered side-effect request requires `idempotency_key`, `retry_policy`, `timeout`, `cancel_token`, lifecycle events, and terminal outcome; core deduplicates idempotency keys and never blindly retries non-idempotent effects.
- Q25 option `a`: core scheduler owns durable provider-job leases and polling; broker execution may return `pending`, and manifests record provider job refs, resume handles, lease owner, polling cadence, cancel token, and terminal outcome while workers stay stateless.
- Q26 option `a`: use single-writer workspace/job leases with heartbeat and TTL; only active lease holders can submit, poll, cancel, continue, or finalize, stale leases recover with event-log evidence, and non-holders get read-only status/resume surfaces.
- Q27 option `a`: first implementation uses file-first portable workspace state with append-only JSONL event log, JSON manifests, atomic writes, file locks, and checksums as source of truth; SQLite/Temporal adapters are optional future export-compatible layers.
- Q28 option `a`: append-only event logs remain the audit source; periodic snapshots record schema version, covered event range, checksum chain, and migration-ledger refs; compaction cannot silently delete original events before final-package or handoff retention allows it.
- Q29 option `a`: localhost intake, review, and resume UI sessions bind loopback-only by default, use random one-time session-token URLs with short TTL, require CSRF plus origin checks for mutating actions, keep static artifacts read-only unless action-scoped, and revalidate CLI/state leases before saving actions.
- Q30 option `a`: use a local actor identity/trust registry. Each host agent gets a stable `agent_id`, fresh `agent_session_id`, human-visible label, host/runtime/version metadata, optional local signing key or fingerprint, and workspace trust status. Unknown agents may inspect read-only status/handoff but need explicit trust before lease acquisition, budget-spending mutation, project-memory writes, handoff mutation, resume confirmation, or finalization.
- Q31 option `a`: use retention classes with a creator-facing cleanup screen. Final packages and opt-in project memory persist until explicit deletion; handoff/status packages persist until superseded or finalized; event logs and snapshots persist until retention rules allow compaction; raw intermediates, temp renders, and provider caches can be pruned only after reproducibility and handoff proofs exist; deletions write tombstone events and raw secrets are never retained as cleanup evidence.
- Q32 option `a`: use host-agent-first LLM decisions with an optional brokered LLM provider adapter. Host agents write typed `decision_manifest.json` and `proposal_manifest.json`; harness core stays deterministic and consumes validated manifests; any harness-owned model call is a brokered `llm_model_call` side effect with provider profile, budget, input hash, model id, output schema, credential ref, trace, actor/trust, and `llm_call_manifest.json` evidence.
- Q33 option `b`: use live-provider-first execution and QA when credentials exist. Credentialed runs and executable demos hit the real provider path after budget, actor-trust, lease, idempotency, broker, and cleanup gates pass; mock/offline routes are explicit fallback lanes for no credentials, forced offline mode, deterministic unit tests, and failure fixtures.
- Q34 option `a`: use provider-profile hard caps for live-provider spend. Credential profiles declare max live smoke spend, max jobs, wall-clock cap, and cleanup requirements; missing or lower caps block live execution and open setup/budget prompt rather than relying on workspace budget alone.
- Q35 option `a`: default CI and PR checks stay mock/offline; live provider checks require manual, scheduled, label-gated, or env-gated triggers with provider-profile cap evidence and cleanup evidence. Local executable demos remain live-first.
- Q36 option `b`: first implementation includes a real second non-image production domain pack, not just a fixture or static boundary test. The exact domain is selected in Q37 and must prove generic lifecycle artifacts outside image generation.

## Required Artifact Names Carried Forward

- `architecture_manifest.json`
- `domain_pack_registry.json`
- `domain_pack_worker_manifest.json`
- `capability_broker_manifest.json`
- `side_effect_request.json`
- `side_effect_result.json`
- broker lifecycle fields: `idempotency_key`, `retry_policy`, `timeout`, `cancel_token`, terminal outcome, retry attempts, cancellation metadata
- provider job lifecycle fields: `provider_job_ref`, `provider_resume_handle`, `lease_owner`, `polling_cadence`, `next_poll_after`, `cancel_token`, `terminal_outcome`
- scheduler lease fields: `lease_id`, `scope`, `holder_agent_id`, `holder_session_id`, `heartbeat_at`, `ttl_seconds`, `expires_at`, `allowed_operations`, `recovery_reason`
- file-first state fields: `event_log.jsonl`, manifest paths, lock metadata, temp-file write protocol, previous/current hash, checksum chain, corruption status, adapter export metadata
- snapshot/migration fields: `snapshot_manifest.json`, `migration_ledger.json`, `schema_version`, covered event range, first/last event ids, snapshot hash, source/target schema versions, migration id, migration evidence path, retention/compaction status
- localhost UI session fields: `ui_session.json`, surface, bind host, route, token hash, issued/expires/consumed timestamps, CSRF hash, allowed origins, action scopes, static read-only flag, lease-check result
- actor identity/trust fields: `agent_identity.json`, `agent_trust_registry.json`, `agent_id`, `agent_session_id`, human-visible label, host name/family, runtime name/version, identity source, optional public key/fingerprint, trust state, granted operations, workspace id, grant source, approver, approved/revoked timestamps, last seen time, actor evidence path
- retention/cleanup fields: `retention_policy.json`, `cleanup_plan.json`, `deletion_tombstone.json` or tombstone event, retention class, proof requirement, proof refs, cleanup action, creator-visible label, explicit delete flag, superseded/finalized ref, actor identity/trust state, artifact hash/path, deletion reason, deleted_at
- LLM decision fields: `decision_manifest.json`, `proposal_manifest.json`, `llm_call_manifest.json`, decision id/type, selected action, advisory alternatives, rationale summary, input refs/hash, host agent id/session, provider id/profile, model id, output schema id, output hash, budget estimate/consumed, credential_ref, trace/idempotency, actor trust state, evidence path
- live-provider execution fields: `run_mode`, `live_provider_default`, `fallback_reason`, `provider_job_ref`, `provider_terminal_status`, `credential_ref`, `budget_envelope`, `spend_estimate`, `spend_consumed`, `actor_trust_state`, `lease_id`, `cleanup_proof_path`, `provider_evidence_path`
- provider-profile cap fields: `max_live_smoke_spend`, `max_live_jobs`, `max_live_wall_clock_seconds`, `cleanup_requirements`, `cap_snapshot`, `cap_check_result`, `cap_evidence_path`, `cap_updated_by`, `cap_updated_at`
- CI/live-check fields: `check_mode`, `default_ci_live_allowed=false`, `live_check_gate_source`, `live_check_trigger`, `provider_cap_snapshot`, `cleanup_evidence_path`, `provider_terminal_status`, `fallback_reason`, `evidence_path`
- second-domain proof fields: `second_domain_pack_id`, `second_domain_manifest_path`, `second_domain_artifact_manifest`, `second_domain_review_session`, `second_domain_final_package`, `second_domain_handoff_evidence`, `image_assumption_rejection_evidence`
- `event_log.jsonl`
- `adapter_registry.json`
- `trace_manifest.json`
- `security_inventory.json`
- `final_package_manifest.json`
- `project_memory_manifest.json`
- `handoff_manifest.json`
