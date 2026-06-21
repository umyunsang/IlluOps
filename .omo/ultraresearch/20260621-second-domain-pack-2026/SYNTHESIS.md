# Ultraresearch Synthesis: Q37 Second Non-Image Domain Pack 2026

Workers: orchestrator-owned research, no subagents due active tool policy · Waves: 1 source sweep plus lead closure · Sources: 32 primary/current sources · Verifications: source-level plus local `ppt-master` inspection

## Executive Summary

Recommendation: Q37 should select option `a`, the presentation/document production pack. This is the best first non-image proof because it produces real domain-native artifacts, supports review/revision/finalization/handoff, reuses the proven `ppt-master`-style Skill + scripts + local preview + export pattern, and avoids collapsing the product into either image generation or a coding-agent clone.

The pack should be named `presentation_document_pack`. Its v1 lifecycle should be: `brief_manifest.json` -> `outline_manifest.json` -> `source_material_manifest.json` -> `deck_doc_plan.json` -> `deck_doc_artifact_manifest.json` -> localhost review session -> saved revision/accept action -> `final_package_manifest.json` -> `handoff_manifest.json`. Optional provider calls, including `gpt-image-2` for conceptual support imagery, must remain brokered side effects with provider-profile caps, credential refs, input hashes, and output manifests. The accepted artifact must stay source-native and editable: PPTX/DOCX/Markdown/HTML/PDF exports where appropriate, not a whole-slide image dump.

Data/report/dashboard is the strongest third pack candidate, not the first. It has excellent structured-output proof, but 2026 data-agent research makes clear that realistic data agents need multi-source integration, messy joins, domain knowledge, file generation, visualization, and harder correctness grading. Code/repo automation is the strongest agent-ops proof, but it overlaps with the host agent and creates the largest safety/CI/sandbox blast radius. Presentation/document sits in the right middle: real production artifact, rich review loop, lower destructive risk, and enough current research/OSS support to be non-toy.

## Evidence By Theme

### 1. Cross-agent harness architecture wants artifacts, state, and human interrupts

LangGraph positions itself as a low-level runtime for long-running stateful agents with durable execution, streaming, human-in-the-loop, and persistence [S1]. Its interrupt model persists graph state and waits for external human input before resuming [S2]. Temporal's workflow streams add durable event channels for long-lived workflows and observers [S3], and Temporal documents idempotent activities because retries can duplicate side effects after worker crashes [S4]. OpenAI's Agents SDK guidance says to use the SDK when the application owns orchestration, tool execution, approvals, and state [S5], while handoffs let one agent delegate to a specialist [S6]. These sources all point to the same harness contract: durable state plus typed artifacts plus explicit human gates.

MCP, A2A, and AG-UI should remain ports, not the core. MCP standardizes LLM-app integration with external tools/data over JSON-RPC [S7]. A2A targets communication and interoperability between opaque agent applications [S8]. AG-UI standardizes an event-based agent-to-frontend connection [S9]. OpenTelemetry's GenAI conventions standardize model/tool-call observability fields such as model, token counts, tool calls, and tool results [S10]. Q37 therefore needs a domain pack that naturally emits inspectable artifacts and review events, not one that hides progress inside chat memory.

### 2. Presentation/document production is now a real evaluated domain

PresentBench, published in 2026, frames automated slide generation as a real-world task and introduces 238 expert-curated instances with an average of 54.1 binary checklist items per instance across fundamentals, layout, completeness, correctness, and fidelity [S11]. PPTAgent uses a two-stage edit-based workflow inspired by human presentation work: analyze reference presentations, draft an outline, then iteratively generate editing actions, with PPTEval covering content, design, and coherence [S12]. That is a close match for a harness domain pack: plan, edit, review, revise, finalize.

The OSS substrate is also strong. PptxGenJS generates standards-compatible OOXML presentations with text, tables, shapes, images, charts, templates, and no PowerPoint install [S13]. Its docs emphasize browser/Node integration, slide masters, major slide objects, SVG, GIF, media, RTL, and Asian font support [S14]. `python-pptx` creates, reads, and updates PPTX files from dynamic content such as database queries, analytics output, or JSON payloads, and can add text, images, tables, shapes, charts, and document properties [S15]. A current AI PPT ecosystem index shows multiple PPTX-native skills/apps, including `ppt-master`, PPTAgent, template-fill flows, and editable PPTX tooling [S16].

Local `ppt-master` confirms the relevant execution shape: it outputs native-shape editable PPTX, snapshots SVG output for archival/re-export, supports filling an existing deck, loads provider credentials from process env before `.env` fallbacks, and uses provider-specific API keys plus explicit backend selection [L1]. Its image generator is manifest-based and idempotent: reruns only process pending/failed items, with explicit backend/model/concurrency settings [L2]. The transferable pattern is not "make slides with images"; it is "Skill contract + deterministic scripts + manifests + preview/export gates + provider profile".

