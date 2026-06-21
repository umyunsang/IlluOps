# Cross-Agent Image Harness Code-Quality Review

Target: `.omo/plans/cross-agent-image-harness.md`
Review mode: read-only design-artifact audit after latest revisions.
Date: 2026-06-21

## Verdict

FAIL

codeQualityStatus: BLOCK
recommendation: REQUEST_CHANGES

reportPath: `.omo/evidence/cross-agent-image-harness-code-review.md`

## Scope And Evidence

- Reviewed the current plan file: `.omo/plans/cross-agent-image-harness.md`.
- Reviewed existing evidence under `.omo/evidence`, including prior plan-quality and ontology reviews.
- No git diff, changed-file list, full diff, or notepad path was available. The workspace root is not a git repository, so this review is grounded in current file contents and evidence artifacts only.
- Existing evidence was treated as untrusted until checked against the current plan.

## Skill-Perspective Check

Ran before judging maintainability and test relevance.

- Loaded `omo:remove-ai-slops`: applied its overfit/slop lens to production-design prose and test/QA plans. The current plan still violates this perspective where tests are specified as tests-after and where QA can pass by matching wording instead of behavior.
- Loaded `omo:programming`: applied its testing and schema/typing discipline. No language-specific references were loaded because this review did not inspect or edit `.py`, `.ts`, `.go`, or `.rs` source. The current plan still violates the programming perspective where typed ontology promises are not mechanically ID-resolved and where test sequencing is explicitly tests-after.

## Previous Evidence Items Now Fixed

- The major SQLite-WAL vs file-first source-of-truth blocker is mostly fixed. Current text makes SQLite-WAL authoritative and JSON/JSONL exported views in the human TL;DR, decision list, R2.4, Scope, Verification, final checks, and success criteria (`cross-agent-image-harness.md:6`, `:14`, `:20`, `:74-78`, `:378`, `:470`, `:753`, `:783`).
- The prior stale effort/risk machine summary is fixed: the machine TL;DR now says XL effort and High risk (`cross-agent-image-harness.md:20`).
- The prior AG-UI ambiguity is fixed enough for planning: R3-PROTO defines AG-UI as normative event vocabulary with strict wire compatibility optional (`cross-agent-image-harness.md:301-311`), and the older "AG-UI-inspired" phrasing no longer appears.
- The prior executable-looking task section is materially quarantined: Appendix A now states the checkboxes, waves, acceptance criteria, QA, evidence paths, and commit lines are illustrative source material only and not executable tasks (`cross-agent-image-harness.md:518-523`).
- The prior generic-schema-freeze risk is improved: R2.7 and the execution strategy mark generic schemas provisional until both packs ship (`cross-agent-image-harness.md:93`, `:245`, `:494-501`).
- The prior unverified benchmark/name issue is improved: current text confirms AgentDojo, PPTAgent/PPTEval, and PresentBench while downgrading unverified names to generic categories (`cross-agent-image-harness.md:25`, `:103-106`).

## Findings

### CRITICAL

None.

### HIGH

1. R3 still promises machine-loadable typed IDs but uses prose references and placeholders.

R3 declares ID forms and says cross-manifest links are by id (`cross-agent-image-harness.md:143-146`). The relationship catalog still uses unqualified concepts rather than declared IDs, including `harness`, `provider_job`, `action`, `budget`, and `ui_confirmation` (`cross-agent-image-harness.md:190-204`). The manifest registry references prose names and placeholders such as `adapter_registry`, `security_inventory`, `side_effect_request/result`, `(all)`, and `(root)` instead of `M-*` IDs (`cross-agent-image-harness.md:214-243`). Pack subtype rows still use `domain_artifact_manifest`, `domain_plan_manifest`, `(intake)`, and `(loop)` rather than declared core manifest IDs or newly defined IDs (`cross-agent-image-harness.md:259-279`).

