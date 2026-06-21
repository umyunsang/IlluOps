# Proposed Package Shape

```text
illuops/
  AGENTS.md
  README.md
  package.json
  skills/
    illuops/
      SKILL.md
      references/
        state-model.md
        artifact-model.md
        security-policy.md
        validation-rubric.md
      workflows/
        recover.md
        eval.md
        security-review.md
      scripts/
        README.md
        illuops
      templates/
      assets/
  docs/
    spec.md
    install-codex.md
    install-claude-code.md
    mcp.md
    a2a.md
  examples/
    minimal/
    invalid-input/
    interrupted-run/
  evals/
    README.md
    fixtures/
    hidden/
  security/
    threat-model.md
    prompt-injection-fixtures/
    tool-poisoning-fixtures/
  releases/
    CHANGELOG.md
```

## Install Targets

```bash
npx skills add <owner>/<repo> --agent codex --global --yes --copy
npx skills add <owner>/<repo> --agent claude-code --global --yes --copy
```

## Design Rule

The skill, CLI, MCP server, and evaluation harness must call the same underlying deterministic operations. Avoid separate behavior paths for each host.
