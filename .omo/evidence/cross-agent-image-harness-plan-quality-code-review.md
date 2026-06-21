# Plan Quality Review: cross-agent-image-harness

Target: `.omo/plans/cross-agent-image-harness.md`
Review mode: design-artifact audit before implementation; plan file not edited.
Input completeness: no diff, changed-file list, evidence bundle, or notepad path was provided. I reviewed the target plan text directly and treated existing evidence/report files as untrusted.

## Verdict

FAIL

codeQualityStatus: BLOCK
recommendation: REQUEST_CHANGES

## Skill-Perspective Check

`omo:remove-ai-slops` and `omo:programming` were consulted before judging maintainability/test relevance. No source-code diff was reviewed, so language-specific implementation references were not loaded. From those perspectives, the plan violates the review criteria conceptually: stale duplicated prose creates conflicting architecture instructions, the plan endorses tests-after for CLI/adapters, and several proposed tests/QA items risk implementation-mirroring or false-confidence checks unless they are rewritten around observable contracts.

## Findings

### CRITICAL

None.

### HIGH

1. State architecture is internally contradictory and implementers could build the wrong persistence layer.

R2 and R3 make SQLite WAL the first-version source of truth and demote JSON/JSONL manifests to exported views (`R2.4`, lines 73-78; invariant `INV-7`, line 281). Later sections still require "file-first portable workspace state" as source of truth with append-only JSONL, atomic writes, file locks, and checksums (Scope line 325; guardrail line 367; Verification line 417; Candidate Task 1 line 471; CLI addition line 489; schema addition line 509; execution addition line 559; final check line 699). This is not just stale wording: it changes storage APIs, concurrency, recovery, tests, failure modes, and effort.

Action: rewrite every Q27/File-first section to SQLite-primary wording: SQLite WAL tables are authoritative; exported JSON manifests/JSONL logs are handoff/finalization/review artifacts; atomic file locks/checksums apply only to exported boundary packages where still required.

### MEDIUM

1. AG-UI terminology conflicts between "inspired" and "normative baseline."

R2.1 says AG-UI is the normative event-shape baseline (line 43), and R2.5 says browser surfaces emit AG-UI-shaped events and the core conforms to MCP/A2A/AG-UI baselines (lines 84-85). Older prose still says AG-UI is merely "inspired" unless strict wire compatibility is approved (line 24), and repeats "AG-UI-inspired" in Scope, verification, and success criteria (lines 328, 684, 719). An implementer could reasonably skip AG-UI event vocabulary conformance.

Action: define the exact compliance level once, e.g. "AG-UI-shaped event vocabulary, not full strict wire compatibility unless approved," and replace the ambiguous "inspired" wording everywhere else.

2. Unverified external benchmark names survive despite R2 fact hygiene.

R2.9 says names not verified to primary sources, including "PresentBench", "WASP", and "AgentDyn", must be downgraded to generic categories or reconfirmed/dropped (line 105). The plan still says AgentDojo/AgentDyn/WASP-style fixtures "must" be part of QA (line 25) and asks the source refresh to verify PresentBench/PPTAgent together (line 471). That creates groundedness debt and could send implementers chasing non-authoritative requirements.

Action: keep AgentDojo and PPTAgent/PPTEval where confirmed, replace the unconfirmed names with generic "current prompt-injection benchmark suite" / "current deck-generation benchmark" wording, or attach primary-source rows before naming them.

3. Candidate work areas are too large and look executable despite the non-executable warning.

The section says candidate work areas are not executable tasks and `/speckit-taskstoissues` is the only task source (lines 465-468), but the same section is formatted as checkbox todos with `Parallelization`, `Commit`, exact acceptance criteria, and QA invocations (for example lines 470-482 and 484-526). Several individual work areas bundle architecture, schemas, security, CLI, state, live-provider policy, UI, and QA into one huge issue. This invites executors to treat them as implementation tasks and undermines the spec-driven workflow.

Action: convert the section into a clearly labeled appendix of source material, remove checkbox/commit affordances, and require `/speckit-taskstoissues` to split by the four R2 epics with implementation+test kept together only after spec approval.

4. Schema-freeze guidance is easy to misread.

R2.7 says generic core schemas are provisional until both image and `presentation_document_pack` ship (lines 93-95), and the execution strategy repeats that schemas freeze only after validation by both packs (lines 441-447). Candidate Task 3 still asks to "Create schemas" and validate a broad schema set in Wave 1 (lines 504-526), including generic core and pack-specific schemas. Without stronger wording, this can freeze an over-generalized or image-shaped schema before the second pack exercises it.

Action: explicitly mark Wave 1 generic schemas as `schema_status: provisional`, limit acceptance to provisional contract/fixture validation, and move "freeze" assertions and compatibility guarantees to Wave 5 after both packs pass.

5. Test strategy conflicts with rigorous test discipline.

The verification strategy states "tests-after for CLI and adapters" (line 401). That conflicts with the programming perspective's preference for behavior-first tests and risks tests that merely mirror the implementation. It also weakens the plan's otherwise strong claim that implementation and tests belong in one issue.

Action: replace this with contract/behavior tests written before or alongside implementation for CLI/adapters, with assertions on observable JSON contracts, state transitions, provider-call gating, and persisted artifacts rather than internal constants.

### LOW

1. Machine TL;DR is stale on effort/risk.

The human TL;DR says XL/High (lines 12-13), but the machine TL;DR still says "Large, medium risk" (line 20). Because this is explicitly machine-facing, it is likely to be consumed by an agent shortcut.

Action: update the machine TL;DR to XL/High or remove the duplicate summary in favor of R2/R3.

2. The plan still contains large duplicated prose blocks.

R3 explicitly exists because the surrounding plan is prose-heavy and duplicated (lines 116-118), but the older Scope, Verification, Candidate Work Areas, Final Verification, and Success Criteria still restate the same contracts repeatedly. This is the source of most conflicts above.

Action: make R2/R3 the short canonical body and move older prose to a conflict-free appendix, or rewrite the lower sections as references to R3 IDs instead of restating full requirements.

## Blockers

- Resolve the SQLite WAL vs file-first/JSONL source-of-truth conflict across Scope, Verification, Candidate Work Areas, Final Verification, and Success Criteria.
- Remove or clearly quarantine stale candidate-task prose so implementers cannot treat it as executable work before `/speckit-taskstoissues`.
- Replace ungrounded benchmark names and ambiguous AG-UI terminology with the R2-grounded canonical wording.

## Residual Risk

No implementation or tests were executed because the task was a document audit. The review is grounded in the plan text, not external source re-verification.
