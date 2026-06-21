# Wave 1: Product and Platform Reference Semantics

Date: 2026-06-20

## Axis

How do current creator-facing image tools treat a reference image: exact copy, style/mood signal, identity/object control, structural control, or source image for editing?

## Findings

Reference semantics are explicitly split across modern tools:

- Midjourney Style Reference is for visual style, not object/person copying. It has a style-weight control and can be combined with other reference types.
- Midjourney Character Reference and Omni Reference target character/object reuse, but both document limitations and require text prompts to disambiguate scene intent.
- Adobe Firefly separates Style Reference / Generative Match from Structure Reference. The structure path uses outline/depth-like similarity and a strength control, while the style path is for cohesive look and brand/aesthetic alignment.
- Runway Gen-4 References treats reference images as reusable assets for styles, characters, objects, traits, sketches, and iteration; the workflow encourages tagging and reusing references, not simply cloning a source image.
- FLUX Kontext/Comfy docs frame reference-image editing as text+image editing, character/object consistency, style reference, and targeted edits.

## Implication

The user correction is right: for creators, a link is often a reference anchor, not a literal copy request. The harness should ask "what role should this reference play?" only when the brief does not make the intent clear.

## Sources

- Midjourney Style Reference: https://docs.midjourney.com/hc/en-us/articles/32180011136653-Style-Reference
- Midjourney Character Reference: https://docs.midjourney.com/hc/en-us/articles/32162917505293-Character-Reference
- Midjourney Omni Reference: https://docs.midjourney.com/hc/en-us/articles/36285124473997-Omni-Reference
- Adobe Firefly Structure Reference API: https://developer.adobe.com/firefly-services/docs/firefly-api/guides/concepts/structure-image-reference/
- Adobe Firefly Structure Reference tutorial: https://www.adobe.com/learn/firefly/web/generative-ai-structure-reference-image
- Adobe Firefly Generative Match: https://www.adobe.com/products/firefly/features/generative-match.html
- Runway Gen-4 Image References: https://help.runwayml.com/hc/en-us/articles/40042718905875-Creating-with-Gen-4-Image-References
- ComfyUI Flux Kontext workflow: https://docs.comfy.org/tutorials/flux/flux-1-kontext-dev
- Black Forest Labs FLUX.1 Kontext editing docs: https://docs.bfl.ml/kontext/kontext_image_editing

## EXPAND

- DEAD END: "reference image always means exact reconstruction" - contradicted by official style, structure, character, object, sketch, and edit reference categories.
- LEAD: Rights and safety policy for external references - WHY: exact likeness/object/style reuse can create legal and safety risks - ANGLE: preserve `source_rights`, `likeness_sensitive`, and `commercial_safe_required` fields in the manifest.
- LEAD: Multi-reference conflict resolution - WHY: users may provide style, character, and structure refs that compete - ANGLE: require per-reference role labels and weights.
