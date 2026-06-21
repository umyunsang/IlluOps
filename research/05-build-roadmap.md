# Build Roadmap

Date: 2026-06-20

This roadmap stays within the current instruction boundary: no production code is written before a real spec is approved.

## Phase 0: Product Definition

Decide the harness domain.

Examples:

- Document generation harness.
- Code migration harness.
- Data analysis harness.
- Browser workflow harness.
- Research-to-report harness.
- Presentation harness.
- CI repair harness.

Deliverables:

- One-paragraph product definition.
- Target users.
- Supported inputs.
- Supported outputs.
- Success criteria.
- Non-goals.

## Phase 1: Skill-First Spec

Write the first spec before code:

- Skill activation conditions.
- Main pipeline.
- Blocking gates.
- State model.
- Artifact model.
- Script commands.
- Validation rules.
- Security policy.
- Eval cases.

Deliverables:

- `docs/spec.md`
- `skills/<name>/SKILL.md` draft
- `references/state-schema.md`
- `references/security-policy.md`
- `evals/README.md`

## Phase 2: Deterministic CLI

Implement only the mechanical spine:

- `init`
- `import`
- `state`
- `validate`
- `export`
- `doctor`

Do not add autonomous planning yet. Prove the project structure and validators first.

## Phase 3: Skill Integration

Make Codex and Claude Code run the workflow through the skill:

- Install via `npx skills add`.
- Verify explicit invocation.
- Verify implicit invocation.
- Verify clean-CWD run.
- Verify recovery after context compression or new chat.

## Phase 4: Evaluation Harness

Add task fixtures:

- Happy path.
- Invalid input.
- Interrupted run.
- Prompt injection.
- Tool poisoning.
- Secret leak attempt.
- Path traversal attempt.
- Network-off mode.

Pass condition:

- The harness can be evaluated without relying on chat memory.

## Phase 5: MCP Server

Only after CLI behavior is stable:

- Add MCP tools over the same core commands.
- Add resources for state and artifacts.
- Add output schemas.
- Add output-size limits.
- Add host-specific setup docs for Codex and Claude Code.

## Phase 6: Hosted Orchestration

Only if needed:

- Add OpenAI Agents SDK, Claude Agent SDK, or LangGraph.
- Use it for hosted workflows, trace collection, approval UI, or durable multi-step state.
- Keep the skill and CLI usable without the hosted service.

## Phase 7: A2A

Only when external agent interoperability matters:

- Publish agent card.
- Implement task start, status, streaming, and artifact return.
- Add signature, TLS, and untrusted-agent controls.

## 30-Day Practical Plan

Week 1:

- Pick the exact harness domain.
- Write `docs/spec.md`.
- Finalize `SKILL.md` workflow gates.
- Define state and artifact manifests.

Week 2:

- Build CLI `init`, `import`, `validate`, `doctor`.
- Add 3 fixture projects.
- Add clean install smoke.

Week 3:

- Add execution and export commands.
- Add prompt-injection and path-traversal tests.
- Run in Codex and Claude Code.

Week 4:

- Add MCP wrapper.
- Add trace logs.
- Add release tag and install docs.
- Run full benchmark suite.

## First Decision Needed

Before coding, choose the harness domain and output surface.

The domain determines everything else: state schema, eval fixtures, templates, security policy, and whether MCP or A2A is useful.

## Image Generation Harness Roadmap Addendum

For this project, the domain is now specific enough to plan:

**Agent Skill-compatible image generation execution harness** for creator-controlled still-image generation, with later video and comics.

Suggested spec sequence:

1. `docs/specs/001-image-harness.md`: define creator intent graph, ComfyUI runtime boundary, project state, artifact model, and non-goals.
2. `docs/specs/002-comfyui-capability-index.md`: define `/object_info`, `/models`, templates, subgraphs, system stats, missing capabilities, and cache invalidation.
3. `docs/specs/003-workflow-compiler.md`: define intent graph -> API-format workflow JSON, template filling, approved subgraph composition, and graph patch ops.
4. `docs/specs/004-control-stack.md`: define LoRA, ControlNet/T2I-Adapter, SAM masks, IP-Adapter, inpaint/outpaint, upscaling, guidance, and route policy.
5. `docs/specs/005-evaluation-provenance.md`: define OCR/text, color, reference fusion, identity, mask preservation, composition, safety, and provenance gates.

First implementation milestone after spec approval:

```text
index_capabilities -> compile known template -> validate workflow -> execute on local ComfyUI -> collect history/output -> evaluate basic gates -> export run bundle
```

Do not start with arbitrary graph synthesis or custom-node installation. Template filling and approved subgraph composition provide enough power for a real creator workflow while keeping validation tractable.
