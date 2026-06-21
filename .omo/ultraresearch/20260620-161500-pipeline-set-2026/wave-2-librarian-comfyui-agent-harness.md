# Wave 2: ComfyUI Agent Harness Attempts

Worker: `019ee3fb-2d97-7aa0-a5a9-793b3c37fed5`

## Bottom Line

There are existing ComfyUI-to-agent efforts. The field is not empty.

However, no checked source fully matches the desired target: an installable Codex/Claude-compatible Skill package that can use the broad ComfyUI custom-node ecosystem, compile creator intent into validated workflows, manage dependencies/models, support detailed control surfaces, and certify creator recipes with repeatable smoke/evaluation gates.

## Projects Checked

### `artokun/comfyui-mcp`

- Strongest current coverage.
- Provides an MCP server for ComfyUI, launched with `npx -y comfyui-mcp`.
- Claims image/video workflow execution, workflow authoring, model and custom-node management, live graph editing, and Claude Code plugin packaging.
- Changelog reports manifest application, custom-node verification, workflow dependency extraction/install, node snapshots, and custom-node install/update/fix/list/sync tools.
- Gap: Claude-centric. Codex compatibility is likely through shell/MCP but not first-class in the checked docs. It is not the same thing as a portable `npx skills add --agent codex` Skill package with our recipe certification layer.

Sources:

- https://github.com/artokun/comfyui-mcp
- https://github.com/artokun/comfyui-mcp/blob/main/CHANGELOG.md

### `HuangYuChuh/ComfyUI_Skills_OpenClaw`

- Strongest Skill-style project.
- Explicitly names Codex and Claude Code.
- Uses a `SKILL.md`, CLI, workflow import, schema mapping, dependency checks, validation, upload, history, and multi-server workflow execution.
- It intentionally hides raw node IDs and exposes schema-mapped parameters.
- Gap: installed by cloning plus `pip install -U comfyui-skill-cli`, not by `npx skills add` in the checked README. It is graph-opaque and does not aim to be a live graph editor or full custom-node control plane.

Sources:

- https://github.com/HuangYuChuh/ComfyUI_Skills_OpenClaw
- https://github.com/HuangYuChuh/ComfyUI_Skills_OpenClaw/blob/main/SKILL.md

### `heshengtao/comfyui_LLM_party`

- Primarily an LLM workflow node pack inside ComfyUI.
- Useful as evidence that LLM/agent workflows are entering the ComfyUI node ecosystem.
- Gap: not a general external control plane for arbitrary ComfyUI workflows/custom nodes.

Source:

- https://github.com/heshengtao/comfyui_LLM_party

### `apppps/comfyui-agent`

- Early ComfyUI agent/supervision overlay.
- Gap: weak evidence for dependency/model management, arbitrary workflow import, validation, or Codex/Skill compatibility.

Source:

- https://github.com/apppps/comfyui-agent

### Official ComfyUI Agent/MCP Surface

- Official docs now expose Agent Tools / MCP.
- Comfy Cloud MCP is invite-only closed beta and cloud-scoped.
- Partner MCP is private preview and provider-oriented.
- Local ComfyUI server API and workflow export remain the reliable integration substrate for a local custom-node ecosystem harness.

Sources:

- https://docs.comfy.org/
- https://docs.comfy.org/agent-tools/cloud
- https://docs.comfy.org/agent-tools/partner-mcp
- https://docs.comfy.org/development/comfyui-server/comms_routes
- https://docs.comfy.org/development/comfyui-server/api-examples

## Contribution Opportunity

The clearest contribution-shaped gap is not "invent ComfyUI automation from scratch." It is:

1. Standardize Codex-compatible Skill packaging around ComfyUI workflows.
2. Add recipe manifests that map ComfyUI workflow JSON, required custom nodes, models, parameter schemas, and smoke tests.
3. Provide a two-tier support model:
   - Universal workflow inventory/execution lane for arbitrary installed nodes/workflows.
   - Certified recipe lane for pipelines that are semantically validated.
4. Upstream useful pieces to `comfyui-mcp`, `ComfyUI_Skills_OpenClaw`, or ComfyUI docs/registry conventions when stable.
