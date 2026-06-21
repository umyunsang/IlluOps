# Cross-Agent Image Harness Ontology Code Review

Result: FAIL
codeQualityStatus: BLOCK
recommendation: REQUEST_CHANGES

Scope: read-only review of `.omo/plans/cross-agent-image-harness.md` as a design/ontology artifact. No implementation diff exists; workspace is not a git repository.

Skill-perspective check: ran. Loaded `omo:remove-ai-slops` and `omo:programming` before judging maintainability/test relevance. No language-specific programming reference was loaded because this review did not write or review `.py`, `.ts`, `.go`, or `.rs` source. The artifact violates both review perspectives: it retains duplicated/stale state-source language and brittle tests-after/grep-style QA, and it does not make core ontology references typed/ID-resolved enough to be converted safely into tasks.

## Findings

### CRITICAL

None.

### HIGH

1. Source-of-truth language is still contradictory outside the corrected R2/R3 passages.
   R2 and later success criteria say SQLite-WAL is authoritative and JSON/JSONL are exported views (`.omo/plans/cross-agent-image-harness.md:350`, `:754`). But earlier canonical-facing summary and future task criteria still say "file-first durable workspace state" / "file-first durable state" (`:6`, `:20`), Q27 "file-first portable workspace state as source of truth" (`:14`), "file-first manifests" (`:24`), "file-first durable state" in task QA (`:506`), "file-first state layout, atomic-write/file-lock/checksum rules" in CLI acceptance (`:525`), and policy tests for "missing lock, failed atomic rename" (`:655`). A task-conversion agent can still implement file-lock/atomic-rename mechanics as primary state behavior despite R2.4.

2. R3 promises ID-addressable typed links, but the registry and relations use prose names and placeholders.
   R3 says cross-manifest links are by id (`:131`) and relation edges are typed (`:175-191`). The manifest registry then references `domain_pack_registry`, `side_effect_request/result`, `workspace`, `(all)`, and `(root)` instead of `M-*` IDs (`:197-224`). Pack subtype rows use `domain_artifact_manifest`, `review_session_manifest`, `(intake)`, and `(loop)` rather than `M-core-*` IDs (`:244-264`). Relations also use unqualified concepts such as `budget` and `ui_confirmation` that are not defined as entities (`:181`, `:186`). This breaks the ontology's own machine-loadable contract.

3. R3 is not actually standalone despite claiming R3-alone reasoning.
   The status block says R3 is the SSOT and that an agent should be able to load R3 alone (`:116`), while the same sentence gives R2 higher precedence. R3 then embeds volatile external standard claims as canonical standards basis (`:120-125`) and the quick index sends readers back to R2 for verified facts and decisions (`:325`). This is not hidden chat context, but it is hidden non-R3 context; the plan should either make R3 self-contained or weaken the R3-alone claim.

4. Verification strategy encodes tests-after and brittle text/spec assertions.
   The plan explicitly says "tests-after for CLI and adapters" (`:426`), which conflicts with the programming skill's TDD/testing perspective. Task 1 QA also asks to grep for phrases like "MCP server" and "A2A" (`:506`) rather than asserting structured decisions/schema fields. This is overfit-prone and likely to produce maintenance-heavy false confidence.

### MEDIUM

1. Lifecycle modeling is not fully normalized.
   R3 advertises one canonical lifecycle (`:266-280`), but the execution work area tells implementers to build an image-shaped lifecycle beginning `initial_intake_needed -> imported -> validated_original -> patched -> validated_patch ...` (`:579`) and only then restates the generic lifecycle as an addition (`:580`). The branch notation `submitted|running|pending` and `continued|accepted|blocked|stopped` is not a transition table with event IDs/guards, so task authors still need to infer legal edges.

2. Namespace ownership is leaky.
   R3 declares `core.*`, `pack.image.*`, `pack.pres.*`, `proto.*`, and `sec.*` namespaces (`:129`), but the adapter entity uses `core/proto` as its namespace (`:173`), and `proto.*` / `sec.*` have no concrete cataloged entity or manifest IDs in R3. Protocol/security concepts are therefore not consistently owned.

3. Schema testability is incomplete at the canonical layer.
   R3.3 lists "key fields" (`:197`) and says every manifest has `schema_version`/`schema_status` (`:131`), but it does not provide required/optional/cardinality/type definitions in R3. The long candidate schema task later supplies many fields (`:530`), meaning agents cannot derive precise schema tasks from the ontology alone.

### LOW

1. R3 canonical standards content includes unstable external facts.
   The R3-STD section includes current-project-count/governance/protocol-methodology claims such as "60k+ projects" and Linux Foundation stewardship (`:120-125`). The plan has a source-refresh gate elsewhere (`:23`, `:37`), but volatile external facts should not live in the canonical ontology without fact IDs and refresh handling.

## Blockers

- Normalize all remaining file-first / atomic-write / file-lock / checksum-chain language so SQLite-WAL is the only authoritative store and JSON/JSONL are consistently exported views.
- Make R3 mechanically ID-resolved: every relationship domain/range, manifest reference, subtype, alias, and placeholder must use declared `E-*`, `M-*`, `S-*`, `CAP-*`, or `V-*` IDs, or define the missing IDs.
- Decide whether R3 is self-contained. If yes, inline/pin the required R2 decisions as ontology facts; if no, remove the "load R3 alone" claim and make the minimal load set explicit.
- Replace tests-after and grep/text assertions with behavior-first schema/fixture/CLI tests that fail on the relevant contract violation, not on wording.

Final status: BLOCK.
