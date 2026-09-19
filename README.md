# CoreSDD

CoreSDD is a minimal specification-driven development framework for coding agents. It keeps decisions, authorized work, implementation, and verification connected without being coupled to an agent vendor or operating system.

> **Status: experimental.** The portable core is defined. Pi and Antigravity are supported; other adapters remain unverified.

## Core method

| Capability | Use it when | Outcome |
| --- | --- | --- |
| `sdd-foundation` | Starting a project, clarifying an ambiguous idea, or recovering insufficient artifacts | Establishes a minimal, agreed foundation using `CONSTITUTION.md`, `SPEC.md`, `PLAN.md`, `TASKS.md`, and a repository instruction file when needed. |
| `minimal-sdd` | Evolving a project with an established foundation | Updates the necessary artifacts before implementation and keeps them aligned with the code. |

The canonical instructions are under [`skills/`](skills/). They define the method; they do not require a particular command syntax, filesystem location, shell, or operating system.

```text
idea or ambiguous repository
        │
        ▼
sdd-foundation
        │  accepted artifacts
        ▼
minimal-sdd
        │  implemented and verified changes
        ▼
continuous evolution
```

## Compatibility and installation

CoreSDD itself lives in [`skills/`](skills/). Integrations are thin adapters: they make the canonical skills discoverable and can add agent-specific metadata or packaging, but they do not redefine the method. See the adapter contract in [`PLAN.md`](PLAN.md).

Install the two canonical skills through the project-scoped CLI guide for your agent. The CLI selects the target agent's project directory; manual copying is intentionally not documented.

| Agent | Status | Installation guide |
| --- | --- | --- |
| Codex | Unverified | [Codex](integrations/codex/) |
| Claude Code | Unverified | [Claude Code](integrations/claude-code/) |
| Pi | Supported | [Pi](integrations/pi/) |
| Antigravity | Supported | [Antigravity](integrations/antigravity/) |

An adapter is **supported** only after its installation and discovery of both canonical skills have been verified in that agent. Unverified guides provide installation instructions but do not constitute a support claim.

The full adapter material can be found in [`integrations/`](integrations/).

## Principles

- Update a source of truth only when the decision changes.
- For a functional change, follow `CONSTITUTION.md` → `SPEC.md` → `PLAN.md` → `TASKS.md` → code and tests.
- Repository instructions govern operation; they do not duplicate product or architecture decisions.
- Ask questions only when they affect scope, behavior, data, architecture, security, cost, or a difficult-to-reverse decision.
- Tasks authorize verifiable work; they do not track every micro-action or document-only administration.
- A bug fix that restores specified behavior, or an internal refactor, does not require artifact changes by default.
