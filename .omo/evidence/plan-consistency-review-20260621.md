# Plan Consistency Review - 2026 Source Refresh

Scope: adversarial review of `.omo/drafts/cross-agent-image-harness.md` and `.omo/plans/cross-agent-image-harness.md`.

Status: plan-document hardening only. Do not enter `/speckit-*` until the user explicitly approves the next workflow stage.

## Verdict

The plan is correctly centered on a Cross-Agent LLM Execution Harness, but the review found three blocking consistency risks:

1. The plan used an executable-looking `Todos` section before `/speckit-taskstoissues`, conflicting with the user's global workflow.
2. The plan still gave image-domain implementation waves more detail than the Q37-locked `presentation_document_pack`.
3. The plan named current protocol surfaces correctly, but needed a sharper maturity policy: Agent Skills/package and deterministic CLI/state are the core; MCP, A2A, AG-UI, durable runtimes, and OpenTelemetry are optional or adapter-level ports unless the approved spec promotes one.

## 2026 Source Constraints

- OpenAI Codex Skills docs support packaged skills as reusable instructions/resources/scripts, reinforcing the Skill/package layer but not replacing the harness state contract: https://developers.openai.com/codex/skills
- MCP latest official specification page is versioned 2025-11-25 and frames MCP as a way to expose resources, prompts, and tools to LLM applications over JSON-RPC. MCP should remain tool/data interop, not the harness core: https://modelcontextprotocol.io/specification/2025-11-25
- MCP tools are model-controlled and the spec calls for visible tool exposure plus human-in-the-loop authorization for operations; this reinforces capability grants and brokered side effects: https://modelcontextprotocol.io/specification/2025-11-25/server/tools
- A2A v1.0 is for independent/opaque agents to communicate, exchange tasks, stream task state, and collaborate. It is an optional remote-agent port, not local tool execution: https://a2a-protocol.org/latest/specification/
- AG-UI is an event-based agent-user interaction layer. The plan should say AG-UI-inspired browser events unless strict wire compatibility is added by a later approved spec: https://docs.ag-ui.com/introduction and https://docs.ag-ui.com/concepts/architecture
- OpenTelemetry GenAI semantic conventions are now maintained in a GenAI-specific repository for GenAI clients, MCP, providers, spans, metrics, and events. The plan should align trace fields where possible, while preserving file-first manifests as the source of truth: https://github.com/open-telemetry/semantic-conventions-genai
- LangGraph interrupts persist state and resume from JSON-serializable human input, reinforcing durable human gates and targeted follow-up surfaces: https://docs.langchain.com/oss/python/langgraph/interrupts
- OpenAI Agents SDK tracing covers generations, tool calls, handoffs, guardrails, and custom events; it is evidence for observability but not a mandate to use the SDK as core: https://openai.github.io/openai-agents-python/tracing/
- AgentDojo and AgentDyn show prompt-injection risk in tool-using agents and dynamic multi-app environments. The plan must treat source materials, tool output, web pages, MCP results, and document content as adversarial until broker/inspection gates pass: https://arxiv.org/html/2406.13352v3 and https://arxiv.org/html/2602.03117
- OWASP Top 10 for Agentic Applications 2026 is the current agent-security baseline for autonomous systems that plan, act, and make decisions across complex workflows. The plan must include prompt-carrier controls, supply-chain controls, sandboxing, browser-surface hardening, and spend abuse controls: https://genai.owasp.org/resource/owasp-top-10-for-agentic-applications-for-2026/
- OWASP LLM Top 10 2025 explicitly includes prompt injection, supply chain, improper output handling, excessive agency, and unbounded consumption. The harness plan must make these first-class QA fixtures, not only general security principles: https://genai.owasp.org/llm-top-10/
- PresentBench 2026 makes slide generation a serious, rubric-grounded domain with source materials, full-deck generation, layout, completeness, correctness, and fidelity checks. This supports `presentation_document_pack` as a real second domain pack: https://arxiv.org/html/2603.07244v1
- PptxGenJS, python-pptx, python-docx, Open XML SDK, and PPTAgent show that source-native editable PPTX/DOCX generation is feasible; the Q37 acceptance proof must stay editable/source-native, not image-only: https://github.com/gitbrent/PptxGenJS, https://python-pptx.readthedocs.io/, https://github.com/python-openxml/python-docx, https://github.com/dotnet/Open-XML-SDK, https://github.com/icip-cas/pptagent
- ComfyUI routes and Civitai Comfy Nodes still support the image pack's import-first/brokered-provider path, but those details must not leak into harness core or the presentation pack: https://docs.comfy.org/development/comfyui-server/comms_routes and https://github.com/civitai/civitai-comfy-nodes

## Required Plan Fixes Applied

- Mark implementation tasks as candidate work areas only; `/speckit-taskstoissues` remains the only executable task source.
- Make Wave 0 conditional on explicit user approval for `/speckit-specify`.
- Add source-refresh gate before `/speckit-plan`.
- Add generic domain lifecycle states and pack-specific mappings for image and presentation/document packs.
- Add neutral core schemas: `domain_run_manifest.json`, `domain_plan_manifest.json`, `domain_artifact_manifest.json`, `review_session_manifest.json`, `human_action_manifest.json`, and `domain_lifecycle_trace.json`.
- Add `presentation_document_pack` schemas and QA artifacts with source-native editable outputs.
- Replace "zero human intervention" wording with agent-executed evidence plus explicit workflow/human gates.
- Clarify `ppt-master` as a reference pattern or optional adapter/worker, not hidden state or a required dependency.
- Add explicit prompt-carrier fixtures across Civitai metadata, Comfy labels, document comments, speaker notes, MCP descriptors, A2A agent cards, and provider/tool output.
- Require external pack supply-chain attestation: publisher identity, signed provenance, immutable digest, pinned dependencies, SBOM/AIBOM, vulnerability scan, install-script policy, revocation/quarantine, and update-diff review.
- Require worker sandboxing beyond out-of-process execution: deny ambient env, deny-by-default egress, read-only inputs, realpath checks, symlink/traversal denial, archive limits, resource limits, and child-process kill tree.
- Harden localhost UI against Host-header abuse, DNS rebinding, XSS, clickjacking, referrer leaks, and permissive CORS.
- Add credential-profile budget ledgers, rolling quotas, concurrent-job accounting, price-drift blocks, failure-rate circuit breakers, and an operator kill switch.
- Clarify AG-UI as inspired event surface unless future spec explicitly requires wire compatibility.
