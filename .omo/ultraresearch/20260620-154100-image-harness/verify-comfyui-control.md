# Verification: ComfyUI LLM Control Feasibility

Date: 2026-06-20

## Verdict

CONFIRMED with a boundary: an LLM harness can orchestrate ComfyUI execution, discover the installed node surface, compile/validate API-format workflow graphs, queue work, observe progress, collect outputs, and reuse templates/subgraphs. It cannot guarantee semantic correctness, side-effect safety, or license/runtime availability for every arbitrary custom node from metadata alone.

## Direct Evidence

### ComfyUI repository state

Command:

```bash
gh repo view Comfy-Org/ComfyUI --json nameWithOwner,url,description,stargazerCount,updatedAt,defaultBranchRef
gh api repos/Comfy-Org/ComfyUI/commits/master --jq .sha
```

Observed:

```text
Comfy-Org/ComfyUI
description: The most powerful and modular diffusion model GUI, api and backend with a graph/nodes interface.
stars: 117631
updatedAt: 2026-06-20T06:39:00Z
default branch: master
HEAD: dc3f8f314a987d23115ed278693e76cf6e72a5a0
```

### Server API and graph lifecycle

Primary docs and Context7 verification show the local ComfyUI server exposes:

- `GET /object_info` and `GET /object_info/{node_class}` for node metadata.
- `GET /models` and `GET /models/{folder}` for model categories/files.
- `POST /prompt` for validating and queueing API-format workflow graphs.
- `WEBSOCKET /ws` for progress/status.
- `GET /history` and `GET /history/{prompt_id}` for completed output metadata.
- `GET /queue`, `POST /queue`, `POST /interrupt`, and `POST /free` for operational control.
- `POST /upload/image`, `POST /upload/mask`, and `GET /view` for asset I/O.
- `GET /workflow_templates` and `GET /global_subgraphs` for reusable templates and subgraph blueprints.

Sources:

- ComfyUI routes docs: https://docs.comfy.org/development/comfyui-server/comms_routes
- Workflow API format docs: https://docs.comfy.org/development/api-development/workflow-api-format
- Context7 `/comfy-org/docs` query: routes and API-format example returned on 2026-06-20.
- Context7 `/comfy-org/comfyui` query: `node_info`, `/object_info`, and `/prompt` implementation excerpts returned on 2026-06-20.

### Node metadata is structural

The current ComfyUI implementation exposes node declarations from `nodes.NODE_CLASS_MAPPINGS`, including:

- `INPUT_TYPES()`
- `RETURN_TYPES`
- optional output names/list flags
- display name
- description
- Python module
- category
- flags such as deprecated/experimental/dev-only where present

This is enough for a harness compiler to reject unknown node classes, missing inputs, incompatible links, invalid combo values, and out-of-range scalar values. It is not a formal proof of the behavior of arbitrary Python code behind a custom node.

Source: https://github.com/Comfy-Org/ComfyUI/blob/dc3f8f314a987d23115ed278693e76cf6e72a5a0/server.py

### Prompt schema remains a known gap

Command:

```bash
gh issue view 8899 --repo Comfy-Org/ComfyUI --json title,url,state,createdAt,updatedAt,body
gh pr view 13094 --repo Comfy-Org/ComfyUI --json title,url,state,createdAt,updatedAt,body
```

Observed:

```text
Issue #8899: Add JSON Schema for Prompt API Format
state: OPEN
createdAt: 2025-07-14T06:02:34Z
updatedAt: 2026-03-21T19:51:09Z
problem: no formal specification for /prompt endpoint; workflow vs prompt format confusion; no validation/IDE support.

PR #13094: [BOUNTY #8899] Add JSON Schema for Prompt API Format
state: OPEN
createdAt: 2026-03-21T19:53:28Z
updatedAt: 2026-03-22T23:00:14Z
summary: adds schemas/prompt.json and docs/api/prompt-schema.md.
```

Implication: a production harness should ship its own conservative prompt/workflow schema layer and validate against live `/object_info`; it should not wait for a universal upstream JSON Schema.

### Official ComfyUI agent direction

ComfyUI official docs now expose an Agent Tools / MCP section:

- Agent Tools overview says MCP connects agents to ComfyUI for image, video, audio, and 3D generation.
- Comfy Cloud MCP is closed beta, scoped to Claude Code and Claude Desktop at capture time, and supports generation, model/node/template search, and workflow execution.
- Comfy Partner MCP is private preview/waitlist, local, API-provider oriented, and exposes unified generation tools across partner providers rather than arbitrary local custom workflows.

Sources:

