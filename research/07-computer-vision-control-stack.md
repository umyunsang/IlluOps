# Computer Vision Control Stack

Date: 2026-06-20

## Goal

The harness should let a creator control image generation through computer vision operations, not only through text prompts. The practical stack is:

```text
creator intent
  -> reference assets
  -> CV extraction
  -> control graph
  -> model/adapters/guidance
  -> generation
  -> visual evaluation
  -> edit/refine/export
```

## Control Layers

| Layer | Representative tools | What it gives the creator | Harness responsibility |
|---|---|---|---|
| Reference intake | image upload, metadata extraction, CLIP/VLM captioning, palette extraction | "Use this character/style/product/layout" | Store references with purpose, license, consent, and reuse scope. |
| Open-vocabulary detection | Grounding DINO, Grounded-SAM, SAM3 text prompts | Text-addressable objects and regions | Convert natural language targets into boxes and masks. |
| Segmentation and masks | SAM3, SAM2, Grounded-SAM, matting nodes | Precise edit regions, foreground/background, object masks | Version masks and tie them to edit operations. |
| Geometry controls | Canny, HED, depth, normal, pose, lineart, scribble, segmentation maps | Composition and structure control | Generate control images, validate size/alignment, attach to workflow slots. |
| Adapters | ControlNet, T2I-Adapter, IP-Adapter, style adapters | Strong conditioning beyond text | Match adapter to base model and route. |
| Personalization | LoRA, DreamBooth, textual inversion, LyCORIS-style variants | Character, brand, style, product memory | Train/register adapters, track trigger tokens, license, and overfit tests. |
| Guidance | CFG, SAG, PAG, self-guidance, negative prompts, schedule control | Prompt adherence, detail, realism, or style bias | Use policy presets and avoid uncontrolled parameter search. |
| Sampling | scheduler, sampler, steps, denoise, seed, latent size | Reproducibility and quality/speed tradeoff | Keep deterministic seeds, trace every generation, and expose repeat/remix. |
| Post-processing | upscalers, face/detail restoration, color grading, background removal, OCR repair | Publishable output | Treat post-processing as explicit workflow nodes with QA gates. |
| Evaluation | OCR, color delta, CLIP/VLM scoring, identity check, mask preservation, human review | Confidence that image matches intent | Block export when core constraints fail. |

## Control Graph Schema

Use a small intermediate schema before lowering to ComfyUI JSON:

```json
{
  "task": "character-consistent-comic-panel",
  "intent": {
    "subject": "original character",
    "style": "clean editorial manga",
    "composition": "medium shot, cafe interior",
    "output": {"width": 1536, "height": 1024, "format": "png"}
  },
  "references": [
    {"id": "char_front", "role": "identity", "path": "refs/char_front.png"},
    {"id": "style_board", "role": "style", "path": "refs/style_board.png"}
  ],
  "controls": [
    {"type": "sam_mask", "target": "character", "source": "char_front"},
    {"type": "depth", "source": "layout_sketch"},
    {"type": "pose", "source": "pose_ref"},
    {"type": "lora", "name": "creator_character_v3", "weight": 0.75}
  ],
  "quality_gates": [
    "mask_preservation",
    "identity_consistency",
    "text_ocr_if_present",
    "composition_match"
  ]
}
```

The LLM may author this schema. The harness compiler, not the LLM alone, should lower it to ComfyUI node IDs and links.

## ComfyUI Mapping

| Harness concept | ComfyUI control surface |
|---|---|
| Base model route | checkpoint, UNet, diffusion model loader, VAE, CLIP/T5 text encoders |
| Prompt and negative prompt | text encoder nodes |
| Seed and sampler policy | sampler nodes and scheduler fields |
| LoRA | LoRA loader nodes and model/clip links |
| ControlNet | preprocessor nodes plus ControlNet apply nodes |
| IP/reference adapters | IP-Adapter or model-specific reference nodes |
| SAM masks | upload image/mask plus segmentation custom nodes |
| Inpaint/edit region | mask, inpaint conditioning, denoise strength, crop/compose nodes |
| Upscale/fix | upscale, tiled diffusion, face/detailer, color nodes |
| Output | SaveImage, preview, metadata, history retrieval |

## Minimum Useful Tool Contracts

Implement these tool contracts before adding advanced UX:

| Tool | Input | Output |
|---|---|---|
| `index_capabilities` | ComfyUI base URL | nodes, models, templates, system stats, missing capabilities |
| `analyze_asset` | image path and role | dimensions, masks, palette, caption, detected objects, risks |
| `compile_control_graph` | creator intent graph | candidate ComfyUI workflow JSON plus explanation |
| `validate_workflow` | workflow JSON | schema, missing nodes, missing models, invalid links, resource estimate |
| `execute_workflow` | validated workflow JSON | prompt id, progress trace, outputs, metadata |
| `evaluate_output` | output paths and gates | pass/fail, scores, suggested next edit |
| `export_project` | generation state | prompt card, workflow JSON, assets, output bundle |

## Practical Presets

Start with presets that map to real creator tasks:

- `text_to_image_fast`: fast draft generation.
- `reference_style_image`: style reference plus prompt.
- `character_consistency`: reference identity plus LoRA/IP-Adapter.
- `controlled_composition`: sketch/pose/depth/edge ControlNet.
- `localized_edit`: SAM mask plus inpaint/edit model.
- `product_scene`: product reference, mask preservation, brand color QA.
- `comic_panel`: character sheet, panel style, dialogue/OCR gate.
- `image_to_video`: still frame to video route with temporal consistency checks.

## Hard Boundaries

- Do not train LoRA without dataset consent, license, and subject policy.
- Do not allow arbitrary custom node installation from the LLM without allowlist and review.
- Do not execute workflow JSON before node, model, and resource validation.
- Do not rely on prompt adherence for protected constraints such as brand color, text spelling, identity, or edit masks; evaluate them.
- Do not hide model license restrictions from creator-facing route selection.

## 2026-06-20 Multi-Control Composition Addendum

Recommended compiler ordering:

```text
base model
  -> LoRA/adapters
  -> spatial controls: ControlNet / T2I-Adapter / ControlNeXt / OminiControl
  -> mask gates from SAM3 / Grounded-SAM
  -> IP/reference conditioning
  -> sampling guidance: CFG / SAG / PAG
```

Rules:

- Lock the base model first. Adapter and control compatibility is model-family specific; do not assume UNet-era ControlNet ordering transfers unchanged to DiT/FLUX-style backbones.
- Treat LoRA as an early style/identity modifier. Multiple LoRAs should have explicit weights and precedence; avoid stacking competing identity/style adapters at full strength.
- Keep spatial controls separated by region or semantic role. If depth, pose, edge, and inpaint masks overlap, reduce per-control scale and prefer explicit priority.
- Use SAM3/Grounded-SAM as mask and ROI generators. They produce candidate boxes/masks for downstream editing/control; they are not ground-truth semantic oracles.
- Treat IP-Adapter and other reference-image controls as global/identity/style signals. If reference conditioning conflicts with geometry, geometry should win unless the task is explicitly subject-centric.
- Apply SAG/PAG/other sampling-time guidance last and keep it mild when spatial controls are already strong. Strong guidance can amplify artifacts or overbind to mask/control boundaries.

Conflict classes to test:

- Overlapping spatial maps.
- Multiple LoRAs targeting the same concept.
- Reference-image identity versus pose/layout control.
- Strong guidance combined with strong ControlNet scale.
- Model-family mismatch between adapter/control checkpoint and base model.
