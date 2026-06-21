# Direct Verification Notes

Date: 2026-06-20

## User Attachments Read

- `/Users/um-yunsang/.codex/attachments/28b79f39-72d2-4ef1-a5ae-c53ac5e39727/pasted-text.txt`
  - Official Comfy Cloud import-models page.
  - Confirms Civitai/Hugging Face model import into Comfy Cloud.
  - Confirms Civitai import link is the download-button link copied from Civitai.
  - Confirms Creator/Pro requirement, Cloud-only availability, safetensor-only restriction, and private import through saved Civitai/HF API keys.
- `/Users/um-yunsang/.codex/attachments/7503e696-a0b7-42b9-b8fb-3c89b98d24b3/pasted-text.txt`
  - Official Comfy Cloud MCP page.
  - Confirms closed-beta Cloud MCP, Claude Code/Desktop scope, `https://cloud.comfy.org/mcp`, slash commands, OAuth and headless API-key paths.

## Live Civitai Site API Checks

### Model/version metadata is strong

Command shape used:

```bash
curl -fsSL 'https://civitai.com/api/v1/model-versions/2514310' | jq '{id,modelId,name,baseModel,air,downloadUrl,trainedWords,files:[.files[]? | {id,name,type,primary,sizeKB,format:.metadata.format,downloadUrl,hashes:.hashes,pickleScanResult,virusScanResult,scannedAt}]}'
```

Observed:

- `air`: `urn:air:sdxl:checkpoint:civitai:827184@2514310`
- `downloadUrl`: `https://civitai.com/api/download/models/2514310`
- primary file: `waiIllustriousSDXL_v160.safetensors`
- format: `SafeTensor`
- scan results: `pickleScanResult=Success`, `virusScanResult=Success`
- hashes include SHA256, BLAKE3, AutoV2, AutoV3.

Conclusion: Civitai model/version links can be normalized into reliable asset manifests.

### Image metadata is inconsistent

Command shape used:

```bash
curl -fsSL 'https://civitai.com/api/v1/images?limit=20&sort=Newest&nsfw=false&withMeta=true' | jq '[.items[] | {id, postId, username, baseModel, hasMeta:(.meta != null), hasPrompt:(.meta.prompt? != null), resourceCount: ((.meta.civitaiResources? // .meta.resources? // []) | length), modelVersionIds:(.modelVersionIds // [])}] | .[0:10]'
```

Observed:

- Some images had prompt/resource metadata.
- Some images had no metadata.
- Some had `modelVersionIds` but no prompt metadata.
- Some metadata was nested under `meta.meta`.
- Some resources were only `{hash,name,type,weight}` without direct `modelVersionId`.

Example detailed image check:

```bash
curl -fsSL 'https://civitai.com/api/v1/images?imageId=134280890&withMeta=true' | jq '.items[0].meta'
```

Observed:

- prompt, seed, steps, width/height, sampler, cfgScale, resources.
- resources included model/LoRA names and hashes, not direct Civitai model-version IDs.

Conclusion: image links can become executable only when metadata is present and resolvable. Otherwise they are reference images/moodboard inputs, not exact workflows.

## Civitai MCP

Fetched:

```bash
curl -fsSL https://mcp.civitai.com/llms.txt
```

Observed:

- MCP endpoint: `https://mcp.civitai.com/mcp`
- Transport: Streamable HTTP.
- Auth: `Authorization: Bearer <CIVITAI_API_KEY>` for user actions; read/browse tools unauthenticated.
- Browse tools include `search_models`, `get_model`, `get_model_version`, `search_images`, `get_image`, `search_creators`, `list_enums`.
- `get_image` explicitly queries `/images?imageId=<id>&withMeta=true`.
- Zero-dependency CLI fallback available via `https://mcp.civitai.com/cli`.

Conclusion: Civitai MCP is a primary source for discovery and AIR resolution.

## Civitai Agent Skill

Fetched:

```bash
curl -fsSL https://raw.githubusercontent.com/civitai/civitai-gen-skill/main/civitai-gen/SKILL.md
```

