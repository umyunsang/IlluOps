# Wave 1: Primary Sources for Graph Augmentation Taxonomy

Date: 2026-06-20

## Core platform constraints

- Comfy Cloud API accepts ComfyUI workflows in API format, where node ids map to `class_type` and `inputs`; jobs are submitted asynchronously through the API and monitored by polling or WebSocket.
  Source: https://docs.comfy.org/development/cloud/overview
- Comfy Cloud exposes `/api/object_info`, which returns available node definitions and input/output specifications. This is the capability catalog the harness should snapshot before graph patching.
  Source: https://docs.comfy.org/development/cloud/api-reference
- ComfyUI workflow API format differs from browser save format: API workflows use numeric node ids, include widget values, and exclude layout/color/group information. The harness must preserve both original UI workflow and executable API workflow when available.
  Source: https://docs.comfy.org/development/api-development/workflow-api-format
- ComfyUI nodes are typed function operators connected by links. Missing nodes after workflow import can mean either stale Comfy Core support or absent third-party custom nodes.
  Source: https://docs.comfy.org/development/core-concepts/nodes
- Custom nodes expand ComfyUI's core capabilities but must be reviewed for trust and security before installation or use.
  Source: https://docs.comfy.org/installation/install_custom_node

## Model and asset augmentation

- ComfyUI model docs describe checkpoints, VAEs, LoRAs, ControlNets, and upscalers as weight files selected through matching loader nodes. The graph patch engine should therefore treat model binding as an asset-loader operation, not only a prompt operation.
  Source: https://docs.comfy.org/development/core-concepts/models
- Comfy Cloud model import supports Hugging Face and Civitai links for cloud users at Creator tier or higher, and supports safetensor-style safe model formats.
  Source: https://docs.comfy.org/cloud/import-models
- Comfy workflow templates can embed model metadata under `properties.models` with `name`, `url`, and `directory`; currently supported sources include Hugging Face and Civitai, with safe formats such as `.safetensors` and `.sft`.
  Source: https://docs.comfy.org/interface/features/template
- Civitai's official JavaScript/Python generator SDK examples use AIR URNs for base models, `additionalNetworks` for LoRA/VAE/Hypernetwork/Textual Inversion/LyCORIS/Checkpoint/LoCon, and `controlNets` for control networks.
  Sources: https://github.com/civitai/civitai-javascript and https://github.com/civitai/civitai-python

## Conditioning, editing, and refinement primitives

- ComfyUI LoRA docs show LoRA as a loader-node chain that modifies both model and CLIP streams, supports strength controls, and can be chained for multiple LoRAs.
  Source: https://docs.comfy.org/tutorials/basic/lora
- ControlNet docs describe spatial/multimodal input conditions such as edge maps, depth maps, and pose keypoints; ComfyUI exposes load/apply nodes with `strength`, `start_percent`, and `end_percent` controls.
  Source: https://docs.comfy.org/tutorials/controlnet/controlnet
- ComfyUI image-to-image docs describe input image conditioning and the `denoise` parameter as the main control over how much the output diverges from the reference image.
  Source: https://docs.comfy.org/tutorials/basic/image-to-image
- ComfyUI inpainting docs describe region editing through input images, masks, and `VAE Encoder (for Inpainting)`; this supports localized edit routes rather than full-image regeneration.
  Source: https://docs.comfy.org/tutorials/basic/inpaint
- ComfyUI upscaling docs describe post-generation enhancement with `Load Upscale Model` and `Upscale Image (Using Model)`, plus chained/hybrid workflows.
  Source: https://docs.comfy.org/tutorials/basic/upscale
- ComfyUI subgraphs package complex node groups into reusable graph units, which makes reusable patch templates a first-class design option.
  Source: https://docs.comfy.org/interface/features/subgraph

## Research grounding for the example technologies

- ControlNet is a spatial conditioning architecture for pretrained text-to-image diffusion models, tested with edge, depth, segmentation, pose, and similar conditional controls.
  Source: https://arxiv.org/abs/2302.05543
- LoRA is low-rank adaptation: it freezes pretrained weights and injects trainable low-rank matrices, making it a lightweight adaptation family rather than a single image-generation trick.
  Source: https://arxiv.org/abs/2106.09685
- Segment Anything introduces promptable segmentation that returns masks from prompts such as points or boxes; in a Comfy workflow this is best classified as mask/segmentation routing for localized generation/editing, not as a generator by itself.
  Source: https://arxiv.org/abs/2304.02643
- IP-Adapter introduces an image-prompt adapter for text-to-image diffusion models and uses decoupled cross-attention to keep image prompting compatible with text prompting.
  Sources: https://arxiv.org/abs/2308.06721 and https://github.com/tencent-ailab/IP-Adapter

## Comfy Cloud supported custom-node evidence

- Comfy Cloud's supported-node catalog lists ComfyUI Impact Pack as a supported pack with detector/detailer nodes and iterative upscaler support.
  Source: https://comfy.org/cloud/supported-nodes/
- The same catalog lists ComfyUI_IPAdapter_plus as a supported pack with IPAdapter, FaceID, style/composition, embeds, loader, tiled, and weight nodes.
  Source: https://comfy.org/cloud/supported-nodes/
- The Impact Pack repository describes the pack as Detector, Detailer, Upscaler, Pipe, and related workflow helpers, and notes version compatibility and subpack requirements.
  Source: https://github.com/ltdrdata/ComfyUI-Impact-Pack

## Research caution for multi-reference creator intent

- UniCustom frames multi-reference generation as subject-identity preservation under multiple references, and identifies cross-reference confusion and attribute leakage as failure modes.
  Source: https://arxiv.org/abs/2605.12088
- ProductConsistency argues that product-centric editing requires preserving fine-grained product features, branding, and text, not only generic instruction following.
  Source: https://arxiv.org/abs/2606.19103
- ServImage evaluates image generation/editing against real commercial design tasks and separates baseline requirement fulfilment, visual execution quality, and commercial necessity satisfaction.
  Source: https://arxiv.org/abs/2604.24023

## EXPAND

- DEAD END: Treating ControlNet, LoRA, SAM, and IPAdapter as the whole feature set. They are examples of broader patch families.
- DEAD END: Letting the LLM create arbitrary node JSON. The reliable boundary is imported workflow + capability snapshot + typed patch manifest + validation.
- LEAD: Future implementation should define per-family schema validators for asset, conditioning, edit, refinement, external-provider, and custom-node patches.
