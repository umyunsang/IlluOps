# Provenance: Presentation Document Pack

Access date: 2026-06-21

## Sources

Research/benchmarks:

- https://arxiv.org/abs/2501.03936
- https://github.com/icip-cas/PPTAgent
- https://presentbench.github.io/
- https://github.com/PresentBench/PresentBench
- https://huggingface.co/datasets/lynnzuo/PresentBench
- https://arxiv.org/abs/2603.07244
- https://arxiv.org/abs/2605.17356
- https://arxiv.org/abs/2601.09487
- https://github.com/YunqiaoYang/SlidesGen-Bench
- https://huggingface.co/datasets/Yqy6/Slides-Align

Tooling:

- https://python-pptx.readthedocs.io/en/latest/user/install.html
- https://python-pptx.readthedocs.io/en/latest/user/quickstart.html
- https://python-pptx.readthedocs.io/en/latest/api/presentation.html
- https://python-pptx.readthedocs.io/en/latest/api/slides.html
- https://github.com/scanny/python-pptx
- https://gitbrent.github.io/PptxGenJS/
- https://gitbrent.github.io/PptxGenJS/docs/quick-start/
- https://gitbrent.github.io/PptxGenJS/docs/api-charts/
- https://gitbrent.github.io/PptxGenJS/docs/masters/
- https://github.com/gitbrent/PptxGenJS
- https://python-docx.readthedocs.io/en/latest/user/quickstart.html
- https://python-docx.readthedocs.io/en/latest/api/table.html
- https://github.com/python-openxml/python-docx
- https://learn.microsoft.com/en-us/office/open-xml/open-xml-sdk
- https://learn.microsoft.com/en-us/office/open-xml/presentation/working-with-presentations
- https://learn.microsoft.com/en-us/office/open-xml/presentation/structure-of-a-presentationml-document
- https://learn.microsoft.com/en-us/office/open-xml/presentation/working-with-presentation-slides
- https://github.com/dotnet/Open-XML-SDK/blob/main/README.md

## Evidence Summary

- PPTAgent/PPTEval, PresentBench, UniPPTBench/UniPPTEval, and SlidesGen-Bench are the core F-6 evidence family.
- PresentBench and SlidesGen-Bench should be saved for current slide-generation benchmark breadth.
- PresentBench and SlidesGen-Bench have dataset/reproducibility surfaces, not only papers.
- UniPPTBench is a watch item because the paper is highly relevant, but a stable public repo was not found in this pass.
- python-pptx/python-docx/PPTXGenJS/Open XML SDK are source-native document-generation/manipulation sources. ReadTheDocs can rate-limit, so GitHub README/source fallback must be recorded.

## Claim Mapping

- Supports `F-6` and `Q37` locked `presentation_document_pack`.
- Supports the decision that the second production pack must produce editable/source-native PPTX/DOCX/Markdown/HTML artifacts rather than image-only outputs.

## Manual Storage Decision

- Save benchmark pages/papers/repos and tooling docs/repo fallback pages.
- Save specific API and dataset pages where they support reproducibility, not only docs homepages.
- Keep DataAgentBench out of the core pack unless later data-agent functionality is explicitly in scope.