Observed:

- Skill name: `civitai-gen`
- Version: `1.0.5`
- Requires Node 18+ and `CIVITAI_API_KEY`.
- Supports images, video, TTS, music, transcription, bulk, cost estimates.
- Uses Civitai MCP for model discovery and AIR URNs.
- README install path: `npx skills add civitai/civitai-gen-skill`.

Conclusion: a Civitai generation Agent Skill already exists. Our planned skill should compose Comfy workspaces, not duplicate this.

## Civitai Comfy Nodes

Fetched:

```bash
curl -fsSL https://raw.githubusercontent.com/civitai/civitai-comfy-nodes/main/README.md
curl -fsSL https://raw.githubusercontent.com/civitai/civitai-comfy-nodes/main/spec/v2-consumers.json | jq -r '.openapi, .info.title, .info.version, (.paths | length), (.paths | keys[] | select(test("recipe|recipes|workflow|workflows|model|image"; "i")) )'
curl -fsSL https://raw.githubusercontent.com/civitai/civitai-comfy-nodes/main/pyproject.toml
```

Observed:

- package: `civitai-comfy-nodes`
- version: `0.2.0`
- early preview, installable from Comfy Registry.
- dependency: `requests`
- about 160 generated nodes.
- exposes Civitai Orchestration recipes as Comfy nodes.
- model/LoRA/ControlNet selectors use AIR and can either run Civitai cloud recipes or download into local Comfy loader paths.
- OpenAPI: `3.1.1`
- title: `Civitai Orchestration Consumer API`
- version: `v2`
- paths: 57
- notable recipes include `/v2/consumer/recipes/comfy`, `/v2/consumer/recipes/customComfy`, `/v2/consumer/recipes/imageGen`, `/v2/consumer/recipes/textToImage`, `/v2/consumer/recipes/videoGen`, training, moderation, upscaling, prompt enhancement.

Conclusion: this is a first-class bridge for Civitai resources inside ComfyUI and should be integrated.

## Comfy-Org Agent Work

Fetched:

```bash
curl -fsSL 'https://api.github.com/repos/Comfy-Org/comfy-cloud-mcp'
curl -fsSL 'https://api.github.com/repos/Comfy-Org/comfy-cloud-mcp/issues/9'
curl -fsSL 'https://api.github.com/repos/Comfy-Org/ComfyUI_frontend/pulls/11547'
```

Observed:

- `Comfy-Org/comfy-cloud-mcp` exists, public, created 2026-03-25.
- Issue #9 requests Codex installer support for the official Comfy Cloud MCP; still open at verification time.
- Frontend PR #11547 is an open draft "in-browser agent for playing with ComfyUI"; it describes natural-language workflow building, node wiring, graph/queue/state introspection, templates, visual validation, and shell-style command dispatch. It is experimental and incomplete.

Conclusion: official agentic Comfy work is real but not yet a stable public Codex/local-all-custom-node harness.

## Community Local MCP Work

Fetched:

```bash
curl -fsSL https://raw.githubusercontent.com/artokun/comfyui-mcp/main/README.md
```

Observed:

- `artokun/comfyui-mcp` targets local/remote/Cloud ComfyUI.
- Provides MCP tools for generation, workflow execution, live graph editing, model and custom-node management, validation, history/logs, gallery, Civitai pairing, skills, agents, hooks.
- README reported 89+ tools and 22 skills at verification time.

Conclusion: there are serious community attempts. Any new harness must either interoperate with them or clearly differentiate as a creator workspace composer.

## ComfyUI-Copilot

Fetched:

```bash
curl -fsSL https://raw.githubusercontent.com/AIDC-AI/ComfyUI-Copilot/main/README.md
```

Observed:

- popular ComfyUI assistant custom node;
- workflow generation/debug/rewrite/model/node recommendations;
- service update says some hosted API features were suspended, but agent capabilities remain available with user's own API key/base URL.

Conclusion: LLM-assisted ComfyUI workflow authoring exists and is useful, but current ecosystem still lacks a robust universal resource-to-workspace harness.
