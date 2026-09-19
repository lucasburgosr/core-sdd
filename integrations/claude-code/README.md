# Claude Code adapter

The canonical CoreSDD skills can be installed as Claude Code project skills. They remain the source of method instructions; this guide adds no Claude-specific rules to them.

## Install in one project

From the target project root, run:

```text
npx skills add lucasburgosr/core-sdd --skill sdd-foundation --skill minimal-sdd --agent claude-code --yes
```

Start a new Claude Code session in the project and confirm that both skills are available before using them. This is the only documented CoreSDD installation method.

This adapter is not yet verified. Its installation guide does not constitute a support claim.
