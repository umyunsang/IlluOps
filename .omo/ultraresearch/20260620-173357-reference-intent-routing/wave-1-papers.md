# Wave 1: 2026 Multi-Reference and Editing Research

Date: 2026-06-20

## Axis

What do 2025-2026 papers imply about reference-image intent, ambiguity, and controllability?

## Findings

Recent research supports typed reference routing because reference-based generation is not a solved single-mode problem.

- UniCustom frames multi-reference generation as text instructions plus multiple visual references where models must preserve identities and appearances while composing selected subjects. It identifies a grounding-binding gap: recognizing which subject is requested is easier than binding the right fine-grained appearance to that subject.
- UniRef-Image-Edit unifies single-image editing and multi-image composition. Its abstract emphasizes consistency problems across multiple conditions and uses training/RL to reconcile conflicting visual constraints.
- MACRO identifies multi-reference generation as important for multi-subject composition, narrative illustration, and novel-view synthesis, and says performance degrades as reference count grows without structured long-context data.
- ProductConsistency focuses on product identity preservation in instruction-based image editing, showing that even product editing needs dedicated datasets and benchmarks.
- ServImage evaluates generation/editing against real commercial imaging tasks, which is closer to creator production value than generic aesthetic scores.

## Implication

The harness should not collapse references into one prompt string. It should store them as typed slots:

```json
{
  "reference_id": "ref_01",
  "role": "style|identity|object|structure|source_edit|workflow|model_asset|moodboard",
  "strength": "low|medium|high|exact_when_possible",
  "conflicts_with": ["ref_02"],
  "requires_clarification": false
}
```

## Sources

- UniCustom, arXiv:2605.12088: https://arxiv.org/html/2605.12088v1
- UniRef-Image-Edit, arXiv:2602.14186: https://arxiv.org/abs/2602.14186
- MACRO, arXiv:2603.25319: https://arxiv.org/abs/2603.25319
- ProductConsistency, arXiv:2606.19103: https://arxiv.org/abs/2606.19103
- ServImage, arXiv:2604.24023: https://arxiv.org/html/2604.24023v1

## EXPAND

- LEAD: Product and brand references - WHY: creator workflows often need object/logo preservation and commercial QA - ANGLE: add product/brand identity gate separate from generic style.
- DEAD END: "open-source models can reliably bind arbitrary many references today" - current papers still present multi-reference binding as an active problem.