### 3. Data/report/dashboard is valuable but too fixture-heavy for Q37

The data-agent direction is important. DAB 2026 evaluates realistic multi-database data-agent tasks and identifies four hard properties: multi-database integration, ill-formatted join keys, unstructured text transformation, and domain knowledge [S17]. The 2026 data-agent taxonomy warns that "data agent" is used inconsistently and proposes autonomy levels from stateless assistance to conditional autonomy and full autonomy [S18]. DataSciBench evaluates data science tasks across preprocessing, statistics, visualization, mining, and interpretability with task/function/code evaluation [S19]. AIDABench targets end-to-end document analytics across question answering, file generation, and visualization [S20].

The data/report open-source substrate is strong but broad. Evidence builds SQL/Markdown data products, dashboards, reports, and embedded analytics [S21]. Observable Framework creates command-line data apps, dashboards, and reports from Markdown, JavaScript, SQL, Python, R, and other languages [S22]. Streamlit is optimized for quickly visualizing and interacting with data, including raw data, charts, and chart libraries [S23]. DuckDB is a portable in-process analytical SQL engine with Parquet/JSON/S3/lakehouse formats, many client APIs, and Hugging Face integration [S24]. This makes data/report a good pack 3, but Q37 would need heavy correctness fixtures, semantic layer policy, data provenance, chart validation, and domain-specific evaluation before it proves harness generality cleanly.

### 4. Code/repo automation is real but too close to the host agent

SWE-bench evaluates LMs on real GitHub issues where the model must generate a patch [S25], and SWE-bench Verified is a human-filtered 500-instance subset [S26]. OpenHands is a full AI software development platform that executes engineering work, plans, writes, and applies changes, with self-hosting, integrations, and enterprise controls [S27]. SWE-agent lets a model use tools to fix real GitHub repository issues [S28], and mini-SWE-agent demonstrates a very small but strong command-line agent [S29]. Open SWE shows the current async coding-agent pattern: GitHub task intake, plan review interrupts, sandboxed execution, tests, review, and PR creation [S30].

This is exactly why code/repo should not be Q37 v1. It would test the harness by rebuilding a coding-agent product inside a coding-agent host. SWE-EVO 2026 shows the risk: long-horizon software evolution across many files remains far harder than SWE-bench Verified, with gpt-5 + OpenHands around 19-21 percent on SWE-EVO versus 65 percent on SWE-bench Verified in the paper's comparison [S31]. A code pack should exist later only with a strong sandbox, repository permission model, CI/PR governance, destructive-operation policy, and dependency isolation.

### 5. Hubs reinforce artifact-native domain packs

Hugging Face Hub is a major open ML platform hosting models, datasets, and Spaces, with papers, agent docs, Spaces as MCP servers/tools/API endpoints, jobs, and repository versioning [S32]. It also hosts paper pages for PresentBench and DAB, linking research, code, and community discussion [S33] [S34]. The hub signal matters because future domain packs should be able to consume open papers/datasets/spaces as source/context, but Q37 should still use a domain where final artifacts are simple to package and review offline. Presentation/document fits that better than a data-agent pack that depends on live enterprise data connectors.

## Candidate Decision

| Candidate | Harness proof strength | Main upside | Main risk | Q37 verdict |
| --- | --- | --- | --- | --- |
| `a` Presentation/document production | High | Real editable artifact, outline/revision/review/final package, strong local `ppt-master` precedent, lower destructive risk | Need avoid image-first slide dumps and require source-native/editable outputs | Recommended Q37 default |
| `b` Data/report/dashboard production | High, but fixture-heavy | Strong structured data, charts, reports, reproducibility, Evidence/Observable/DuckDB ecosystem | Correctness/provenance/semantic-layer contracts are heavy for v1 | Defer as pack 3 |
| `c` Code/repo automation | Highest agent-ops proof | Closest to autonomous agent work, strong OSS/evals | Overlaps host agent, high sandbox/CI/security blast radius, long-horizon weakness | Defer until security-hardening phase |

## Required Q37 Pack Shape

`presentation_document_pack` must include:

- `domain_pack_manifest.json` with `pack_id=presentation_document_pack`, schema version, capability declarations, source-native artifact formats, review renderer, finalizer, and memory policy.
- `brief_manifest.json` for creator/client-facing objective, audience, use case, constraints, tone, source materials, brand/design requirements, output formats, budget/provider profile, and review priorities.
- `outline_manifest.json` with narrative structure, sections/slides/pages, audience intent, evidence needs, and unresolved source gaps.
- `deck_doc_plan.json` with selected format route: PPTX-native deck, DOCX/Markdown report, HTML/PDF static artifact, or multi-export.
- `deck_doc_artifact_manifest.json` with generated source files, editable export paths, rendered preview paths, checksums, source references, and quality checklist.
- `review_session.json` plus localhost review UI for outline/deck/doc comments, accept/revise/branch/stop actions, and saved feedback.
- `revision_action.json` or `feedback.json` so post-review continuation is human-authorized, not LLM-autonomous.
- `final_package_manifest.json` with accepted editable sources, static preview, exported deliverables, provenance, source/license notes, generated asset manifests, cost/budget summary, checksums, and reproducibility commands.
- `handoff_manifest.json` with current status, allowed next actions, selected deliverables, trace/evidence refs, excluded secrets, and no chat transcript.

