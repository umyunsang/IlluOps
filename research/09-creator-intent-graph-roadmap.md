# Creator Intent Graph Roadmap

Date: 2026-06-20

## Product Thesis

The creator should not think in node names first. They should express intent, references, constraints, and iteration goals. The harness translates that into model routes, ComfyUI workflows, CV preprocessing, adapter selection, and QA gates.

## Creator Intent Graph

Use a durable project state object:

```json
{
  "project": {
    "title": "brand character launch set",
    "mode": "comic-and-video-ready-image-pack",
    "license_policy": "commercial-safe"
  },
  "subjects": [
    {"id": "hero", "kind": "character", "refs": ["hero_front", "hero_side"], "consistency": "strict"}
  ],
  "style": {
    "medium": "editorial manga",
    "palette": ["#1D3557", "#F1FAEE", "#E63946"],
    "avoid": ["low contrast", "washed out"]
  },
  "scene": {
    "setting": "small cafe interior",
    "camera": "medium shot",
    "composition": "character on left, product on right"
  },
  "controls": {
    "identity": ["ip_adapter", "lora"],
    "layout": ["depth", "pose"],
    "edit_regions": ["sam_mask"],
    "guidance": {"cfg": "balanced", "sag": "when_supported"}
  },
  "outputs": [
    {"id": "panel_01", "type": "image", "size": "1536x1024"},
    {"id": "motion_seed", "type": "image_to_video_source", "size": "1280x720"}
  ],
  "quality_gates": [
    "identity_consistency",
    "brand_color_delta",
    "ocr_text_if_present",
    "mask_preservation",
    "composition_match"
  ]
}
```

This graph should survive every iteration. The LLM edits the graph. The compiler builds workflows from it.

## User Workflow

1. Intake the brief and references.
2. Normalize assets: crop, mask, caption, palette, detect objects, compute safety/license notes.
3. Build or select a workflow recipe.
4. Compile to ComfyUI API workflow.
5. Validate models, nodes, dimensions, controls, and resource budget.
6. Execute and observe.
7. Evaluate output against gates.
8. Produce a revision plan.
9. Export final image, prompt card, workflow JSON, masks, references, and lineage.

## Mode Roadmap

| Mode | First workflow | Required controls | Output package |
|---|---|---|---|
| Fast ideation | text-to-image fast route | prompt, style, seed, size | image grid, prompt cards |
| Controlled still | ControlNet/reference route | depth/edge/pose, reference, seed | image, controls, workflow |
| Character pack | LoRA/IP/reference route | character refs, LoRA, identity eval | turnarounds, expressions, prompt cards |
| Localized edit | SAM/inpaint route | mask, edit prompt, preservation gate | before/after, mask, edit trace |
| Product/brand scene | product reference route | mask preservation, color QA, text QA | publishable image, brand report |
| Comic panel | multi-image consistency route | character sheet, panel style, OCR gate | panel image, speech text report |
| Image-to-video | Wan/LTX/other video route | start frame, motion prompt, temporal gate | video, keyframes, prompt card |
| Story pack | multi-panel route | characters, style, layout, continuity gate | pages/panels, asset bible |

## Project File Layout

```text
projects/<slug>/
  intent.json
  assets/
    refs/
    masks/
    controls/
  workflows/
    compiled/
    templates/
  runs/
    <run-id>/
      workflow_api.json
      prompt_card.md
      progress.jsonl
      outputs/
      eval.json
  exports/
```

## Roadmap

### Milestone 0: Research lock

- Freeze model and control taxonomy.
- Define intent graph schema.
- Define quality gates and route policy.

### Milestone 1: Template-driven ComfyUI execution

- Support known-good ComfyUI workflows.
- Fill typed slots from intent.
- Execute and collect outputs.
- Save prompt cards and workflow lineage.

### Milestone 2: Control stack presets

- Add LoRA, ControlNet depth/pose/edge, SAM mask, inpaint, IP/reference, upscale presets.
- Add capability indexing and route validation.
- Add output evaluation.

### Milestone 3: Graph composition

- Compose approved subgraphs from intent.
- Add graph patch operations with type checking.
- Add failure recovery and automatic simplification.

### Milestone 4: Personalization

- Add LoRA training projects.
- Track datasets, consent, trigger tokens, overfit tests, license, and eval samples.
- Route final generation through registered LoRAs.

### Milestone 5: Video and comics

- Add multi-image consistency project state.
- Add panel-to-panel character/style checks.
- Add image-to-video routes, temporal consistency checks, and export packs.

## UX Principle

Expose creator controls first, technical controls second:

- "Keep this face consistent" maps to reference adapters, LoRA, and identity eval.
- "Use this sketch layout" maps to depth/edge/pose control images.
- "Only change the jacket" maps to SAM mask, inpaint route, and mask preservation.
- "Make this usable as a comic panel" maps to text/OCR, panel layout, character continuity, and export metadata.

The harness should reveal the generated workflow and technical choices after the plan is compiled, so power users can inspect or override them.

## 2026-06-20 Schema Addendum

Add these fields to the first intent graph schema:

```json
{
  "route_policy": {
    "speed_quality_mode": "draft|balanced|final",
    "privacy": "local_only|hosted_allowed",
    "commercial_license_required": true,
    "max_cost_usd": 5.0,
    "max_runtime_seconds": 600
  },
  "capability_requirements": {
    "requires_lora": false,
    "requires_controlnet": true,
    "requires_sam_masks": true,
    "requires_text_rendering": false,
    "requires_video_ready_export": false
  },
  "review_gates": {
    "human_confirm_custom_node_install": true,
    "human_confirm_likeness_preservation": true,
    "human_confirm_external_api_route": true
  },
  "provenance": {
    "record_workflow_json": true,
    "record_model_hashes": true,
    "record_control_assets": true,
    "attach_c2pa_when_available": true
  }
}
```

This keeps creator intent separate from runtime policy. The LLM may edit creative goals, but route/security/provenance policy must remain explicit and reviewable.
