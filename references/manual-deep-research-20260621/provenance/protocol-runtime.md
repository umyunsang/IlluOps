# Provenance: Protocol, Runtime State, and Agent-Skill References

Access date: 2026-06-21

## Sources

Agent Skills:

- https://developers.openai.com/codex/skills
- https://developers.openai.com/api/docs/guides/tools-skills
- https://developers.openai.com/cookbook/examples/skills_in_api
- https://github.com/openai/skills

AG-UI:

- https://docs.ag-ui.com/concepts/events
- https://docs.ag-ui.com/concepts/interrupts
- https://docs.ag-ui.com/sdk/js/core/events
- https://docs.ag-ui.com/sdk/js/core/types
- https://github.com/ag-ui-protocol/ag-ui

A2A:

- https://a2a-protocol.org/latest/specification/
- https://a2a-protocol.org/latest/definitions/
- https://a2a-protocol.org/latest/announcing-1.0/
- https://a2a-protocol.org/latest/topics/agent-discovery/
- https://github.com/a2aproject/A2A

MCP:

- https://modelcontextprotocol.io/specification/2025-11-25
- https://modelcontextprotocol.io/specification/2025-11-25/basic/authorization
- https://modelcontextprotocol.io/specification/2025-11-25/basic/authorization/security-considerations
- https://modelcontextprotocol.io/docs/tutorials/security/security_best_practices
- https://modelcontextprotocol.io/specification/draft/basic/authorization

Runtime state and telemetry:

- https://sqlite.org/wal.html
- https://github.com/open-telemetry/semantic-conventions-genai
- https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/gen-ai/README.md

## Evidence Summary

- Agent Skills sources support `F-1` and `R2.10`: skill bundles are directory-based packages with `SKILL.md`, optional scripts, and references, and can be reused through hosted/API surfaces.
- AG-UI sources support `F-2` and `Q29`: event and interrupt semantics are primary protocol surfaces, while draft generative-UI/meta-event pages should remain advisory if used later.
- A2A sources support `F-3`, `R2.5`, and `R2.6`: the public `latest` specification and definitions pages are the current protocol/reference surfaces, and the release announcement gives version context.
- MCP sources support `F-9`, `R2.5`, and `R2.12`: the released `2025-11-25` specification is the primary pin; draft authorization material is recorded as advisory only.
- SQLite WAL supports `F-11`, `R2.4`, and `R2.6`: the durable-state claim should cite SQLite's WAL documentation rather than file-first local snapshots.
- OpenTelemetry GenAI sources support `F-10` and `R2.10` as development-status telemetry vocabulary references, not as a finalized normative standard.

## Claim Mapping

- `F-1`: Agent Skills packaging and reuse.
- `F-2`: AG-UI event vocabulary and interrupt semantics.
- `F-3`: A2A agent-card/discovery/interoperability references.
- `F-9`: MCP released spec plus security guidance.
- `F-10`: OpenTelemetry GenAI telemetry source family with development-status caveat.
- `F-11`: SQLite WAL durable-state substrate.
- `R2.5`, `R2.6`, `R2.10`, `R2.12`, and `Q29`.

## Manual Storage Decision

- Use `primary_url_confirmed` for current released protocol docs and official repositories.
- Use `draft_or_advisory` for MCP draft authorization material.
- Use `development_status_primary` for OpenTelemetry GenAI material because the repository itself is the current source, but the semantics are still moving.
- Store metadata/source cards only for these dynamic documentation pages; do not claim full local snapshots unless a reviewer later approves a specific capture method.
