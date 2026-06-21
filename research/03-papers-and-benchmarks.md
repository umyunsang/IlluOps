# Papers And Benchmarks

Date: 2026-06-20

## Highest-Signal Reading List

### Harnessing the Harnesses: Survey of LLM Agent Harness Landscape

Source: [preprint](https://www.preprints.org/manuscript/202604.0428), [Awesome Agent Harness](https://github.com/Gloriaameng/Awesome-Agent-Harness)

Why it matters:

- Frames the harness as a primary determinant of agent performance.
- Treats harness design as environment, tools, control flow, state, logging, and verification, not just prompting.
- Useful taxonomy for designing your own harness modules.

What to borrow:

- Explicit environment model.
- Tool governance.
- State and logging.
- Verification as a harness component.
- Separation between agent policy and execution substrate.

### Terminal-Bench 2.0

Source: [arXiv HTML](https://arxiv.org/html/2601.11868v1)

Why it matters:

- Evaluates terminal agents in containerized environments.
- Tasks include instructions, tests, and reference solutions.
- Very close to Codex and Claude Code execution reality.

What to borrow:

- Containerized task format.
- Hidden tests plus reference solution.
- Pass/fail evaluation independent of chat transcript.
- Task suite for shell, filesystem, package manager, and debugging behavior.

### SWE-bench

Source: [SWE-bench](https://www.swebench.com/), [GitHub](https://github.com/SWE-bench/SWE-bench)

Why it matters:

- Real GitHub issues as agent tasks.
- Docker-based evaluation harness.
- Common yardstick for coding agents.

What to borrow:

- Issue-to-patch task packaging.
- Reproducible environment setup.
- Test-based scoring.
- Separation between generation and evaluation.

### OpenHands

Source: [OpenReview](https://openreview.net/forum?id=OJd3ayDDoF), [GitHub](https://github.com/OpenHands/OpenHands), [runtime docs](https://docs.openhands.dev/openhands/usage/architecture/runtime)

Why it matters:

- Full agent developer platform with code, shell, browser, and sandboxed runtime.
- Strong reference for action/observation loops and runtime isolation.

What to borrow:

- Runtime abstraction.
- Action and observation logging.
- Browser plus terminal workflow.
- Sandboxed execution model.

### Context Engineering Survey

Source: [arXiv HTML](https://arxiv.org/html/2507.13334v1)

Why it matters:

- Reframes prompting as only one part of context construction.
- Covers retrieval, memory, tool-integrated context, and multi-agent context management.

What to borrow:

- Treat context as a managed resource.
- Use structured state and retrieval instead of stuffing everything into `SKILL.md`.
- Keep references out of the main skill file until needed.

### Agentic Context Engineering

Source: [arXiv HTML](https://arxiv.org/html/2510.04618v1)

Why it matters:

- Focuses on adaptive context for long-running agents.
- Useful when a harness needs multiple runs, restarts, or evolving task state.

What to borrow:

- Reusable context artifacts.
- Context update policies.
- Latency-aware context selection.

### Survey on Evaluation of LLM-based Agents

Source: [arXiv HTML](https://arxiv.org/html/2503.16416v2)

Why it matters:

- Evaluation should cover sequential behavior, cost, robustness, safety, and generalization.
- Single final-answer scoring is not enough for execution harnesses.

What to borrow:

- Multi-axis evaluation.
- Robustness tests.
- Safety tests.
- Cost and latency tracking.

### Agent Protocol Survey

Source: [arXiv HTML](https://arxiv.org/html/2504.16736v2)

Why it matters:

- Distinguishes context-oriented protocols from inter-agent protocols.
- Helps avoid confusing MCP and A2A.

What to borrow:

- MCP for tool/context integration.
- A2A for agent-to-agent task exchange.
- Layered protocol architecture.

### SandboxEscapeBench

Source: [arXiv HTML](https://arxiv.org/html/2603.02277v1)

Why it matters:

- LLM agents with code and shell tools can attempt or accidentally create sandbox escape paths.
- Evaluation must include hostile or compromised tasks.

What to borrow:

- Sandbox escape regression tests.
- Network and filesystem boundary tests.
- Container hardening checks.

### MCP Threat Model And Tool Poisoning

Sources:

- [MCP threat model](https://arxiv.org/html/2603.22489v1)
- [Invariant Labs MCP tool poisoning](https://invariantlabs.ai/blog/mcp-security-notification-tool-poisoning-attacks)

Why it matters:

- Tool descriptions and server metadata are model-readable instruction surfaces.
- Malicious MCP servers can smuggle instructions into tool metadata.

What to borrow:

- Treat MCP descriptions as untrusted.
- Pin and review MCP servers.
- Validate tool outputs.
- Keep tool annotations advisory, not trusted.
- Use explicit user consent for risky actions.

## Benchmark Suite For This Harness

Build your own benchmark before writing production integrations.

### Task Types

- Fresh install through `npx skills add`.
- Skill activation by explicit mention.
- Source import and normalization.
- Multi-phase run with a blocking gate.
- Interrupted run and resume.
- Invalid source file recovery.
- Artifact validation failure and repair.
- MCP unavailable fallback to CLI.
- Network-off evaluation run.
- Prompt injection inside a source document.
- Tool poisoning simulation.
- Secret exfiltration attempt.
- Path traversal attempt.
- Sandbox escape probe.

### Metrics

- Task success rate.
- Validation pass rate.
- Gate compliance.
- Recovery success.
- Cost and latency.
- Number of unsafe tool calls blocked.
- Artifact correctness.
- Human review burden.
- Reproducibility from clean checkout.
