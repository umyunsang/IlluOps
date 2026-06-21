# Expansion Log: Reference Intent Routing

Date: 2026-06-20

Core question: In a Comfy Cloud-only Cross-Agent LLM Execution Harness, how should a Civitai/reference link be interpreted: exact reproduction, style/mood reference, structural/control reference, source transformation, or asset import?

Axes:

1. Creator reference workflows before and after AI: references as mood, style, composition, pose, subject continuity, and transformation material.
2. 2025-2026 reference-based generation/editing research: multi-reference, identity/style consistency, ControlNet-style structural conditioning, IPAdapter/reference encoders, localized editing, image-to-image transformation.
3. Civitai/Comfy Cloud capability and metadata constraints: model import reliability, image metadata inconsistency, Cloud supported workflows/nodes, Civitai AIR/resources.
4. Harness intent-routing design: how to infer link intent, ask minimal clarification, route to exact/transform/reference/asset modes, and define loop/evaluation semantics.

No subagents launched because the current tool instructions require explicit subagent/delegation request; direct web and local verification will be used.

## Wave 1

Files:

- `wave-1-product-docs.md`
- `wave-1-papers.md`
- `direct-verification.md`
- `SYNTHESIS.md`

Leads investigated:

- Creator reference semantics across Midjourney, Adobe Firefly, Runway, FLUX Kontext, and ComfyUI.
- 2026 multi-reference generation/editing papers and benchmarks.
- Civitai image metadata reliability via live public API probes.
- Civitai model/version reliability via live public API probe.
- Comfy Cloud import/API/supported-node/template surfaces.

Closed leads:

- "Every Civitai image link should default to exact reproduction" - rejected.
- "Civitai image metadata is reliable enough for exact-by-default" - rejected.
- "Reference-board mode is a fallback failure" - rejected; it is a correct product route when metadata is missing or the user asks for mood/style.

Convergence:

The routing decision is stable: links must resolve to typed `source_kind` plus typed `intent_role`; execution should ask one compact clarification when the link role is ambiguous.
