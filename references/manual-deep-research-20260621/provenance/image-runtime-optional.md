# Provenance: Image Runtime and Provider-Profile Context

Access date: 2026-06-21

## Sources

Comfy Cloud:

- https://docs.comfy.org/development/cloud/overview
- https://docs.comfy.org/development/cloud/api-reference
- https://docs.comfy.org/get_started/cloud

Civitai:

- https://developer.civitai.com
- https://developer.civitai.com/site/guide/air

OpenAI image provider profile:

- https://developers.openai.com/api/docs/models
- https://developers.openai.com/api/docs/models/gpt-image-2

## Evidence Summary

- Comfy Cloud sources support the cloud-only runtime and API constraints referenced by `F-4` and `F-5`.
- Civitai developer/AIR sources support model-asset identity and open model ecosystem context for `F-5`.
- OpenAI model pages support optional provider-profile context for `F-12`, but this row is not a replacement for the core `presentation_document_pack` references.

## Claim Mapping

- `F-4`: Comfy Cloud runtime/API surface.
- `F-5`: Civitai AIR/open model ecosystem context.
- `F-12_optional_provider_profile`: optional image provider profile context.

## Manual Storage Decision

- Store source cards and provenance only for these dynamic documentation/product pages.
- Do not use optional provider pages as plan-critical support for the second production pack after the `presentation_document_pack` pivot.
- Keep OpenAI image provider rows separate from the broader optional image product page row in `optional-downgrades.md`.
