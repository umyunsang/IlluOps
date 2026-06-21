# Direct Verification: Reference Intent Routing

Date: 2026-06-20

## Purpose

Verify the fragile claims behind Civitai/reference-link routing:

1. Civitai model/version resources are stronger automation inputs than Civitai image/post pages.
2. Civitai image metadata is not reliable enough to default to exact reproduction.
3. Comfy Cloud can execute reference-driven workflows, but the harness must route through explicit workflow recipes and supported node/model surfaces.

## Live Civitai API Probe

Command summary:

```text
GET https://civitai.com/api/v1/images?limit=50&nsfw=false&withMeta=true
```

Observed on 2026-06-20:

```json
{
  "sample_count": 48,
  "meta_present": 34,
  "meta_null": 14,
  "resources_present": 0,
  "modelVersionId_present": 0,
  "sample_ids_without_meta": [127883707, 91322548, 60455213, 128088054, 110821038, 47407380, 99836704, 98005683, 76352187, 99377869],
  "sample_ids_with_meta": [110915488, 127677013, 124369493, 96045281, 111352851, 124490854, 111566505, 52540955, 111442411, 127740363]
}
```

Interpretation: image pages can be useful references, but a bare image/post URL cannot be assumed to contain a reconstructable workflow. The router must distinguish `exact_candidate` from `reference_only`.

## Civitai Metadata Shape Probe

Command summary:

```text
GET https://civitai.com/api/v1/images?imageId=110915488&withMeta=true
GET https://civitai.com/api/v1/images?imageId=127883707&withMeta=true
```

Observed on 2026-06-20:

```json
{
  "id": 110915488,
  "meta": {
    "id": 110915488,
    "meta": {
      "Size": "1248x1824",
      "seed": 818544170345672,
      "Model": "Nickel Saffron Manga",
      "steps": 30,
      "hashes": {"model": "38FB5B8E02"},
      "prompt": "present",
      "negativePrompt": "present"
    }
  },
  "modelVersionIds": []
}
```

```json
{
  "id": 127883707,
  "meta": {"id": 127883707, "meta": null},
  "modelVersionIds": [164821, 2442439, 2460437, 2515203]
}
```

Interpretation: even when an image endpoint returns a `meta` object, the actual generation metadata may be nested or null. Some pages expose `modelVersionIds` without prompts/settings. The parser must normalize multiple shapes and preserve uncertainty.

## Civitai Model Version Probe

Command summary:

```text
GET https://civitai.com/api/v1/model-versions/290640
```

Observed on 2026-06-20:

```json
{
  "id": 290640,
  "modelId": 257749,
  "name": "V6 (start with this one)",
  "baseModel": "Pony",
  "air": "urn:air:sdxl:checkpoint:civitai:257749@290640",
  "files": [
    {
      "name": "ponyDiffusionV6XL_v6StartWithThisOne.safetensors",
      "type": "Model",
      "format": "SafeTensor",
      "downloadUrl_present": true,
      "hash_keys": ["AutoV1", "AutoV2", "AutoV3", "BLAKE3", "CRC32", "SHA256"]
    },
    {
      "name": "sdxl_vae.safetensors",
      "type": "VAE",
      "format": "SafeTensor",
      "downloadUrl_present": true,
      "hash_keys": ["AutoV1", "AutoV2", "AutoV3", "BLAKE3", "CRC32", "SHA256"]
    }
  ]
}
```

Interpretation: model/version/download/AIR links are strong bootstrap inputs for the Comfy Cloud-only harness. They can produce model asset manifests, import actions, compatibility checks, and recipe selection. They do not by themselves define the user's reference intent.

## Comfy Cloud and ComfyUI Surface Checks

Primary documentation checked:

- Comfy Cloud import models: https://docs.comfy.org/cloud/import-models
- Comfy Cloud API reference: https://docs.comfy.org/development/cloud/api-reference
- Comfy Cloud supported custom nodes: https://comfy.org/cloud/supported-nodes/
- ComfyUI image-to-image workflow: https://docs.comfy.org/tutorials/basic/image-to-image
- ComfyUI workflow templates: https://docs.comfy.org/interface/features/template
- ComfyUI workflow concepts: https://docs.comfy.org/development/core-concepts/workflow

Key findings:

- Comfy Cloud model import is a Cloud-only feature for Creator tier or higher, and uses Civitai/Hugging Face model links.
- Comfy Cloud API exposes `object_info`, input upload, workflow submission, job status, WebSocket progress, output download, cancel, and interrupt.
- The API is marked experimental; the harness should store capability snapshots and not assume stable endpoint semantics.
- Comfy Cloud supported-node inventory includes Impact Pack with SAM, segmentation, detector, detailer, ControlNet-SEGS, and IPAdapter-SEGS nodes; it also includes IPAdapter Plus and video/upscale/masking packs.
- ComfyUI image-to-image explicitly treats a reference image plus prompt as a conditional workflow, with denoise controlling deviation from the reference.
- Workflow templates can embed direct Hugging Face or Civitai model links under node `properties.models`.

## Verdict

Confirmed: the harness must not use a single "Civitai link means reproduce" rule.

Better rule:

```text
link -> typed evidence -> intent route -> reproducibility confidence -> one precise clarification only when needed
```

The default for a bare Civitai image/post link should be `reference_or_transform_candidate`, not `exact_reproduction`. Exact reproduction is allowed only when the user explicitly asks for it and the resolver proves sufficient metadata/workflow/resource coverage.
