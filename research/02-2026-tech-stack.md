# 2026 Tech Stack Recommendation

Date: 2026-06-20

## Recommended Stack

### Packaging

- Primary distribution: GitHub repository installable through `npx skills add`.
- Skill layout: Agent Skills standard with `SKILL.md`, `scripts/`, `references/`, `assets/`, and `templates/`.
- Locking: include a lock file or release tag guidance for repeatable installs.
- Versioning: semantic versions plus changelog entries for skill behavior, script behavior, schemas, and security policy.

### Languages

- TypeScript for CLI, MCP server, schemas, and cross-platform developer tooling.
- Python for ML-heavy, document-heavy, notebook-heavy, or scientific processing.
- Shell only for tiny wrappers. Avoid business logic in shell.

Reasoning:

TypeScript maps well to npm and `npx skills add` workflows. Python remains practical for document conversion, data processing, benchmark harnesses, and ML ecosystem tooling. A serious harness may use both, but each deterministic operation should have one owner.

### CLI

The CLI should be the stable execution spine:

```text
harness init
harness import
harness plan
harness execute
harness validate
harness export
harness eval
harness doctor
```

The skill calls the CLI. MCP calls the same CLI or its underlying library. Tests call the same CLI. This prevents the skill, MCP server, and evaluation harness from drifting apart.

### Schemas

Use schemas everywhere:

- `harness_state.json`
- `project_manifest.json`
- `artifact_manifest.json`
- `tool_policy.json`
- `eval_result.json`
- MCP input and output schemas

The model can write prose, but the harness should persist machine-checkable state.

### State And Orchestration

Start simple:

- File-backed state for the first version.
- Append-only event log for evidence.
- Recovery command that reconstructs current state from disk.

Add LangGraph only when you need:

- Long-running graph execution.
- Checkpoint and replay.
- Human interrupts and resume.
- Branching plans.
- Multiple tool loops with durable state.

Add OpenAI Agents SDK or Claude Agent SDK when:

- You are building a hosted app or service around the harness.
- Your app owns model calls, tracing, guardrails, and approvals.
- Codex or Claude Code becomes one tool inside a larger orchestrator.

### MCP

Implement MCP after the CLI stabilizes.

Recommended MCP tools:

```text
init_project(input) -> project_manifest
import_sources(input) -> source_manifest
read_state(input) -> harness_state
validate_project(input) -> validation_report
run_step(input) -> step_result
export_artifacts(input) -> export_manifest
run_eval(input) -> eval_report
```

Recommended MCP resources:

```text
harness://project/{id}/state
harness://project/{id}/artifact/{name}
harness://reference/{topic}
```

Recommended MCP prompts:

```text
plan_run
review_artifact
recover_interrupted_run
```

### A2A

Add A2A only if cross-agent collaboration is required.

Recommended first A2A surface:

- Agent card with supported tasks and artifact types.
- `send message` for task start and updates.
- Streaming status for long-running tasks.
- Artifact return for completed outputs.

Treat incoming agent cards and remote task messages as untrusted input.

### Runtime And Sandbox

Minimum runtime controls:

- Project-local working directory.
- Allowlisted commands.
- Explicit network policy.
- Secrets only through environment variables or host secret manager.
- No secret material in logs or artifacts.
- Artifact path validation to prevent writes outside the project.

Stronger controls:

- Docker or OCI container per evaluation task.
- Read-only source mounts where possible.
- Separate network-off evaluation mode.
- File diff review before export.
- Sandbox escape regression tests.

### Observability

Capture:

- Model-facing decisions.
- CLI commands and exit codes.
- State transitions.
- Artifact hashes.
- Validation reports.
- Approval points.
- Security decisions.

Keep logs useful to a human reviewer. A harness that cannot explain what happened is not production-ready.

## MVP Stack

For the first build, do this:

```text
Agent Skill package
TypeScript CLI
JSON schemas
File-backed state
Python helper scripts only where needed
Eval fixtures
Security policy file
Optional MCP server after CLI is stable
```

Do not start with:

- Multi-agent orchestration.
- A2A.
- Hosted UI.
- Complex memory.
- Broad remote tool access.

Those are expansion layers, not the foundation.

## Production Stack

After MVP proves the workflow:

```text
Skill package
CLI library core
MCP server
LangGraph or SDK-based orchestrator for hosted mode
Containerized eval runtime
Trace store
Policy engine
Release signing or pinned install metadata
A2A endpoint if external agent collaboration matters
```

## Compatibility Matrix

| Layer | Codex | Claude Code | Other MCP Hosts | External Agents |
|---|---:|---:|---:|---:|
| Agent Skill | Primary | Primary | Indirect | Indirect |
| CLI scripts | Primary | Primary | Via tool wrapper | Via task worker |
| MCP server | Primary | Primary | Primary | Indirect |
| A2A endpoint | Optional | Optional | Optional | Primary |
| Hosted SDK orchestrator | Optional | Optional | Optional | Optional |
