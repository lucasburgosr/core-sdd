# Pi adapter

Pi discovers directory skills with `SKILL.md` from a project `.agents/skills/` directory. This layout can be shared with compatible agents without changing the CoreSDD source.

## Install in one project

From the target project root, run:

```text
npx skills add lucasburgosr/core-sdd --skill sdd-foundation --skill minimal-sdd --agent pi --yes
```

Start Pi in the project and confirm that both skills are available before using them. This is the only documented CoreSDD installation method.

This adapter is not yet marked supported: it still requires the Pi verification recorded in T-04.
