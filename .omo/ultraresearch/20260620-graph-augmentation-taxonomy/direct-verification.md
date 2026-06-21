# Direct Verification: Civitai and Local Artifact Surface

Date: 2026-06-20

## Civitai model-version API sample

Command executed:

```bash
curl -L --fail --silent 'https://api.civitai.com/v1/model-versions/290640' | python3 -m json.tool
```

Relevant observed fields:

- `air`: `urn:air:sdxl:checkpoint:civitai:257749@290640`
- `baseModel`: `Pony`
- primary model file: `ponyDiffusionV6XL_v6StartWithThisOne.safetensors`
- primary model metadata: `format=SafeTensor`, `size=pruned`, `fp=fp16`
- hashes included `SHA256`, `BLAKE3`, `AutoV1`, `AutoV2`, `AutoV3`, and `CRC32`
- companion VAE file present with type `VAE`
- example image entries had mixed metadata coverage: some had `meta: null`, another had prompt, seed, steps, sampler, CFG, and size.

Verdict: Civitai model-version links are strong asset inputs for model binding, hash validation, base-model compatibility, and companion-file discovery. Civitai image metadata remains conditional and should not be treated as enough for exact reproduction unless workflow/settings/resources are present.

## Civitai AIR documentation redirect

Command executed:

```bash
curl -L --fail --silent 'https://raw.githubusercontent.com/wiki/civitai/civitai/AIR-%E2%80%90-Uniform-Resource-Names-for-AI.md'
```

Observed output says the AIR wiki page moved to `https://developer.civitai.com/site/guide/air` and that Civitai documentation now lives at `https://developer.civitai.com`.

Verdict: Keep AIR parsing in the manifest, but cite the live API response and official SDK examples when the developer site is unavailable through the current fetch path.

## Local artifact surface

Command executed:

```bash
find /Users/um-yunsang/image-harness-2026 -name AGENTS.md -print
rg --files /Users/um-yunsang/image-harness-2026/.omo | sort
```

Observed:

- The only discovered AGENTS file is under `/Users/um-yunsang/image-harness-2026/scaffold/AGENTS.md`, outside the `.omo` artifacts changed here.
- Existing artifacts are documentation/research/planning files under `.omo`.

Verdict: This task remains artifact editing, not product-code implementation.
