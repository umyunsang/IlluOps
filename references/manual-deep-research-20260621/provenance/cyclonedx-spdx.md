# Provenance: CycloneDX ML-BOM + SPDX AI/Dataset

Access date: 2026-06-21

## Sources

- CycloneDX ML-BOM page: https://cyclonedx.org/capabilities/mlbom/
- CycloneDX AI/ML-BOM guide PDF: https://cyclonedx.org/guides/OWASP_CycloneDX-Authoritative-Guide-to-AI-ML-BOM-en.pdf
- CycloneDX specification overview: https://cyclonedx.org/specification/overview/
- OWASP CycloneDX project: https://owasp.org/www-project-cyclonedx/
- ECMA-424: https://ecma-international.org/publications-and-standards/standards/ecma-424/
- CycloneDX newsroom: https://cyclonedx.org/news/
- SPDX AI profile: https://spdx.dev/learn/areas-of-interest/ai/
- SPDX Dataset profile: https://spdx.dev/learn/areas-of-interest/dataset/
- SPDX areas of interest: https://spdx.dev/learn/areas-of-interest/
- SPDX v3.0.1 PDF: https://spdx.dev/wp-content/uploads/sites/31/2024/12/SPDX-3.0.1-1.pdf
- CycloneDX v1.7 JSON schema: https://raw.githubusercontent.com/CycloneDX/specification/master/schema/bom-1.7.schema.json
- Legacy CycloneDX ai-bom URL: https://cyclonedx.org/capabilities/ai-bom/
- ECMA-424 1st edition PDF: https://ecma-international.org/wp-content/uploads/ECMA-424_1st_edition_june_2024.pdf
- SPDX model README: https://raw.githubusercontent.com/spdx/spdx-3-model/develop/README.md
- SPDX model changelog: https://raw.githubusercontent.com/spdx/spdx-3-model/develop/CHANGELOG.md

## Evidence Summary

- The stale CycloneDX `/capabilities/ai-bom/` URL should be replaced with the current `/capabilities/mlbom/` page.
- The current public naming mixes `ML-BOM`, `AI/ML-BOM`, and `AI-BOM`; this naming drift should be preserved in the ledger.
- CycloneDX ML-BOM supports model/dataset transparency, dataset provenance, training methodology, and AI framework configuration.
- SPDX AI and Dataset profiles are accepted alternate/adjacent standards for AI/dataset transparency.
- CycloneDX v1.7 schema grounds the narrative ML-BOM page in concrete fields: `machine-learning-model`, `modelCard`, `data`, and `componentData`.
- Legacy `/capabilities/ai-bom/` is obsolete/404 and should be retained only as a failure-to-replacement note.
- ECMA-424 current landing page represents the current standard; older first-edition PDF is historical context.

## Claim Mapping

- Supports `F-13`, `R2.10`, and `R2.12`.
- Resolves backlog P1 stale CycloneDX `ai-bom` row with current sources.

## Manual Storage Decision

- Save CycloneDX ML-BOM page and AI/ML-BOM guide PDF as the primary replacement pair.
- Save ECMA/SPDX pages as standards pinning support.
- Save schema/raw model references as precise support for field-level AIBOM claims.
