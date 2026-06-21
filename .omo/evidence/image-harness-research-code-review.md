# Image Harness Research Code Review

Date: 2026-06-20

Verdict: APPROVE

codeQualityStatus: WATCH
recommendation: APPROVE
blockers: none

## Scope Reviewed

- `.omo/ultraresearch/20260620-154100-image-harness/SYNTHESIS.md`
- `.omo/ultraresearch/20260620-154100-image-harness/verify-comfyui-control.md`
- `.omo/ultraresearch/20260620-154100-image-harness/expansion-log.md`
- `research/00-source-index.md`
- `research/06-image-generation-model-trends-2026.md`
- `research/07-computer-vision-control-stack.md`
- `research/08-comfyui-full-control-analysis.md`
- `research/09-creator-intent-graph-roadmap.md`
- `research/10-recent-paper-watchlist-2026.md`
- `evidence/image-generation-findings.md`
- `evidence/local-findings.md`
- `.omo/ulw-loop/019ee3bd-cc58-7d40-9c0f-1886d00759a0/goals.json`

## Skill Perspective Check

Ran the required skill-perspective check by loading:

- `omo:remove-ai-slops`: consulted for overfit/slop review. No production/test diff exists; no deletion-only tests, tautological tests, implementation-mirroring tests, or unnecessary production parsing/normalization were introduced by this research deliverable.
- `omo:programming`: consulted for maintainability criteria. No `.py`, `.ts`, `.tsx`, `.rs`, or `.go` implementation diff is under review. The proposed architecture is consistent with typed boundaries, parse-at-boundary discipline, and avoiding direct LLM free-form workflow mutation.

Result: no violation of either skill perspective. The review concerns below are research coverage/bookkeeping risks, not code slop.

## Findings

### CRITICAL

None.

### HIGH

None.

### MEDIUM

1. Existing-attempt coverage is incomplete for a user-critical question.

   The synthesis says the closest public project is `artokun/comfyui-mcp`, then lists a small set of narrower projects and concludes no public universal harness was found (`SYNTHESIS.md:31`, `SYNTHESIS.md:33`, `SYNTHESIS.md:35`, `SYNTHESIS.md:42`). The verification/source index similarly records only `artokun`, `LingyiChen-AI`, `MieMieeeee`, and HuangYuChuh projects (`verify-comfyui-control.md:109`, `verify-comfyui-control.md:124`, `verify-comfyui-control.md:126`, `research/00-source-index.md:140`, `research/00-source-index.md:144`, `research/00-source-index.md:145`, `research/00-source-index.md:146`, `research/00-source-index.md:147`).

   Current direct checks found additional relevant public attempts that should be in the source map before implementation planning: `AIDC-AI/Pixelle-MCP` (ComfyUI + MCP + LLM, workflow-as-MCP-tool, 1053 stars), `joenorton/comfyui-mcp-server` (local ComfyUI MCP server, 360 stars, and the wrapper cited by ComfyUI issue #7780), `21Pdontno/comfyui-workflow-skills` (Agent Skill-style workflow lifecycle automation), plus smaller skill repos. These omissions do not overturn the conclusion, because they still appear to be partial wrappers rather than a formal full-control, cross-agent, security/eval/provenance harness. They do weaken the "aggressive current research" coverage for existing attempts.

### LOW

1. ULW bookkeeping remains open even though all criteria are recorded pass.

   `goals.json` records C001, C002, and C003 as `pass` (`goals.json:23`, `goals.json:33`, `goals.json:43`), but the goal itself remains `status: "in_progress"` and `activeGoalId` still points to it (`goals.json:14`, `goals.json:54`). This is a process artifact risk: a later reviewer could misread the ULW run as unfinished despite the criterion evidence.

2. `T2I-CoReBench` is correctly flagged as unverified in the synthesis, but still appears as a normal watchlist source elsewhere.

   The synthesis says the exact `T2I-CoReBench` primary page could not be verified and should remain unverified (`SYNTHESIS.md:83`, `SYNTHESIS.md:190`). `research/10-recent-paper-watchlist-2026.md` still presents it in the evaluation table and "what to read first" list before the later caveat (`research/10-recent-paper-watchlist-2026.md:43`, `research/10-recent-paper-watchlist-2026.md:61`, `research/10-recent-paper-watchlist-2026.md:81`). This is not blocking because the final synthesis does not rely on it as a core gate, but the watchlist should mark it unverified inline.

3. Evidence contains small drift/inconsistency in third-party repo counts.

   `verify-comfyui-control.md` records `artokun/comfyui-mcp` as 88 tools / 14 skills (`verify-comfyui-control.md:126`), while `research/08-comfyui-full-control-analysis.md` records 89 tools / 22 skills (`research/08-comfyui-full-control-analysis.md:144`). Current README content advertises an even larger tool count later in the file. This is normal repo drift and does not affect the architectural conclusion, but exact counts should be avoided unless pinned to a commit or capture timestamp.

## ComfyUI Conclusion Defensibility

Defensible.

The central conclusion is properly bounded: use ComfyUI as graph runtime and build a skill-first controller above it (`SYNTHESIS.md:9`, `research/08-comfyui-full-control-analysis.md:9`, `research/08-comfyui-full-control-analysis.md:24`). The deliverable does not overclaim universal semantic control; it explicitly says metadata cannot prove arbitrary custom-node behavior or safety (`SYNTHESIS.md:13`, `verify-comfyui-control.md:64`, `research/08-comfyui-full-control-analysis.md:42`, `research/08-comfyui-full-control-analysis.md:216`).

Current external verification supports the main evidence:

- ComfyUI master still matches the recorded SHA `dc3f8f314a987d23115ed278693e76cf6e72a5a0`, with the same GitHub metadata class recorded in `verify-comfyui-control.md:20`.
- ComfyUI issue #8899 and PR #13094 are still open and still concern `/prompt` JSON Schema (`verify-comfyui-control.md:80`, `verify-comfyui-control.md:86`).
- Official Comfy docs expose Agent Tools / MCP, Cloud MCP closed beta, Partner MCP private preview, server routes, and API workflow format, matching `SYNTHESIS.md:25`, `SYNTHESIS.md:27`, and `SYNTHESIS.md:29`.
- Current `server.py` directly supports the route/control claims around `/object_info`, `/prompt`, `/history`, `/queue`, upload/view, and `/free`.

The conclusion should be phrased as "no public project found that fully combines all required dimensions" rather than "no public attempt exists"; the synthesis already mostly uses that bounded form.

## Criteria Coverage

- C001: PASS. The synthesis and research files exist and cover image models, CV controls, ComfyUI control, existing attempts, architecture, security, evaluation, and roadmap. The one caveat is incomplete public-attempt coverage, rated MEDIUM.
- C002: PASS with WATCH. The contested ComfyUI feasibility and issue/PR claims are backed by current docs, GitHub issue/PR state, and source-level route inspection. The conclusion is defensible.
- C003: PASS. The scaffold remains aligned with the research plan and no implementation code was modified. The ULW goal bookkeeping should be closed separately.

## Final Recommendation

APPROVE for the research deliverable. Before turning this into an implementation spec, add the omitted repo attempts to the source index and update the existing-attempts section so future planning does not miss `Pixelle-MCP`, `joenorton/comfyui-mcp-server`, and `21Pdontno/comfyui-workflow-skills`.
