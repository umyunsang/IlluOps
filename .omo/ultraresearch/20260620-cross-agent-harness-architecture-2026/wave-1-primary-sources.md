# Wave 1 Primary Sources: Cross-Agent Harness Architecture

Date accessed: 2026-06-20

## Portable Package Surface

- OpenAI Codex Agent Skills: https://developers.openai.com/codex/skills
  - Use: Codex frames skills as packages of instructions, resources, and optional scripts, built on the open Agent Skills standard.
  - Architecture implication: the harness can be a portable skill package whose deterministic scripts/CLI do the work while agent-specific hosts only load guidance.
- Agent Skills standard: https://github.com/agentskills/agentskills
  - Use: defines the lightweight folder plus `SKILL.md` pattern and optional bundled scripts/templates/resources.
  - Architecture implication: put host-neutral instructions in `SKILL.md`; put deterministic behavior in CLI/scripts; keep per-host adapters thin.

## Agent Orchestration And Runtime

- OpenAI Agents SDK guide: https://developers.openai.com/api/docs/guides/agents
  - Use: agents are applications that plan, call tools, collaborate across specialists, and keep enough state for multi-step work; SDK is for owned orchestration, tool execution, approvals, and state.
  - Architecture implication: the harness should own orchestration/state rather than depend on a host chat loop.
- OpenAI Agents SDK handoffs: https://openai.github.io/openai-agents-python/handoffs/
  - Use: handoffs delegate tasks to specialized agents and are represented to the LLM as tools.
  - Architecture implication: internal specialist lanes can be modeled as typed handoff actions, but the durable contract remains the harness state machine.
- OpenAI Agents SDK results: https://openai.github.io/openai-agents-python/results/
  - Use: handoffs can change final output types; run items carry agent/tool/handoff/approval metadata.
  - Architecture implication: result manifests must not assume one static output type; retain run items and approval metadata.
- OpenAI Agents SDK tracing: https://github.com/openai/openai-agents-python/blob/main/docs/tracing.md
  - Use: tracing records LLM generations, tool calls, handoffs, guardrails, and custom events.
  - Architecture implication: the harness evidence ledger should mirror this span model for every agent/tool/backend transition.

## Agent-To-Agent Protocol

- A2A latest specification: https://a2a-protocol.org/latest/specification/
  - Use: a complex agent response can be a Task with status and artifact updates; clients can get task state including artifacts/history.
  - Architecture implication: external agents should be connected through a task/artifact envelope, not hidden as raw tool calls.
- A2A streaming and async: https://a2a-protocol.org/latest/topics/streaming-and-async/
  - Use: supports SSE for long-running tasks, status updates, artifact updates, resubscription, and push notifications for disconnected scenarios.
  - Architecture implication: long image jobs need streamable progress plus recoverable polling or push mechanisms.
- A2A overview: https://a2a-protocol.org/latest/topics/what-is-a2a/
  - Use: A2A solves bespoke inter-agent integration and is complementary to MCP.
  - Architecture implication: A2A is the remote-agent port, not the tool/data port.
- A2A extensions: https://a2a-protocol.org/latest/topics/extensions/
  - Use: extensions add profile data, requirements, RPC methods, or state-machine overlays without changing core data structures.
  - Architecture implication: image-generation-specific state such as `generating-image` or review-gallery metadata can be an extension/profile, not a forked protocol.

## Tool And Data Protocol

- MCP transports: https://modelcontextprotocol.io/specification/2025-11-25/basic/transports
  - Use: MCP uses JSON-RPC over stdio or Streamable HTTP.
  - Architecture implication: MCP belongs behind an adapter boundary with explicit transport metadata and cancellation/session handling.
- MCP authorization: https://modelcontextprotocol.io/specification/2025-11-25/basic/authorization
  - Use: MCP HTTP authorization uses OAuth 2.1 patterns, protected resource metadata, and authorization server discovery.
  - Architecture implication: no raw tokens in workspace artifacts; store credential references and discovered scopes/providers.
- MCP security best practices: https://modelcontextprotocol.io/docs/tutorials/security/security_best_practices
  - Use: documents confused deputy, token passthrough, SSRF, session hijacking, local server compromise, and scope minimization.
  - Architecture implication: enforce least privilege per tool, block token passthrough, and scan artifacts/logs for secrets.

## Agent-To-User Protocol

- AG-UI introduction: https://docs.ag-ui.com/introduction
  - Use: AG-UI is an event-based, bidirectional protocol between agentic frontends and backends; it explicitly distinguishes AG-UI, MCP, and A2A layers.
  - Architecture implication: localhost intake and review gallery should use an AG-UI-like event boundary, not terminal prompts.
- AG-UI architecture: https://docs.ag-ui.com/concepts/architecture
  - Use: client-server architecture with user app, AG-UI client, backend agents, optional secure proxy, event-driven communication, and transport flexibility.
  - Architecture implication: local browser UI can speak a stable event stream while the harness stays host-neutral.
- AG-UI events: https://docs.ag-ui.com/concepts/events
  - Use: events cover lifecycle, text, tool calls, state management, activity, and custom events.
  - Architecture implication: intake/review flows should emit lifecycle/state/tool/proposal events and persist them to session JSON.