Impact: a task-conversion or schema-generation agent cannot safely build a graph, JSON-LD triples, or typed manifest schemas without inventing missing IDs. This is a direct blocker for the requested LLM-friendly ontology and machine-loadable schema readiness.

2. Test discipline still conflicts with behavior-first, contract-focused QA.

The verification strategy explicitly says "tests-after for CLI and adapters" (`cross-agent-image-harness.md:454`). Appendix QA still includes wording/grep-style checks, for example verifying by grep that "MCP server" and "A2A" are not used incorrectly (`cross-agent-image-harness.md:535`). Even though Appendix A is non-executable, `/speckit-taskstoissues` is instructed to consume it as source material, so brittle QA can still be converted into tasks.

Impact: this creates false confidence and implementation-mirroring tests. The plan should require behavior-first contract/schema/fixture tests for observable JSON contracts, state transitions, permissions, and persisted artifacts, not wording checks or tests written after implementation.

### MEDIUM

1. R3 canonicality is internally inconsistent.

R3 says it is the SSOT for every concept, manifest, state, relationship, capability, and invariant, while the same status block gives R2 higher precedence and says an agent should be able to load R3 alone (`cross-agent-image-harness.md:131`). R3 then points back to R2 for verified external facts and resolved decisions (`cross-agent-image-harness.md:353`).

Impact: R3 can be a canonical ontology or an ontology layer subordinate to R2, but the current wording claims both. Agents following the "load R3 alone" instruction can miss R2 decisions that R3 itself says outrank it.

2. Lifecycle states are not yet a machine-loadable transition table.

R3 defines the lifecycle as a single string with branch notation, not as declared `S-*` rows with legal transitions, event IDs, and guard references (`cross-agent-image-harness.md:281-283`). Later source material still contains an image-shaped lifecycle sequence as implementation source material (`cross-agent-image-harness.md:608`).

Impact: agents must infer whether `submitted`, `running`, and `pending` are alternatives, substates, or concurrent states; they must also infer legal edges for `continued|accepted|blocked|stopped`. This is not ready for deterministic schema or state-machine generation.

3. Canonical manifest rows are still key-field sketches, not schema-ready definitions.

R3.3 lists "key fields" but does not define required/optional fields, cardinality, scalar types, enum IDs, link target IDs, or version/freeze rules at the row level (`cross-agent-image-harness.md:208-254`). The detailed schema material exists later in Appendix A as prose source material (`cross-agent-image-harness.md:558-570`), not in the canonical ontology.

Impact: R3 is much clearer than the old prose, but it is not yet sufficient for a generator to emit JSON Schema/Zod/Pydantic contracts without re-reading non-canonical prose and making judgment calls.

### LOW

1. One residual "file-first/SQLite manifests are authoritative" phrase should be normalized.

R2.10 still says "file-first/SQLite manifests are authoritative" (`cross-agent-image-harness.md:110`). This is no longer a blocking contradiction because R2.4, INV-7, Scope, QA, and success criteria consistently make SQLite-WAL authoritative and JSON/JSONL exported views, but this phrase should be rewritten to avoid reintroducing the previous ambiguity.

2. R3 standards basis includes volatile external facts in the canonical layer.

R3-STD embeds current external ecosystem claims such as AGENTS.md project count and stewardship (`cross-agent-image-harness.md:135-141`). The plan has a source-refresh gate, so this is not a blocker, but volatile claims would be safer as fact IDs with refresh policy rather than canonical ontology content.

## Blockers

- Make R3 mechanically ID-resolved: every relationship domain/range, manifest reference, subtype, alias, and placeholder must use declared `E-*`, `M-*`, `S-*`, `CAP-*`, `V-*`, or `R-*` IDs, or define the missing IDs.
- Replace "tests-after for CLI and adapters" with behavior-first contract/schema/fixture test discipline and remove grep/text-only QA from the source material before `/speckit-taskstoissues` converts it.

Final status: BLOCK.
