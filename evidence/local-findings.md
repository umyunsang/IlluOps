# Local Findings

Date: 2026-06-20

## Project Folder

Created:

```text
/Users/um-yunsang/Documents/Codex/2026-06-20/new-chat/outputs/agent-skill-compatible-llm-harness-2026/
```

Subfolders:

```text
research/
evidence/
scaffold/
```

## Skills CLI

Command:

```bash
npx --yes skills --version
```

Observed:

```text
1.5.12
```

Command:

```bash
npx --yes skills --help
```

Important observed capabilities:

- `skills add <package>`
- `skills use <package>@<skill>`
- `skills remove`
- `skills list`
- `skills find`
- `skills update`
- `skills experimental_install`
- `skills init`
- `skills experimental_sync`
- Add options include `--global`, `--agent`, `--skill`, `--copy`, `--all`, and `--full-depth`.
- Examples include installing to `claude-code` and using a skill through an agent.

## Local `ppt-master`

Current local Codex path observed in this session:

```text
/Users/um-yunsang/.codex/tools/ppt-master
```

Observed files:

```text
/Users/um-yunsang/.codex/tools/ppt-master/skills/ppt-master/SKILL.md
/Users/um-yunsang/.codex/tools/ppt-master/AGENTS.md
/Users/um-yunsang/.codex/tools/ppt-master/CLAUDE.md
```

The current filesystem did not contain:

```text
/Users/um-yunsang/.agents/skills/ppt-master
```

Older memory from prior work recorded a `.claude` to `.agents` skill path behavior. Treat that as historical context, not current filesystem proof.

### `ppt-master` Skill Traits

The local `SKILL.md` describes:

- Multi-format source conversion: PDF, DOCX, URL, Markdown, and other document forms.
- SVG page generation and PPTX export.
- Main pipeline: source document, project creation, template option, strategist, image generation, executor live preview, quality check, post-processing, export.
- Strict serial execution.
- Blocking gates.
- No speculative execution.
- Per-page `spec_lock.md` reread.
- Script-backed source conversion, project management, image generation, SVG quality checking, post-processing, and export.

The local `AGENTS.md` says `skills/ppt-master/SKILL.md` is the authoritative workflow for project creation, role switching, serial execution, quality gates, post-processing, and export.

## Repository Metadata Captured With `gh repo view`

As of 2026-06-20:

| Repository | Stars | Forks | Pushed At | Notes |
|---|---:|---:|---|---|
| `hugohe3/ppt-master` | 29441 | 2579 | 2026-06-20T05:42:57Z | MIT. Active reference skill package. |
| `openai/codex` | 92222 | 13634 | 2026-06-20T05:54:46Z | Apache-2.0. Lightweight coding agent. |
| `anthropics/claude-code` | 133398 | 21570 | 2026-06-19T01:20:50Z | Claude Code terminal coding agent. |
| `SWE-agent/mini-swe-agent` | 5287 | 720 | 2026-06-19T00:56:41Z | MIT. Minimal coding-agent harness. |
| `OpenHands/OpenHands` | 77798 | 9889 | 2026-06-19T14:28:49Z | AI-driven development platform. |
| `Aider-AI/aider` | 46487 | 4627 | 2026-05-22T14:02:20Z | Apache-2.0. Terminal AI pair programming. |
| `a2aproject/A2A` | 24367 | 2468 | 2026-06-12T10:40:26Z | Apache-2.0. Agent2Agent protocol. |
| `modelcontextprotocol/modelcontextprotocol` | 8439 | 1602 | 2026-06-20T02:29:07Z | MCP specification and documentation. |
| `agentskills/agentskills` | 20785 | 1310 | 2026-05-20T17:23:06Z | Apache-2.0. Agent Skills specification and documentation. |
| `langchain-ai/langgraph` | 35246 | 5909 | 2026-06-19T10:05:20Z | MIT. Durable agent graph execution. |
| `openai/openai-agents-python` | 27273 | 4205 | 2026-06-19T06:45:24Z | MIT. Multi-agent workflow framework. |

## Current Workspace Addendum

Current working directory for this pass:

```text
/Users/um-yunsang/image-harness-2026
```

This folder is not currently a git repository:

```text
fatal: not a git repository (or any of the parent directories): .git
```

Observed top-level files:

```text
README.md
evidence/
research/
scaffold/
.omo/
```

ULW state created:

```text
.omo/ulw-loop/019ee3bd-cc58-7d40-9c0f-1886d00759a0/
```

Research session created:

```text
.omo/ultraresearch/20260620-154100-image-harness/
```

Artifact alignment check:

- Existing README and scaffold already point to the right product shape: skill-first, deterministic CLI/scripts, optional MCP/A2A later.
- New research does not require changing the scaffold yet.
- Future implementation should happen only after a spec is approved, consistent with the supplied AGENTS workflow.
