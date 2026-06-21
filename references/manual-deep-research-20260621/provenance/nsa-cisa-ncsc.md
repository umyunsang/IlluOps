# Provenance: NSA MCP + CISA/NCSC Secure AI

Access date: 2026-06-21

## Sources

- NSA press page: https://www.nsa.gov/Press-Room/Press-Releases-Statements/Press-Release-View/Article/4496698/nsa-releases-security-design-considerations-for-ai-driven-automation-leveraging/
- NSA PDF: https://www.nsa.gov/Portals/75/documents/Cybersecurity/CSI_MCP_SECURITY.pdf?ver=bmgiSbNQLP6Z_GiWtRt6bg%3D%3D
- NSA PDF alternate media URL: https://media.defense.gov/2026/Jun/02/2003943289/-1/-1/0/CSI_MCP_SECURITY.PDF
- CISA AI landing page: https://www.cisa.gov/ai
- CISA AI use cases: https://www.cisa.gov/ai/cisa-use-cases
- CISA partner AI publications: https://www.cisa.gov/ai/partner-products
- CISA OT secure AI joint guidance page: https://www.cisa.gov/resources-tools/resources/principles-secure-integration-artificial-intelligence-operational-technology
- CISA OT secure AI joint guidance PDF: https://www.cisa.gov/sites/default/files/2026-01/joint-guidance-principles-for-the-secure-integration-of-artificial-intelligence-in-operational-technology-508cV2.pdf
- NCSC secure AI collection: https://www.ncsc.gov.uk/collection/guidelines-secure-ai-system-development
- NCSC secure AI PDF: https://www.ncsc.gov.uk/files/Guidelines-for-secure-AI-system-development.pdf

## Evidence Summary

- NSA published the MCP cybersecurity information sheet on 2026-05-20. The press page identifies the title as "Model Context Protocol (MCP): Security Design Considerations for AI-Driven Automation" and describes MCP trust-boundary and agent misuse risks.
- The NSA PDF is the authoritative guidance artifact. CLI retrieval can return 403 in this environment; preserve this as manual provenance rather than claiming automated archive success.
- CISA AI is the current federal AI hub. Related CISA pages are useful for AI governance and joint publication provenance.
- NCSC secure AI guidance is stable and covers secure design, secure development, secure deployment, and secure operation/maintenance. The PDF is the portable primary source for secure AI lifecycle guidance.

## Claim Mapping

- Supports `R2.12`: security source gate for MCP trust boundaries, secure AI lifecycle, and high-assurance AI integration.
- Supports backlog P0/P1 rows for NSA MCP and CISA/NCSC secure AI replacement.

## Manual Storage Decision

- `status=manual_provenance_required` for NSA official sources until a direct browser/manual official snapshot or reviewer-approved mirror is stored with checksum.
- `status=primary_url_confirmed` for the CISA AI landing page and NCSC collection page.
- `status=saved_pdf` for `sources/ncsc-guidelines-secure-ai-system-development.pdf`; checksum is recorded in `sources/checksums.sha256` and in the ledger.
