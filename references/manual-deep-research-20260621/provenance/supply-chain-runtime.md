# Provenance: Supply Chain, Runtime, Sandbox, and Policy Sources

Access date: 2026-06-21

## Sources

Sigstore/cosign/SLSA/in-toto:

- https://www.sigstore.dev/
- https://docs.sigstore.dev/quickstart/quickstart-cosign/
- https://docs.sigstore.dev/cosign/verifying/attestation/
- https://docs.sigstore.dev/about/bundle/
- https://slsa.dev/spec/v1.2/
- https://slsa.dev/spec/v1.2/build-provenance
- https://slsa.dev/spec/v1.2/source-requirements
- https://slsa.dev/spec/v1.2/distributing-provenance
- https://github.com/in-toto/attestation/blob/main/spec/README.md
- https://github.com/in-toto/attestation/blob/main/spec/v1/statement.md
- https://github.com/in-toto/attestation/blob/main/spec/v1/envelope.md

Syft/Grype/Scorecard:

- https://oss.anchore.com/docs/guides/sbom/getting-started/
- https://oss.anchore.com/docs/guides/sbom/
- https://oss.anchore.com/docs/guides/sbom/scan-targets/
- https://oss.anchore.com/docs/guides/sbom/formats/
- https://oss.anchore.com/docs/reference/syft/cli/
- https://github.com/anchore/syft/blob/main/README.md
- https://oss.anchore.com/docs/guides/vulnerability/getting-started/
- https://oss.anchore.com/docs/guides/vulnerability/
- https://oss.anchore.com/docs/guides/vulnerability/scan-targets/
- https://oss.anchore.com/docs/reference/grype/data-sources/
- https://oss.anchore.com/docs/architecture/grype-db/
- https://oss.anchore.com/docs/reference/grype/cli/
- https://github.com/anchore/grype/blob/main/README.md
- https://scorecard.dev/
- https://openssf.org/projects/scorecard/
- https://github.com/ossf/scorecard/blob/main/docs/checks.md
- https://github.com/ossf/scorecard/blob/main/docs/probes.md
- https://scorecard.dev/viewer/?uri=github.com%2Fossf%2Fscorecard

Wasmtime/WASI:

- https://docs.wasmtime.dev/security.html
- https://docs.wasmtime.dev/security-what-is-considered-a-security-vulnerability.html
- https://github.com/bytecodealliance/wasmtime/security/advisories
- https://github.com/bytecodealliance/wasmtime/blob/main/docs/WASI-capabilities.md
- https://wasi.dev/
- https://github.com/bytecodealliance/wasmtime/blob/main/docs/WASI-tutorial.md
- https://component-model.bytecodealliance.org/running-components/wasmtime.html
- https://bytecodealliance.org/articles/WASI-0.3

Optional mature OSS candidates:

- https://github.com/open-policy-agent/opa
- https://github.com/go-jose/go-jose
- https://github.com/auth0/node-jsonwebtoken
- https://github.com/RustCrypto/traits
- https://github.com/RustCrypto/signatures
- https://github.com/bucket4j/bucket4j
- https://github.com/sony/gobreaker
- https://github.com/resilience4j/resilience4j

## Evidence Summary

- Sigstore/cosign supports signing, verification, and in-toto attestation operations.
- SLSA v1.2 is the provenance requirement/spec surface.
- in-toto attestation defines the statement/envelope/predicate structure.
- Syft supports SBOM generation; Grype supports vulnerability scanning; Scorecard supports security posture/check scoring.
- Syft detail pages are needed for scan targets, output formats, offline behavior, and attestation CLI support.
- Grype detail pages are needed for data sources, Grype DB/update cadence, SBOM scan targets, and offline behavior.
- Scorecard checks/probes are needed for exact SBOM and Vulnerabilities check scoring behavior.
- Wasmtime/WASI supports a capability-based sandbox model with no ambient authority unless the host grants it.
- Optional auth/crypto/spend-control candidates should remain candidates, not locked dependencies.

## Claim Mapping

- Supports `R2.10`: mature OSS before bespoke provenance/SBOM/vuln/sandbox/auth/spend-ledger primitives.
- Supports `R2.11`: sandbox backend matrix and fail-closed worker policy.
- Supports `R2.12`: selected OSS provenance/SBOM/vuln/sandbox/auth/spend-control source gate.

## Manual Storage Decision

- Required save set: Sigstore/cosign attestation, SLSA v1.2/build provenance, in-toto attestation, Syft, Grype, Scorecard, Wasmtime security, WASI.
- Optional candidates are stored as evaluation context only.
