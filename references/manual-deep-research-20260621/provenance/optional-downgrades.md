# Provenance: Optional/Future/Downgraded Sources

Access date: 2026-06-21

## Decisions

- Save:
  - PresentBench: https://presentbench.github.io/ ; https://github.com/PresentBench/PresentBench
  - SlidesGen-Bench: https://arxiv.org/abs/2601.09487 ; https://github.com/YunqiaoYang/SlidesGen-Bench
- Downgrade/future:
  - DataAgentBench: https://ucbepic.github.io/DataAgentBench/ ; https://github.com/ucbepic/DataAgentBench
- Optional image-asset provider context:
  - Adobe Firefly: https://www.adobe.com/products/firefly.html
  - OpenAI image generation guide: https://developers.openai.com/api/docs/guides/image-generation
  - Midjourney: https://www.midjourney.com/

## Evidence Summary

- PresentBench and SlidesGen-Bench are directly on-pivot for `presentation_document_pack`.
- DataAgentBench is a real benchmark, but it is off-pivot for presentation/document generation and belongs to future data-agent/report/dashboard planning.
- Adobe/Midjourney/OpenAI image product pages can support optional image-asset generation/provider-profile context, but they are not core presentation/document evidence and should not block this backlog.

## Claim Mapping

- Supports backlog P2 decisions and downgrades.
- Keeps F-12 image-model/product references optional and distinct from F-6 presentation/document sources.
