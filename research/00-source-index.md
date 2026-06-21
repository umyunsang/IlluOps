# Source Index

Date: 2026-06-20

This index separates primary sources, current local evidence, and research papers. Prefer primary docs over blog posts when implementing.

## Agent Skills And Skill Packaging

| Source | Why It Matters |
|---|---|
| [Agent Skills](https://agentskills.io/home) | Defines the open Agent Skills format: `SKILL.md` plus optional scripts, references, and assets. This is the direct compatibility layer for a `ppt-master`-style package. |
| [Codex Skills](https://developers.openai.com/codex/skills) | Official Codex skill and plugin model. Codex treats skills as workflow authoring format and plugins as installable bundles that may include skills, apps, MCP config, and assets. |
| [Claude Code Skills](https://code.claude.com/docs/en/skills) | Official Claude Code skill surface. Important for cross-install compatibility and invocation behavior. |
| Local `skills` CLI 1.5.12 | Confirms `npx skills add <package>`, `--agent`, `--global`, `--copy`, lock restore, and project/global skill management. See `../evidence/local-findings.md`. |

## Codex And Claude Code Execution Surfaces

| Source | Why It Matters |
|---|---|
| [Codex AGENTS.md](https://developers.openai.com/codex/guides/agents-md) | Codex instruction discovery and merge behavior. A harness should ship precise `AGENTS.md` guidance for repo-level workflows. |
| [Codex MCP](https://developers.openai.com/codex/mcp) | How Codex connects MCP servers and exposes structured tools. |
| [Codex configuration](https://developers.openai.com/codex/config-reference) | Approval, sandbox, MCP elicitation, and skill approval policy controls. |
| [Codex as MCP server with Agents SDK](https://developers.openai.com/codex/guides/agents-sdk) | Shows how Codex can be called as a tool by a larger orchestrator. |
| [OpenAI Agents guide](https://developers.openai.com/api/docs/guides/agents) | Use when the application owns orchestration, tool execution, approvals, and state. |
| [Claude Code overview](https://code.claude.com/docs/en/overview) | Claude Code execution surface: file editing, shell commands, hooks, skills, MCP, subagents, SDK. |
| [Claude Agent SDK](https://code.claude.com/docs/en/agent-sdk/overview) | Programmatic access to Claude Code's agent loop, tools, and context management. |
| [Claude Code hooks](https://code.claude.com/docs/en/hooks) | Lifecycle interception points for approvals, logging, validation, and policy checks. |
| [Claude Code subagents](https://code.claude.com/docs/en/sub-agents) | Separate context and tool permissions. Useful for review, extraction, or validation roles. |
| [Claude Code MCP](https://code.claude.com/docs/en/mcp) | MCP server support, remote transport, output limits, environment variables, and elicitation. |
| [Claude Code settings and plugins](https://code.claude.com/docs/en/settings) | Plugin system can extend Claude Code with skills, agents, hooks, and MCP servers. |

## Protocols

| Source | Why It Matters |
|---|---|
| [MCP specification 2025-11-25](https://modelcontextprotocol.io/specification/2025-11-25) | Current protocol model for tools, resources, prompts, roots, sampling, elicitation, and JSON-RPC transport. |
| [MCP tools spec](https://modelcontextprotocol.io/specification/2025-11-25/server/tools) | Tool schema, annotations, output schema validation, and human-in-the-loop recommendations. |
| [A2A specification](https://a2a-protocol.org/latest/specification/) | Agent-to-agent protocol for messages, tasks, parts, artifacts, streaming, and push notifications. |
| [A2A v1 changes](https://a2a-protocol.org/latest/whats-new-v1/) | Protocol version headers, version negotiation, agent cards, pagination, signature verification, and mutual TLS priorities. |
| [A2A GitHub](https://github.com/a2aproject/A2A) | Active open protocol repository for opaque agent interoperability. |

## Frameworks And OSS Harnesses

| Source | Why It Matters |
|---|---|
| [OpenAI Agents SDK Python](https://github.com/openai/openai-agents-python) | Lightweight multi-agent workflow framework with tools, handoffs, guardrails, tracing, sessions, and MCP integration. |
| [LangGraph](https://github.com/langchain-ai/langgraph) | Durable graph execution, checkpoints, interrupts, replay, and long-running agent workflows. |
| [OpenAI Codex CLI](https://github.com/openai/codex) | Local coding agent and useful reference for sandbox, approvals, CLI ergonomics, and MCP integration. |
| [Claude Code](https://github.com/anthropics/claude-code) | High-adoption terminal coding agent. Important compatibility target. |
| [ppt-master](https://github.com/hugohe3/ppt-master) | Reference skill package for multi-phase artifact generation with scripts, templates, live preview, validation, and export. |
| [mini-swe-agent](https://github.com/SWE-agent/mini-swe-agent) | Minimal coding-agent harness with strong benchmark results and small architecture. |
| [SWE-agent docs](https://swe-agent.com/latest/) | Useful reference for minimal config-driven agents and benchmark-oriented execution. |
| [OpenHands](https://github.com/OpenHands/OpenHands) | Large OSS agent platform with sandboxed runtime, browser, shell, and multi-agent execution. |
| [OpenHands runtime architecture](https://docs.openhands.dev/openhands/usage/architecture/runtime) | Docker runtime model and action/observation protocol. |
| [aider repo map](https://aider.chat/docs/repomap.html) | Context engineering reference: repository map and graph-ranking under token budget. |

## Benchmarks And Papers

| Source | Why It Matters |
|---|---|
| [SWE-bench](https://www.swebench.com/) | Real GitHub issue benchmark and Docker evaluation harness. |
| [SWE-bench GitHub](https://github.com/SWE-bench/SWE-bench) | Evaluation harness and dataset implementation. |
| [Terminal-Bench 2.0](https://arxiv.org/html/2601.11868v1) | Terminal task benchmark with containerized environments, tests, and reference solutions. Very relevant for execution harness evaluation. |
| [OpenHands paper](https://openreview.net/forum?id=OJd3ayDDoF) | Platform architecture for AI software developers with code, CLI, browser, sandbox, and benchmark emphasis. |
| [Harnessing the Harnesses survey](https://www.preprints.org/manuscript/202604.0428) | 2026 survey framing agent harnesses as a primary determinant of performance and reliability. |
| [Awesome Agent Harness](https://github.com/Gloriaameng/Awesome-Agent-Harness) | Curated landscape map and harness taxonomy. |
| [Context Engineering survey](https://arxiv.org/html/2507.13334v1) | Formalizes retrieval, memory, tool-integrated, and multi-agent context management. |
| [LLM Agent Evaluation survey](https://arxiv.org/html/2503.16416v2) | Evaluation dimensions, limitations, and gaps for sequential agent behavior. |
| [Agent Protocol survey](https://arxiv.org/html/2504.16736v2) | Distinguishes context-oriented protocols from inter-agent protocols and discusses security and scalability tradeoffs. |
| [Agentic Context Engineering](https://arxiv.org/html/2510.04618v1) | Context adaptation strategies for long-running agents. |
| [SandboxEscapeBench](https://arxiv.org/html/2603.02277v1) | Container and sandbox escape risks for LLM agents with shell/code tools. |
| [MCP threat model](https://arxiv.org/html/2603.22489v1) | Large-scale MCP security study, tool poisoning, and vulnerable server findings. |
| [Invariant Labs MCP tool poisoning](https://invariantlabs.ai/blog/mcp-security-notification-tool-poisoning-attacks) | Concrete MCP tool poisoning attack pattern and mitigation motivation. |

## Local Evidence

| Evidence | Notes |
|---|---|
| `/Users/um-yunsang/.codex/tools/ppt-master/skills/ppt-master/SKILL.md` | Current local Codex-installed ppt-master skill entrypoint. |
| `/Users/um-yunsang/.codex/tools/ppt-master/AGENTS.md` | Confirms `SKILL.md` is authoritative for generation workflow and repo modification. |
| `npx --yes skills --version` | Returned `1.5.12` on 2026-06-20. |
| `gh repo view ...` | Current repository metadata captured for ppt-master, Codex, Claude Code, mini-swe-agent, OpenHands, aider, A2A, MCP, Agent Skills, LangGraph, and OpenAI Agents SDK. |

## Image Generation Models And Runtimes

| Source | Why It Matters |
|---|---|
| [OpenAI image generation guide](https://developers.openai.com/api/docs/guides/image-generation) | Hosted image generation/editing route and API semantics. |
| [OpenAI gpt-image-2 model page](https://developers.openai.com/api/docs/models/gpt-image-2) | Current OpenAI image model reference for high-quality hosted generation and editing. |
| [FLUX.2](https://bfl.ai/models/flux-2) | Current BFL model family with multi-reference, prompt following, and text-rendering emphasis. |
| [black-forest-labs/flux2](https://github.com/black-forest-labs/flux2) | Open-source entrypoint and model variant notes for FLUX.2. |
| [QwenLM/Qwen-Image](https://github.com/QwenLM/Qwen-Image) | Open image model family with text rendering, editing, LoRA/full training, and ComfyUI support notes. |
| [Tongyi-MAI/Z-Image](https://github.com/Tongyi-MAI/Z-Image) | 6B model family with Turbo and editing variants for fast generation. |
| [Tencent-Hunyuan/HunyuanImage-3.0](https://github.com/Tencent-Hunyuan/HunyuanImage-3.0) | Native multimodal generation/editing model direction with MoE and distilled variants. |
| [baidu/ERNIE-Image](https://github.com/baidu/ERNIE-Image) | Open-source 8B text-to-image model and prompt-enhancer/aesthetic benchmark reference. |
| [Wan video](https://github.com/Wan-Video/Wan2.1) | Image-to-video, text-to-video, video editing, and video-to-audio path for later secondary creation. |

## 2026 Verification Sources

| Source | Why It Matters |
|---|---|
| [FLUX.2 blog](https://bfl.ai/blog/flux-2) | Official FLUX.2 positioning: multi-reference, text rendering, brand guidelines, structured prompts, and up to 4MP editing. |
| [Qwen-Image GitHub](https://github.com/QwenLM/Qwen-Image) | 20B MMDiT image model, complex text rendering, precise editing, Apache-2.0 license. |
| [Qwen-Image-Edit blog](https://qwenlm.github.io/blog/qwen-image-edit/) | Editing architecture signal: Qwen2.5-VL semantic control plus VAE appearance control. |
| [Z-Image GitHub](https://github.com/Tongyi-MAI/Z-Image) | 6B efficient image generation family with turbo/edit variants. |
| [HunyuanImage-3.0 GitHub](https://github.com/Tencent-Hunyuan/HunyuanImage-3.0) | 80B MoE native multimodal image-generation direction. |
| [ERNIE-Image technical report](https://arxiv.org/html/2605.25347v1) | 8B single-stream DiT, FLUX.2 VAE, prompt enhancer, and contemporary open-model comparison. |
| [STRICT](https://aclanthology.org/2025.emnlp-main.1070/) | Text-in-image rendering benchmark. |
| [TextAtlas5M](https://textatlas5m.github.io/) | Dense and structured text/layout generation benchmark. |
| [MultiRef](https://multiref.github.io/) | Multi-reference image generation benchmark. |
| [Co-EditBench](https://openreview.net/forum?id=tKz0XEaZXw) | Human-aligned instruction-edit benchmark. |
| [GenColorBench](https://openreview.net/forum?id=E9zStzWz6M) | Color/palette fidelity benchmark for T2I. |
| [GenEval 2](https://arxiv.org/html/2512.16853v1) | Compositional prompt-adherence benchmark under benchmark drift. |
| [DreamBench++](https://dreambenchplus.github.io/) | Personalized image generation benchmark. |
| [ServImage](https://openreview.net/forum?id=bH2JgJdHp0) | Real commercial design utility benchmark. |

## Computer Vision And Control

| Source | Why It Matters |
|---|---|
| [SAM3](https://github.com/facebookresearch/sam3) | Current promptable detection, segmentation, and tracking route for masks and object control. |
| [Meta SAM3 announcement](https://ai.meta.com/blog/segment-anything-model-3/) | Official framing for SAM3 and SAM3.1 in image/video segmentation and tracking. |
| [Grounded-SAM-2](https://github.com/IDEA-Research/Grounded-SAM-2) | Combines open-vocabulary detection, segmentation, and tracking. |
| [Depth Anything V2](https://github.com/DepthAnything/Depth-Anything-V2) | Depth-map generation for geometry/control workflows. |
| [ControlNet](https://github.com/lllyasviel/ControlNet) | Core control-image conditioning mechanism. |
| [LoRA](https://arxiv.org/abs/2106.09685) | Low-rank adaptation method for efficient personalization. |
| [Self-Attention Guidance](https://arxiv.org/abs/2210.00939) | Training-free guidance method relevant to SAG control policies. |

## ComfyUI Control Surface

| Source | Why It Matters |
|---|---|
| [ComfyUI API overview](https://docs.comfy.org/development/api-development/overview) | Compares local ComfyUI Server API and Comfy Cloud API; both use the same workflow format. |
| [ComfyUI workflow API format](https://docs.comfy.org/development/api-development/workflow-api-format) | Defines API-format workflow JSON for programmatic graph execution. |
| [ComfyUI server routes](https://docs.comfy.org/development/comfyui-server/comms_routes) | Documents `/prompt`, `/ws`, `/object_info`, `/models`, `/history`, `/queue`, `/interrupt`, `/free`, and upload/view routes. |
| [ComfyUI custom nodes V3 migration](https://docs.comfy.org/custom-nodes/v3_migration) | Current custom node schema direction for typed inputs/outputs, async execution, progress, and extension lifecycle. |
| [ComfyUI workflow templates](https://docs.comfy.org/custom-nodes/workflow_templates) | Shows how custom nodes expose example workflows for template-based harnessing. |
| [ComfyUI subgraph blueprints](https://docs.comfy.org/custom-nodes/subgraph_blueprints) | Shows reusable subgraph blueprints for composition. |
| [ComfyUI Agent Tools / MCP](https://docs.comfy.org/agent-tools) | Official agent/MCP direction for image, video, audio, and 3D generation. |
| [artokun/comfyui-mcp](https://github.com/artokun/comfyui-mcp) | Community proof point for local/remote ComfyUI MCP, Claude Code plugin, skills, graph editing, model management, and custom-node workflows. |
| [ComfyUI issue #7780](https://github.com/Comfy-Org/ComfyUI/issues/7780) | Shows explicit demand for MCP support in ComfyUI core and cites separate server implementations. |
| [ComfyUI issue #8899](https://github.com/Comfy-Org/ComfyUI/issues/8899) | Documents lack of formal `/prompt` API JSON Schema and automation difficulties. |
| [ComfyUI PR #13094](https://github.com/Comfy-Org/ComfyUI/pull/13094) | Open PR attempting `/prompt` JSON Schema and docs. |
| [LingyiChen-AI/comfyui-workflow-skill](https://github.com/LingyiChen-AI/comfyui-workflow-skill) | Natural language to ComfyUI workflow JSON skill. |
| [MieMieeeee/comfyui-agent-skill](https://github.com/MieMieeeee/comfyui-agent-skill) | Agent Skill-style registered ComfyUI workflow executor. |
| [HuangYuChuh/ComfyUI_Skills_OpenClaw](https://github.com/HuangYuChuh/ComfyUI_Skills_OpenClaw) | Cross-agent ComfyUI workflow skill packaging for OpenClaw, Hermes, Codex, and Claude Code. |
| [HuangYuChuh/ComfyUI_Skill_CLI](https://github.com/HuangYuChuh/ComfyUI_Skill_CLI) | Agent-friendly CLI for ComfyUI workflow skills. |
| [AIDC-AI/Pixelle-MCP](https://github.com/AIDC-AI/Pixelle-MCP) | ComfyUI + MCP + LLM multimodal AIGC solution; strong prior art for agent control breadth. |
| [joenorton/comfyui-mcp-server](https://github.com/joenorton/comfyui-mcp-server) | Lightweight local Python MCP server for ComfyUI, cited from ComfyUI MCP demand. |
| [21Pdontno/comfyui-workflow-skills](https://github.com/21Pdontno/comfyui-workflow-skills) | Skill-module approach for automating ComfyUI workflow lifecycle from agent platforms. |

## Security And Provenance

| Source | Why It Matters |
|---|---|
| [MCP tools spec 2025-11-25](https://modelcontextprotocol.io/specification/2025-11-25/server/tools) | Tool discovery/invocation, structured content, annotations, and human-in-the-loop recommendations. |
| [MCP authorization spec 2025-11-25](https://modelcontextprotocol.io/specification/2025-11-25/basic/authorization) | OAuth/HTTP authorization model for MCP. |
| [NSA MCP Security Design Considerations](https://www.nsa.gov/Portals/75/documents/Cybersecurity/CSI_MCP_SECURITY.pdf?ver=bmgiSbNQLP6Z_GiWtRt6bg%3D%3D) | May 2026 production security guidance for MCP tool poisoning, arbitrary code execution, hidden instructions, and validation controls. |
| [ComfyUI custom node server overview](https://docs.comfy.org/custom-nodes/backend/server_overview) | Confirms custom nodes are Python classes and part of the executable runtime surface. |
| [C2PA specification](https://spec.c2pa.org/) | Content provenance manifests and validation model. |
| [SynthID safeguards](https://ai.google.dev/responsible/docs/safeguards/synthid) | Watermarking capabilities and detection caveats. |
