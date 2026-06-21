# Wave 1: Multi-Control Pipeline Primitives

Worker: `019ee3f4-6289-70f1-b2a7-0cda939a9fe7`

## Key Findings

- Separate true control primitives from downstream refinement. Pipeline failures often come from mixing the wrong class of control at the wrong stage.
- LoRA/LyCORIS/DoRA/rsLoRA belong to a style/identity/subject adapter slot, not a spatial-control slot.
- ControlNet/T2I-Adapter/ControlNeXt/Uni-ControlNet/OminiControl belong to spatial and structural control.
- IP-Adapter belongs to reference-image conditioning.
- SAM2/Grounding DINO/Grounded-SAM belong to mask/object selection upstream of inpaint/outpaint/detailer passes.
- SAG/PAG are sampling-time guidance methods, not structural controllers.
- Inpaint/outpaint/upscale/detailer belong in later refinement passes.

## Recommended Slot Order

1. Base model slot.
2. Style / identity slot.
3. Reference-image slot.
4. Spatial control slot.
5. Mask slot.
6. Sampling guidance slot.
7. Refinement slot.

## Failure Modes To Certify Against

- Backbone mismatch.
- Condition competition.
- Spatial drift during refinement.
- Mask boundary artifacts.
- Reference-image leakage.
- Sampler overguidance.
- Composability illusion.

## Sources Reported By Worker

- LoRA: https://arxiv.org/abs/2106.09685
- LyCORIS: https://github.com/KohakuBlueleaf/LyCORIS
- DoRA: https://arxiv.org/abs/2402.09353
- rsLoRA: https://arxiv.org/abs/2312.03732
- ControlNet: https://github.com/lllyasviel/controlnet and https://arxiv.org/abs/2302.05543
- T2I-Adapter: https://github.com/tencentarc/T2I-Adapter and https://arxiv.org/abs/2302.08453
- ControlNeXt: https://github.com/JIA-Lab-research/ControlNeXt and https://arxiv.org/abs/2408.06070
- Uni-ControlNet: https://github.com/ShihaoZhaoZSH/Uni-ControlNet and https://openreview.net/forum?id=VgQw8zXrH8
- OminiControl: https://github.com/Yuanshi9815/OminiControl and https://arxiv.org/abs/2411.15098
- IP-Adapter: https://github.com/tencent-ailab/IP-Adapter and https://arxiv.org/abs/2308.06721
- Diffusers IP-Adapter docs: https://huggingface.co/docs/diffusers/en/using-diffusers/ip_adapter
- ComfyUI IPAdapter Plus: https://github.com/cubiq/comfyui_ipadapter_plus
- SAM2: https://github.com/facebookresearch/sam2 and https://arxiv.org/abs/2408.00714
- Grounding DINO: https://github.com/IDEA-Research/GroundingDINO
- Grounded-SAM: https://github.com/IDEA-Research/Grounded-Segment-Anything
- Grounded SAM 2: https://github.com/IDEA-Research/grounded-sam-2
- ComfyUI inpaint docs: https://docs.comfy.org/tutorials/basic/inpaint
- ComfyUI upscale docs: https://docs.comfy.org/tutorials/basic/upscale
- SAG: https://arxiv.org/abs/2210.00939
- PAG: https://arxiv.org/abs/2403.17377

## Worker EXPAND Markers

LEAD:

- LoRA / LyCORIS / DoRA / rsLoRA for global style, identity, and subject adaptation.
- ControlNet / T2I-Adapter / ControlNeXt / Uni-ControlNet / OminiControl for spatial and structural control.
- IP-Adapter for reference-image conditioning.
- SAM2 + Grounding DINO + Grounded-SAM for mask production.
- SAG / PAG for sampler-time quality guidance.
- Inpaint / outpaint / upscale / detailer as later refinement pass.

DEAD END:

- No official ComfyUI core "detailer" page was found; detailer is inferred from inpaint/upscale workflows and popular custom-node packs.
- No single official source prescribes universal ordering across all backbones; ordering is synthesized from papers/docs.
