# Antigravity adapter

Antigravity supports workspace skills as directory packages containing `SKILL.md`. CoreSDD uses that standard layout directly.

## Install in one project

From the target project root, run:

```text
npx skills add lucasburgosr/core-sdd --skill sdd-foundation --skill minimal-sdd --agent antigravity --yes
```

Start a new Antigravity conversation in the project and confirm that both skills are available before using them. This is the only documented CoreSDD installation method.

This adapter is supported: installation and discovery of both canonical skills have been verified in Antigravity.
