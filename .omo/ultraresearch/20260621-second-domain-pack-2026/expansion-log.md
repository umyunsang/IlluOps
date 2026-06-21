# Expansion Log: Q37 Second Non-Image Domain Pack 2026

Date: 2026-06-21

## Phase 0 Decomposition

Core question: Which real second non-image production domain pack should ship with v1 of the Cross-Agent LLM Execution Harness so the product remains a general execution harness rather than an image-generation app?

Axes covered:
- Cross-agent execution architecture: durable workflows, human interrupts, tool/data protocols, agent-to-agent protocols, browser event surfaces, observability, and long-running job safety.
- Presentation/document production: slide-generation papers, PPTX-native libraries, AI PPT open-source ecosystem, local `ppt-master` pattern, and creator review/finalization fit.
- Data/report/dashboard production: 2026 data-agent benchmarks, BI-as-code tools, embedded analytics engines, chart/table/report review, and evaluation complexity.
- Code/repo automation: SWE-bench, SWE-EVO, OpenHands, SWE-agent, Open SWE, PR/CI workflow fit, sandbox and security blast radius.
- Hubs and ecosystem signals: Hugging Face papers/datasets/spaces, GitHub repos/awesome lists, official docs for reusable runtimes and artifact platforms.

Codebase relevant: yes, plan artifacts only. External: yes. Browsing: yes. Verification likely: source-level only; no product code exists in this planning workspace. Report requested: Markdown synthesis plus Q37 plan update.

## Wave 1 Sources Opened

Primary/current source families:
- LangGraph docs for long-running, stateful agent orchestration, durable execution, streaming, persistence, and human-in-the-loop interrupts.
- Temporal docs for durable workflow streams, signals/updates/queries, retries, idempotent activities, long-running workflow state, and AI SDK integration.
- OpenAI Agents SDK docs for application-owned orchestration, tool execution, approvals, state, handoffs, guardrails, and tracing.
- MCP 2025-11-25 specification for LLM application to tools/data integrations.
- A2A GitHub/spec surface for agent-to-agent task/artifact interoperability.
- AG-UI docs for event-based agent-to-user frontends.
- OpenTelemetry GenAI docs/blog for model/tool-call observability fields.
- Hugging Face Hub docs for papers, models, datasets, Spaces, agent features, and open collaboration surfaces.
- PresentBench, PPTAgent, and PPTX-native generation library docs for presentation/document production.
- DAB, Data Agents taxonomy, DataSciBench, AIDABench, Evidence, Observable Framework, Streamlit, and DuckDB for data/report/dashboard production.
- SWE-bench, SWE-EVO, OpenHands, SWE-agent, mini-SWE-agent, and Open SWE for code/repo automation.
- Local `ppt-master` README/reference docs for provider-specific credential profiles, idempotent manifest generation, preview/export, and native editable PPTX packaging.

## Leads Opened And Closed

- LEAD: Should Q37 select code/repo automation because it is closest to "agent execution"? CLOSED: code/repo automation is the strongest agent-ops proof, but it overlaps with the host coding agent itself, requires sandbox/PR/CI/governance, and SWE-EVO shows long-horizon multi-file software evolution remains difficult. It is too high-blast-radius for the first non-image pack.
- LEAD: Should Q37 select data/report/dashboard because it has strong structured artifacts? CLOSED: data/report is attractive and should become a later pack, but 2026 data-agent benchmarks emphasize multi-database joins, domain knowledge, unstructured text transformation, and end-to-end analytics failure modes. It needs richer fixtures/contracts than Q37 should impose on v1.
- LEAD: Does presentation/document generation have enough 2026 research maturity to be a real production pack, not a toy? CLOSED: PresentBench and PPTAgent show slide generation is now evaluated through fine-grained rubrics, content/design/coherence, and human-like edit workflows; PPTX-native libraries and AI PPT skill ecosystems provide concrete editable artifacts, preview, revision, final package, and handoff patterns.
- LEAD: Does presentation/document pack prove the generic harness lifecycle without image assumptions? CLOSED: yes, if scoped as `presentation_document_pack` with brief -> outline -> deck/doc artifact -> localhost review -> revision action -> final package -> handoff, using document-native artifacts and no Civitai/Comfy/gallery/graph-patch fields.
- LEAD: Should the pack use image-first deck generation because `gpt-image-2` is already referenced by `ppt-master`? CLOSED: no. `gpt-image-2` may be a brokered conceptual-support asset provider, but the domain pack must remain document/PPTX-native or source-native. Final proof is editable deck/doc package plus review/finalization manifests, not whole-slide images.

## Convergence

Convergence reason: all three Q37 candidates were checked against 2026 architecture evidence, current papers/benchmarks, official framework/library docs, OSS/hub signals, and the existing local `ppt-master` reference. The evidence supports one best-practice default: recommend Q37 option `a`, a presentation/document production pack, while recording data/report as pack 3 and code/repo automation as a later high-security pack.