## Must-Not-Have

- Do not define the pack as image-first deck generation. `gpt-image-2` may create conceptual support assets, but the pack's proof is editable/source-native deck or document artifacts.
- Do not reuse image-pack fields such as Civitai, Comfy, workflow graph patch, gallery image sidecars, model AIR, ControlNet/LoRA/SAM/IPAdapter, or visual generation cost assumptions.
- Do not let the pack directly call providers or write files outside its granted roots. Provider calls, external asset search, file writes, memory writes, and tool/agent calls remain brokered side effects.
- Do not make code/repo automation part of Q37 v1. It is a later high-security domain pack.

## Plan Impact

- Treat Q37 as research-recommended option `a`: presentation/document production pack.
- Keep Q37 technically pending until the user confirms or vetoes, because it is a visible product/domain decision.
- Update the plan's "next move" from "answer Q37" to "confirm Q37-a recommendation".
- Add pack-specific artifacts and stop conditions to the future implementation spec after confirmation.
- Record data/report/dashboard as the preferred third pack and code/repo automation as a later security-heavy pack.

## Sources

- [S1] LangGraph overview: https://docs.langchain.com/oss/python/langgraph/overview
- [S2] LangGraph interrupts: https://docs.langchain.com/oss/python/langgraph/interrupts
- [S3] Temporal Workflow Streams: https://docs.temporal.io/develop/python/workflows/workflow-streams
- [S4] Temporal activity idempotency: https://docs.temporal.io/activity-definition
- [S5] OpenAI Agents SDK guide: https://developers.openai.com/api/docs/guides/agents
- [S6] OpenAI Agents SDK handoffs: https://openai.github.io/openai-agents-python/handoffs/
- [S7] MCP specification 2025-11-25: https://modelcontextprotocol.io/specification/2025-11-25
- [S8] A2A GitHub/spec surface: https://github.com/a2aproject/A2A
- [S9] AG-UI overview: https://docs.ag-ui.com/introduction
- [S10] OpenTelemetry GenAI observability: https://opentelemetry.io/blog/2026/genai-observability/
- [S11] PresentBench: https://arxiv.org/html/2603.07244v1
- [S12] PPTAgent: https://arxiv.org/html/2501.03936v3
- [S13] PptxGenJS GitHub: https://github.com/gitbrent/PptxGenJS
- [S14] PptxGenJS docs: https://gitbrent.github.io/PptxGenJS/
- [S15] python-pptx docs: https://python-pptx.readthedocs.io/en/latest/
- [S16] Awesome AI PPT ecosystem index: https://github.com/ningzimu/awesome-ai-ppt
- [S17] Data Agent Benchmark: https://arxiv.org/html/2603.20576v1
- [S18] Data Agents taxonomy: https://arxiv.org/html/2602.04261v1
- [S19] DataSciBench: https://arxiv.org/html/2502.13897v1
- [S20] AIDABench: https://arxiv.org/html/2603.15636v1
- [S21] Evidence docs: https://docs.evidence.dev/
- [S22] Observable Framework: https://observablehq.com/framework/
- [S23] Streamlit data/chart docs: https://docs.streamlit.io/develop/api-reference/data
- [S24] DuckDB: https://duckdb.org/
- [S25] SWE-bench GitHub: https://github.com/swe-bench/SWE-bench
- [S26] SWE-bench leaderboard: https://www.swebench.com/
- [S27] OpenHands: https://www.openhands.dev/
- [S28] SWE-agent: https://github.com/swe-agent/swe-agent
- [S29] mini-SWE-agent: https://github.com/SWE-agent/mini-swe-agent
- [S30] Open SWE: https://www.langchain.com/blog/introducing-open-swe-an-open-source-asynchronous-coding-agent
- [S31] SWE-EVO: https://arxiv.org/html/2512.18470v3
- [S32] Hugging Face Hub docs: https://huggingface.co/docs/hub/en/index
- [S33] Hugging Face PresentBench page: https://huggingface.co/papers/2603.07244
- [S34] Hugging Face DAB page: https://huggingface.co/papers/2603.20576
- [L1] Local `ppt-master` README: `/Users/um-yunsang/.codex/tools/ppt-master/README.md`
- [L2] Local `ppt-master` image generator reference: `/Users/um-yunsang/.codex/tools/ppt-master/skills/ppt-master/references/image-generator.md`
