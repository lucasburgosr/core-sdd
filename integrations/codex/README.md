# Codex adapter

This adapter contains optional Codex UI metadata for the two canonical CoreSDD skills. The portable instructions remain the `SKILL.md` files under [`skills/`](../../skills/).

## Install in one project

From the target project root, run:

```text
npx skills add lucasburgosr/core-sdd --skill sdd-foundation --skill minimal-sdd --agent codex --yes
```

Start a new Codex session in the project and confirm that both skills are available before using them. This is the only documented CoreSDD installation method.

This adapter is not yet marked supported: it still requires the verification recorded in T-02.
