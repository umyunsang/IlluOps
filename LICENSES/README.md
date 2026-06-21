# License And Asset Boundaries

IlluOps is a planned cross-agent execution harness. The tracked repository content is licensed under Apache-2.0 unless another license is stated in the relevant file.

## Repository-Owned Material

Apache-2.0 applies to IlluOps-owned:

- plans and documentation
- future core Skill package files
- future CLI and schema code
- future test fixtures created for this repository
- future domain-pack interface contracts

## Domain Packs

Domain packs may have additional license obligations because they can include templates, provider integrations, model references, examples, or generated artifacts.

Each future pack must record:

- pack license
- publisher or maintainer identity
- source URL or provenance
- immutable digest or release identifier
- dependency license inventory
- SBOM or AIBOM where applicable
- redistribution restrictions
- commercial-use restrictions

The core harness must not silently relicense domain-pack content.

## ComfyUI And Custom Nodes

IlluOps is not a ComfyUI fork and is not affiliated with, endorsed by, or maintained by Comfy Org.

Future code that imports, links against, or derives from ComfyUI internals or GPL-licensed custom nodes must live in an explicitly marked subtree with the correct compatible license. Such code must not be mixed into the Apache-2.0 core without a reviewed license decision.

## Third-Party Creative Assets

Third-party models, LoRAs, embeddings, workflows, images, masks, fonts, datasets, slide templates, document templates, brand assets, and source materials are not relicensed by this repository.

Before execution, export, memory reuse, or redistribution, manifests must record:

- original license or rights statement
- source URL or owner-provided provenance
- permitted usage scope
- commercial-use allowance
- attribution requirements
- reuse or memory restrictions
- deletion or retention requirements

## Provider Outputs

Provider outputs are governed by the provider terms, selected model or asset license, user rights in the inputs, and the active workspace policy. A final package must include enough provenance, model, adapter, seed, parameter, cost, and rights metadata to reproduce and audit the result.

## Security And Supply Chain

External packs, worker bundles, MCP servers, A2A peers, templates, workflows, and model references are untrusted until the security inventory, broker policy, and supply-chain checks pass.

Future implementation must support signed provenance or equivalent attestation, vulnerability scanning, dependency-confusion checks, install-script policy, revocation or quarantine state, and update-diff review before enabling external packs or workers.
