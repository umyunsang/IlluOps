# IlluOps Manual Deep-Research References

Created: 2026-06-21

This folder stores manually selected primary-source references for the collection backlog. It intentionally does not use `scripts/archive_references.py` or any broad automated extraction run.

## Layout

- `source-ledger.json`: machine-readable source ledger with claim mapping, priority, retrieval status, and provenance notes.
- `claim_support_matrix.json` / `claim_support_matrix.tsv`: plan/backlog claim-to-source matrix for `F-1` through `F-13`, `R2.*`, `Q29`, `Q37`, and backlog-review rows.
- `r3_direct_edges.json` / `r3_direct_edges.tsv`: direct `source_id -> R3 item` and `claim_id -> R3 item` edges for graph navigation.
- `provenance/`: one markdown provenance note per source family.
- `sources/`: manually saved source copies where direct saving is appropriate and accessible.
- `mirrors/`: reserved for reviewer-approved non-official mirrors; contains only a placeholder unless explicitly justified.
- `notes/`: research notes and downgrade rationale.

## Retrieval Policy

- Official documents are preferred over mirrors.
- If an official source is WAF/403-prone in CLI but reachable via browser/search evidence, record it as `manual_provenance_required` rather than claiming archived content.
- For copyrighted or dynamic pages, store metadata/provenance and short snippets only; do not reproduce full source text unless the source is an official downloadable document intended for redistribution.
- Every saved file must have a ledger row with access date, URL, source family, and support target.

## Storage Modes

1. `saved_pdf` / `saved_artifact`: official source file is stored in `sources/`.
   - Requires `saved_path`, `sha256`, `content_type` or clear file extension, `retrieval_method`, and access date.
   - `sources/checksums.sha256` must include every saved file.
2. `primary_url_confirmed`: official source URL is confirmed, but no full local copy is stored.
   - Requires source card and provenance note.
   - Used for web pages, dynamic docs, or sources where URL/provenance is enough for review.
3. `manual_provenance_required`: official source is current but blocked, WAF/403-prone, or not safely copied.
   - Requires explicit reason and manual provenance note.
   - Must not be described as archived or saved.
4. `download_candidate`: official downloadable file exists but has not yet been stored or checksumed.
   - Must be upgraded to `saved_pdf` after direct save, or downgraded with reason.
5. `optional_context`, `downgraded_future_context`, `watch_item_primary_paper_only`, `draft_or_advisory`, `development_status_primary`.
   - These are tracked but not allowed to satisfy plan-critical requirements unless the support matrix explicitly marks them acceptable for that claim.
6. `mirror_saved`.
   - Allowed only when a reviewer records why official access is unavailable and why the mirror is acceptable.
   - Must label the source as non-official and link the official URL it substitutes.

## Required Metadata

Every source entry requires:

- `id`
- `family`
- `url`
- `priority`
- `supports`
- `status`
- top-level or effective `access_date`
- `provenance_note`

Every saved artifact additionally requires:

- `saved_path`
- `sha256`
- `content_type` or a clear file extension
- `retrieval_method` or capture mode
- `final_url` if redirects occurred
- `http_status` when available

Every non-saved tracked source additionally requires:

- `reason` when access, legal, or quality constraints explain why no copy is stored
- a concrete disposition, not a vague pass/fail label

## Checksum And Fixity

- Use SHA-256 for every saved binary/page artifact.
- Keep `sources/checksums.sha256` in sync with the ledger.
- Do not rewrite saved originals silently; if a file changes, record a new checksum and note why the source was refreshed.

## Copyright And Quote Policy

- Store full copies only for official downloadable documents or sources where local preservation is appropriate.
- For copyrighted or dynamic pages, store metadata and minimal snippets only.
- Snippets are internal support evidence, not republication of the source.
- Do not rely on a fixed word-count rule as a legal safe harbor; keep excerpts minimal and tied to a review purpose.

## Reviewer Acceptance

A reviewer may accept a source row when:

- the ledger row exists and has mandatory metadata;
- the provenance note exists;
- the source card exists under `notes/source-cards/`;
- a saved artifact has a matching SHA-256 when `status` claims a saved file;
- a non-saved source has a concrete reason and does not claim archive success;
- optional/future/draft sources are not used as plan-critical support without explicit matrix rationale.

## Current Manual Collection Snapshot

- Ledger rows: 92.
- Source cards: 92.
- Provenance notes: 8.
- Saved official PDFs: 3, all SHA-256 checked in `sources/checksums.sha256`.
- Claim/backlog matrix rows: 34.
- Plan-critical rows with `replacement_status=none`: 0.
- Current R3 direct-edge coverage: 89 source rows with direct current-R3 edges, 3 off-pivot optional/future context rows intentionally without current-R3 edges, 2,420 direct edge rows, and 102 distinct R3 item targets.

The public-safe matrix is mirrored to `../claim_support_matrix.json` and `../claim_support_matrix.tsv` so older archive-verifier workflows can find the evidence, but this folder remains the source of truth for the manual deep-research pass. It intentionally uses public-safe claim anchors instead of embedding internal `.omo` plan or evidence paths.
