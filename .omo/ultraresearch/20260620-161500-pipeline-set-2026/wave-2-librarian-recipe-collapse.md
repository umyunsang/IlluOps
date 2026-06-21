# Wave 2: Paper-Driven Recipe Collapse

Worker: `019ee3f8-c1e9-7550-ab0c-d28b578843fb`

## Bottom Line

The broad 2026 paper landscape should not become eight MVP recipes. It collapses into a smaller set:

1. General create/edit.
2. Localized edit / selection-guided edit.
3. Reference-conditioned composition, with multi-reference as the default direction.
4. Optional high-resolution refine/upscale.

Identity, product consistency, text rendering, style transfer, and OCR fidelity should be modeled as recipe variants or quality gates, not as first-wave standalone recipes.

## Why This Collapse Is Defensible

- Current model families increasingly unify text-to-image generation and image editing.
- Localized/selection editing remains operationally distinct because it depends on masks, boxes, or selection prompts and has a stronger outside-region preservation contract.
- Reference-conditioned generation/editing is now a central capability family rather than a niche add-on.
- High-resolution editing/refinement is a downstream production pass with different compute and QA constraints.

## Worker Evidence

- OpenAI image product docs expose generation and editing in one user-facing surface.
- Qwen-Image-Edit extends Qwen-Image into editing and integrates T2I/TI2I/I2I-style tasks.
- FLUX.2 frames multi-reference control as a central product/character/style consistency surface.
- MultiBanana formalizes multi-reference generation/editing as a 2026 benchmark area.
- High-resolution editing papers treat high-res edit/refine as a distinct task or downstream pass.
- PICABench/PhyEdit indicate physical realism is specialized and should stay roadmap-only.

## Harness Interpretation

For the ComfyUI harness, paper-level recipes should be translated into implementation recipes:

- `controlled_t2i`: text-to-image with optional LoRA, ControlNet/spatial control, IPAdapter/reference, and guidance nodes.
- `controlled_img2img`: image-to-image with denoise control plus the same adapter/control slots.
- `localized_edit`: user mask or SAM/Grounded-SAM/SAM3 mask, inpaint/outpaint/detailer path.
- `reference_composition`: one or more references for style, subject, character, product, or layout consistency.
- `refine_upscale`: optional production pass for high resolution, detail, and artifact repair.

This preserves creator-facing clarity while matching the 2026 research trend toward unified generation/editing and multi-reference conditioning.

## Open Result

The worker ended with `EXPAND`, so this is not a final decision by itself. It became the hypothesis tested by direct source verification and later waves.
