# Image Generation Harness Skill

Use this skill when a user wants to create, edit, refine, or export images through a controlled image-generation workflow that may use ComfyUI, LoRA, ControlNet, SAM masks, reference images, guidance policies, and post-processing.

## Core Rule

Do not treat image generation as a single prompt call. Treat it as a reproducible project:

1. Capture creator intent.
2. Index available generation capabilities.
3. Build a typed intent graph.
4. Compile a validated workflow.
5. Execute through an approved runtime.
6. Evaluate the visual result.
7. Save artifacts and lineage.

## Supported Runtimes

- Local ComfyUI Server API.
- Comfy Cloud API when available.
- Hosted image APIs for routes that are explicitly configured.
- Local training tools for LoRA and adapters when configured and allowed.

## Workflow

### 1. Project Intake

Create or open a project folder. Record:

- creator brief
- target output type
- references and their roles
- model/license constraints
- quality gates
- cost/speed/privacy preferences

### 2. Capability Index

Before planning execution, collect runtime capabilities:

- ComfyUI server URL and status
- node schemas from `object_info`
- installed model folders from `models`
- embeddings
- templates and subgraphs
- system stats and VRAM/device limits
- configured hosted routes

If a required capability is missing, report the missing node/model/tool and suggest the smallest install or route change.

### 3. Intent Graph

Convert the brief into an intent graph containing:

- subject identity
- style
- composition
- references
- masks and edit regions
- geometry controls
- LoRA/adapters
- output size and format
- quality gates

The LLM may draft the intent graph. The compiler must validate it.

### 4. Workflow Compile

Lower the intent graph into one of:

- known-good workflow template with typed parameter slots
- approved subgraph composition
- schema-guided graph patch against current ComfyUI `object_info`

Never execute a workflow that has not passed validation.

### 5. Validation Gate

Check:

- node classes exist
- node inputs exist and have compatible types
- model files exist
- LoRA/control adapters are compatible with the selected base model
- images and masks match required dimensions
- resource budget is acceptable
- license policy allows the selected route
- output path is inside the project

Stop and report exact blockers if validation fails.

### 6. Execute

Submit the workflow through the configured runtime. For ComfyUI:

- upload required images and masks
- POST API-format workflow JSON to `/prompt`
- monitor `/ws`
- retrieve `/history/{prompt_id}`
- fetch image outputs via `/view`
- save workflow JSON, progress trace, output files, and metadata

### 7. Evaluate

Run quality gates appropriate to the project:

- prompt adherence
- composition match
- identity consistency
- mask preservation
- text/OCR accuracy
- brand color delta
- safety and policy checks
- artifact/quality defects

If evaluation fails, produce a revision plan before re-running.

### 8. Export

Export:

- final images
- prompt cards
- workflow API JSON
- references and masks used
- model and adapter metadata
- seeds and parameters
- evaluation report
- license and provenance notes

## Safety Rules

- Do not auto-install arbitrary custom nodes.
- Do not train on private or copyrighted references without explicit authorization.
- Do not use noncommercial models for commercial projects.
- Do not run unvalidated workflow JSON.
- Do not hide failed quality gates.
- Do not delete project state to fix a failed run.

## Expected Scripts

The final package should provide:

- `harness init`
- `harness index-capabilities`
- `harness analyze-asset`
- `harness plan`
- `harness compile`
- `harness validate`
- `harness execute`
- `harness evaluate`
- `harness export`

## Good Output

A good run produces both the image and the evidence needed to reproduce it:

- final visual artifact
- exact workflow
- exact model/adapters
- exact control images and masks
- exact seed and sampler settings
- evaluation result
- revision history
