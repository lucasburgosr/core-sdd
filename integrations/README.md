# Integrations

CoreSDD itself lives in `../skills/`. This directory holds optional adapters for individual agents.

An adapter may supply discovery metadata, packaging instructions, or agent-specific validation. It must not fork or redefine the canonical method. See the adapter contract in [`PLAN.md`](../PLAN.md).

## Project installation via CLI

Each guide installs the two canonical skills with `npx skills add`. The CLI selects the target agent's project directory; manual copying is intentionally not documented.

| Agent | Status | Guide |
| --- | --- | --- |
| Codex | Supported | [Codex](codex/) |
| Claude Code | Supported | [Claude Code](claude-code/) |
| Pi | Supported | [Pi](pi/) |
| Antigravity | Supported | [Antigravity](antigravity/) |
