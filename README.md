# IlluOps Research 2026

Date: 2026-06-20

This folder is a research and design workspace for **IlluOps**, a creator-facing Cross-Agent LLM Execution Harness that can be used from Codex and Claude Code in the same practical style as `ppt-master`.

IlluOps is a ComfyUI-powered companion layer for creators making illustrations, comics, and animation frames. It is not a fork of ComfyUI and is not affiliated with Comfy Org.

## Bottom Line

The locked product name is:

`IlluOps`

The package and CLI name is:

`illuops` (for example, `npx illuops`)

In 2026 terms, IlluOps means a packaged workflow that exposes:

- An Agent Skill front door: `SKILL.md`, references, scripts, assets, and examples.
- A deterministic execution core: CLIs, validators, project state, import/export commands, and quality gates.
- Optional MCP tools: for structured tool calls from Codex, Claude Code, and other MCP hosts.
- Optional A2A endpoint: for collaborating with other opaque agents or agent products.
- Evaluation and security harnesses: benchmark tasks, sandbox checks, trace logs, and supply-chain controls.
- A visual generation control stack: model routing, LoRA management, ControlNet and adapter control, segmentation and mask tools, guidance/sampler policy, and image/video/comic export gates.

`ppt-master` is a strong reference because it is not just a prompt. It is a skill package with a serial workflow, project initialization, script-backed transforms, live preview, quality checks, and export gates.

For ComfyUI, the right strategy is not to port or reimplement ComfyUI. Keep ComfyUI as the graph execution engine. Build a higher-level LLM harness that discovers the local ComfyUI capability surface, compiles creator intent into validated workflow graphs, executes them through the ComfyUI API, observes outputs, and iterates with explicit quality gates.

## Files

- `LICENSE`: Apache-2.0 license for the core repository.
- `LICENSES/README.md`: license boundary for core code, future ComfyUI in-process extensions, and third-party creative assets.
- `research/00-source-index.md`: source map with official docs, papers, repos, and local evidence.
- `research/01-architecture-synthesis.md`: recommended architecture and compatibility model.
- `research/02-2026-tech-stack.md`: concrete stack choices for a 2026 build.
- `research/03-papers-and-benchmarks.md`: papers, benchmarks, and what to borrow from each.
- `research/04-security-threat-model.md`: threat model and hardening checklist.
- `research/05-build-roadmap.md`: phased roadmap from research to installable skill package.
- `research/06-image-generation-model-trends-2026.md`: current image generation model trends and what they imply for the harness.
- `research/07-computer-vision-control-stack.md`: LoRA, ControlNet, SAM, SAG, adapters, geometry, and post-processing control stack.
- `research/08-comfyui-full-control-analysis.md`: whether and how an LLM can control ComfyUI's full node and workflow surface.
- `research/09-creator-intent-graph-roadmap.md`: creator-facing intent model and roadmap toward video and comics.
- `research/10-recent-paper-watchlist-2026.md`: recent papers and benchmarks to track while building.
- `scaffold/SKILL.md`: draft Agent Skill entrypoint template.
- `scaffold/image-generation-harness-SKILL.md`: image-specific Agent Skill scaffold.
- `scaffold/AGENTS.md`: draft repository instruction template.
- `scaffold/package-shape.md`: proposed package layout.
- `evidence/local-findings.md`: local commands, installed tool observations, and repository metadata captured in this session.
- `evidence/image-generation-findings.md`: image generation, ComfyUI, model, and CV-control findings captured in this session.

## Recommended Product Shape

Build the first version as a skill-first repository:

```text
illuops/
  AGENTS.md
  skills/illuops/SKILL.md
  skills/illuops/references/
  skills/illuops/scripts/
  skills/illuops/templates/
  skills/illuops/workflows/
  docs/
  examples/
  evals/
```

For the image-generation target, add this layer:

```text
illuops-image-pack/
  skills/illuops/references/model-cards/
  skills/illuops/references/workflow-recipes/
  skills/illuops/scripts/capability-index
  skills/illuops/scripts/workflow-compile
  skills/illuops/scripts/workflow-validate
  skills/illuops/scripts/comfy-execute
  skills/illuops/scripts/visual-evaluate
  skills/illuops/templates/comfy-workflows/
  skills/illuops/templates/control-stacks/
  projects/
  evals/
```

Then add MCP and A2A only after the skill and CLI behavior are stable. Skill compatibility gives Codex and Claude Code the simplest shared install surface. MCP gives structured tools. A2A gives cross-agent interoperability.

## Design Principle

Do not build an "agent framework demo." Build a reproducible workflow product:

1. Clear activation conditions in `SKILL.md`.
2. Explicit gates and stop points.
3. Project workspace with durable state.
4. Deterministic scripts for file transforms and validation.
5. Human-readable artifacts after every phase.
6. Evaluation tasks that prove the harness works outside a happy-path chat.
7. Security controls for shell, network, secrets, prompt injection, tool poisoning, and supply chain.

## ComfyUI Control Thesis

An LLM can control ComfyUI's pipeline surface if the harness treats ComfyUI as a typed graph runtime:

1. Discover nodes, models, templates, and system limits from ComfyUI APIs.
2. Compile creator intent into an intermediate graph plan.
3. Lower the graph plan into ComfyUI API-format workflow JSON.
4. Validate node classes, inputs, model files, and resource budgets before execution.
5. Queue the workflow, monitor WebSocket progress, collect history and images.
6. Run visual and semantic checks before exporting or iterating.

The LLM should not directly free-write arbitrary workflows without a compiler, schema validation, and risk policy.
