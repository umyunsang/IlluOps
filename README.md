# IlluOps

IlluOps is a planned Cross-Agent LLM Execution Harness for creator workflows.

This repository is no longer a loose research dump. The earlier `research/`, `scaffold/`, and `evidence/` artifacts were removed because they predate the current product boundary.

## Current Plan Lock

Product name:

`IlluOps`

Package and CLI name:

`illuops`

Current planning status:

- The detailed working plan remains local agent workspace material and is not published from this repository.
- No implementation task is authorized until the spec-driven workflow is explicitly resumed.
- `/speckit-taskstoissues` remains the only source of executable implementation tasks.
- Image generation is reference domain pack 1, not the product boundary.
- `presentation_document_pack` is the locked second production domain pack.

## Product Boundary

IlluOps is a reusable execution substrate:

1. Agent Skill packaging for Codex, Claude Code, and compatible agent hosts.
2. Deterministic CLI and state contract.
3. Manifest-first domain-pack registry and allowlist.
4. Out-of-process domain-pack workers.
5. Core capability brokers for side effects.
6. SQLite-WAL durable workspace state with exported JSON/JSONL manifests.
7. Same-host single-writer workspace and provider-job leases.
8. Local actor identity and trust registry.
9. Loopback-only UI sessions with one-time tokens, CSRF, origin checks, and lease revalidation.
10. Optional MCP, A2A, AG-UI, and direct provider adapters.

The core must not import or special-case Civitai, ComfyUI, image-generation, deck, document, or provider-specific logic. All workload behavior enters through a registered domain pack and its manifest.

## Domain Packs

### Reference Pack 1: Image

The image pack proves the domain-pack model with creator workflows around ComfyUI, Civitai/Hugging Face model links, workflow import, typed graph patches, review galleries, visual evaluation, provenance, and final reproducibility packages.

The image pack is open-model and Comfy Cloud/local ComfyUI oriented. Commercial image providers can be used only through explicit provider profiles with credential references, live spend caps, and source-refresh evidence.

### Production Pack 2: Presentation And Document

`presentation_document_pack` must prove the same lifecycle without image-pack assumptions. It must produce editable/source-native artifacts such as PPTX, DOCX, Markdown, and HTML, plus static review/export evidence.

Required artifact families include:

- `brief_manifest.json`
- `outline_manifest.json`
- `source_material_manifest.json`
- `deck_doc_plan.json`
- `deck_doc_artifact_manifest.json`
- `review_session.json`
- `revision_action.json` or `feedback.json`
- `final_package_manifest.json`
- `handoff_manifest.json`

It must reject source-only image exports, hidden instructions in source documents or speaker notes, unsupported active content, missing source-rights evidence, and final packages that cannot map reviewed slides, sections, or pages to final artifacts.

## Four Epics

Epic A: Core substrate and state

- Skill and CLI contract.
- Generic provisional domain lifecycle schemas.
- SQLite-WAL source-of-truth state.
- Exported manifests for handoff, finalization, and cross-agent resume.
- Actor identity, trust, handoff, finalize, and resume-confirmation flow.

Epic B: Isolation, brokerage, and security

- Out-of-process workers.
- Capability broker for file, network, provider, budget, memory, tool, and agent side effects.
- Idempotency, retry, timeout, cancel, and terminal-outcome semantics.
- Worker sandbox policy.
- Supply-chain attestation and AIBOM.
- Localhost UI hardening and spend ledger.

Epic C: Image reference domain pack

- Civitai/HF/AIR resolvers.
- Comfy workflow import.
- Typed graph patches.
- Comfy Cloud thin polling wrapper.
- Local or cloud execution evidence.
- Review gallery and final package proof.

Epic D: `presentation_document_pack`

- Edit-based deck and document generation.
- Source-native editable artifacts.
- Static review and export surfaces.
- Source-material rights checks.
- Revision actions and clean handoff.

## Non-Goals

IlluOps is not:

- A ComfyUI fork.
- A new MCP server as the core product.
- An image app with agent features attached.
- A generic prompt wrapper.
- A code task list before `/speckit-taskstoissues`.
- A system that lets the LLM free-write arbitrary workflow JSON without capability discovery and validation.

## Security Model

Treat all external material as untrusted data, including web content, MCP results, provider outputs, document text, slide notes, image metadata, workflow labels, tool descriptors, A2A messages, and peer-agent cards.

Future implementation must cover:

- Prompt-carrier isolation and taint maps.
- MCP tool poisoning and untrusted annotations.
- OWASP agentic and LLM risk categories.
- AgentDojo and AgentDyn-style injection fixtures.
- Worker sandbox limits and broker-only side effects.
- Secret redaction and no raw secret retention.
- Provider-profile caps and spend-abuse controls.
- Retention classes, proof-gated cleanup, and tombstone events.

## Repository Contents

- [LICENSE](LICENSE): Apache-2.0 license for this repository's own content.
- [LICENSES/README.md](LICENSES/README.md): license and third-party asset boundaries.

Local agent/session artifacts such as `.omo/`, `.codex/`, `codex/`, `.claude/`, `.claude-code/`, and `claude-code/` are intentionally ignored and must not be treated as publishable source.