- https://docs.comfy.org/agent-tools
- https://docs.comfy.org/agent-tools/cloud
- https://docs.comfy.org/agent-tools/partner-mcp

## Prior Attempt Verification

Commands:

```bash
gh repo view artokun/comfyui-mcp --json nameWithOwner,description,url,stargazerCount,updatedAt,defaultBranchRef,isArchived
gh repo view MieMieeeee/comfyui-agent-skill --json nameWithOwner,description,url,stargazerCount,updatedAt,defaultBranchRef,isArchived
gh repo view HuangYuChuh/ComfyUI_Skill_CLI --json nameWithOwner,description,url,stargazerCount,updatedAt,defaultBranchRef,isArchived
gh repo view HuangYuChuh/ComfyUI_Skills_OpenClaw --json nameWithOwner,description,url,stargazerCount,updatedAt,defaultBranchRef,isArchived
gh repo view twwch/comfyui-workflow-skill --json nameWithOwner,description,url,stargazerCount,updatedAt,defaultBranchRef,isArchived
gh repo view AIDC-AI/Pixelle-MCP --json nameWithOwner,description,url,stargazerCount,updatedAt
gh repo view joenorton/comfyui-mcp-server --json nameWithOwner,description,url,stargazerCount,updatedAt
gh repo view 21Pdontno/comfyui-workflow-skills --json nameWithOwner,description,url,stargazerCount,updatedAt
gh issue view 7780 --repo Comfy-Org/ComfyUI --json title,url,state,createdAt,updatedAt,body
```

Observed:

| Project | Observed state on 2026-06-20 | What it proves |
|---|---|---|
| `artokun/comfyui-mcp` | 166 stars, updated 2026-06-20, description claims Claude Code plugin + MCP server, 88 tools, 14 skills, live graph editing, model/custom-node management. | Strongest current community proof that LLM/MCP-driven ComfyUI graph control is feasible. |
| `MieMieeeee/comfyui-agent-skill` | 86 stars, updated 2026-06-16, description says agents run registered ComfyUI workflows through a local server with structured JSON outputs. | Direct Agent Skill-style wrapper pattern. |
| `HuangYuChuh/ComfyUI_Skill_CLI` | 26 stars, updated 2026-06-11, agent-friendly CLI for managing/executing workflow skills. | CLI layer for ComfyUI workflow skills. |
| `HuangYuChuh/ComfyUI_Skills_OpenClaw` | 324 stars, updated 2026-06-18, claims OpenClaw/Hermes/Codex/Claude Code skill compatibility. | Evidence of cross-agent skill packaging for ComfyUI workflows. |
| `LingyiChen-AI/comfyui-workflow-skill` | 312 stars, updated 2026-06-20, description says natural language to workflow JSON, 34 templates, 360+ node definitions. | Evidence of natural-language workflow generation as a skill. |
| `AIDC-AI/Pixelle-MCP` | 1053 stars, updated 2026-06-18, describes an open-source multimodal AIGC solution based on ComfyUI + MCP + LLM. | Strong MCP/LLM integration proof point, but broad product surface does not equal universal custom-node semantic control. |
| `joenorton/comfyui-mcp-server` | 360 stars, updated 2026-06-19, lightweight Python MCP server for local ComfyUI. | Confirms the wrapper pattern cited by ComfyUI MCP issue discussions. |
| `21Pdontno/comfyui-workflow-skills` | 4 stars, updated 2026-06-06, workflow lifecycle skill for OpenClaw, Claude Code, and other skill-based agents. | Low-adoption but directly relevant skill-architecture prior art. |
| ComfyUI issue #7780 | OPEN, created 2025-04-24, requests MCP support in core and cites separate MCP server. | Demand for core MCP integration existed before current official MCP docs. |

## Constraints for a Harness Plan

1. Use ComfyUI as runtime, not as code to migrate into the harness.
2. Put a typed intermediate graph between creator intent and ComfyUI workflow JSON.
3. Build a live capability index from `/object_info`, `/models`, `/system_stats`, `/workflow_templates`, and `/global_subgraphs`.
4. Validate all generated graphs before `POST /prompt`.
5. Treat custom-node docs, model cards, workflow metadata, and node descriptions as untrusted data.
6. Disallow LLM-driven custom-node installation unless allowlisted and pinned.
7. Persist workflow JSON, node/model versions, seeds, input assets, output hashes, and evaluation results as provenance.

## Cleanup Receipt

No ComfyUI server, browser, tmux session, temp clone, or long-running process was started for this verification. Direct network checks used `gh`, web fetch, Context7, and Hugging Face read-only tools.