## Framework And Workflow Runtimes

- LangGraph overview: https://docs.langchain.com/oss/python/langgraph/overview
  - Use: focuses on durable execution, streaming, human-in-the-loop, persistence, and memory.
  - Architecture implication: graph runtimes validate the need for explicit state nodes, checkpointing, and human gates.
- Google ADK agents: https://adk.dev/agents/
  - Use: an agent is a self-contained execution unit; growing tasks become workflows with multiple agents or executable nodes.
  - Architecture implication: decompose monolithic image generation into workflow nodes once complexity exceeds context/tool limits.
- Google ADK sessions: https://adk.dev/sessions/
  - Use: distinguishes Session, State, and Memory.
  - Architecture implication: keep current run state, cross-session preferences, and durable artifacts separate.
- Google ADK events: https://adk.dev/events/
  - Use: immutable events are the fundamental information flow and capture user messages, tool calls, state changes, control signals, and errors.
  - Architecture implication: the harness should be event-sourced enough that any run can be replayed or audited.
- Google ADK plugins: https://adk.dev/plugins/
  - Use: plugins hook lifecycle stages for logging, policy enforcement, metrics, and caching.
  - Architecture implication: use lifecycle hooks for budget gates, credential gates, policy checks, and observability.
- CrewAI docs: https://docs.crewai.com/
  - Use: crews/flows combine agents, guardrails, memory, knowledge, observability, RBAC, and production automations.
  - Architecture implication: distinguish autonomous agent collaboration from deterministic flow state.
- CrewAI flows: https://docs.crewai.com/en/concepts/flows
  - Use: flows are event-driven, manage/share state, and support branching/loops.
  - Architecture implication: the proposal/feedback loop should be a flow, not a one-off callback.
- Pydantic AI agents: https://pydantic.dev/docs/ai/core-concepts/agent/
  - Use: agents can control an application/component; multiple agents can interact in complex workflows.
  - Architecture implication: type-safe agent containers match schema-first CLI commands.
- Pydantic AI graph execution API: https://pydantic.dev/docs/ai/api/pydantic-ai/agent/
  - Use: internal agent graph can be iterated node-by-node and streamed.
  - Architecture implication: expose internal node/run events in the harness instead of only final text.
- LlamaIndex Workflows: https://developers.llamaindex.ai/python/llamaagents/workflows/
  - Use: workflows are event-driven, step-based; steps receive events and emit events.
  - Architecture implication: design modules around event in/out contracts.

## Durable Execution

- Temporal platform: https://temporal.io/
  - Use: workflows capture state and resume from failures.
  - Architecture implication: long-running paid image jobs should have resumable lifecycle states.
- Temporal dynamic AI agents: https://temporal.io/blog/of-course-you-can-build-dynamic-ai-agents-with-temporal
  - Use: workflow orchestration code is deterministic; nondeterministic LLM/tool work belongs in activities.
  - Architecture implication: the harness control plane should be deterministic, while LLM decisions and external generation are recorded activities.
- Temporal plus Pydantic AI: https://temporal.io/blog/build-durable-ai-agents-pydantic-ai-and-temporal
  - Use: durable execution supports API failures, restarts, deploys, long-running async, and human-in-the-loop interactions.
  - Architecture implication: budget waits, credential setup, and creator review can pause for hours without losing state.

## Observability

- OpenTelemetry GenAI semantic conventions: https://github.com/open-telemetry/semantic-conventions-genai
  - Use: covers spans, metrics, and events for GenAI clients, MCP, and provider-specific conventions.
  - Architecture implication: define `trace_id`, `run_id`, `task_id`, model/tool/backend spans, token/cost metadata, and artifact hashes.
- W3C Trace Context: https://www.w3.org/TR/trace-context/
  - Use: standardizes trace context propagation across distributed systems.
  - Architecture implication: propagate `traceparent` or an equivalent trace field through A2A, MCP, HTTP adapters, and artifact manifests.

## Security And Governance

- OWASP Top 10 for Agentic Applications 2026: https://genai.owasp.org/resource/owasp-top-10-for-agentic-applications-for-2026/
  - Use: peer-reviewed framework for critical risks in autonomous agentic systems.
  - Architecture implication: policy gates and adversarial tests are first-class modules, not later hardening.
- NSA MCP Security Design Considerations, May 2026: https://www.nsa.gov/Portals/75/documents/Cybersecurity/CSI_MCP_SECURITY.pdf
  - Use: warns that MCP security requires lifecycle-level design, runtime scheduling, service integration, monitoring, access control, tool output inspection, logging, vulnerability tracking, and MCP server inventory.
  - Architecture implication: the harness must inventory tool servers/adapters, inspect outputs before downstream use, and keep audit-grade logs.

## Artifact Names Derived From Sources

- `architecture_manifest.json`: unit/function/module/connection boundaries.
- `event_log.jsonl`: immutable lifecycle and state-transition events.
- `adapter_registry.json`: direct provider, MCP, A2A, and local UI adapter inventory.
- `trace_manifest.json`: trace IDs, span edges, and artifact hashes.
- `security_inventory.json`: scopes, auth modes, output-inspection policies, SSRF/webhook posture, and vulnerability watch.
