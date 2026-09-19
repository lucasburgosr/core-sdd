# CoreSDD

CoreSDD is a minimal specification-driven development framework for coding agents. It keeps decisions, authorized work, implementation, and verification connected without being coupled to an agent vendor or operating system.

> **Status: experimental.** The portable core is defined; agent adapters require their own validation before they are called supported.

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

| Agent | Installation guide |
| --- | --- |
| Codex | [Codex](integrations/codex/) |
| Claude Code | [Claude Code](integrations/claude-code/) |
| Pi | [Pi](integrations/pi/) |
| Antigravity | [Antigravity](integrations/antigravity/) |

An adapter is **supported** only after its discovery path and a representative `sdd-foundation` then `minimal-sdd` workflow have been verified. Until then, it is experimental. The current guides provide installation instructions; they do not yet constitute a verified support claim.

The full adapter material can be found in [`integrations/`](integrations/).

## Principles

- Update a source of truth only when the decision changes.
- For a functional change, follow `CONSTITUTION.md` → `SPEC.md` → `PLAN.md` → `TASKS.md` → code and tests.
- Repository instructions govern operation; they do not duplicate product or architecture decisions.
- Ask questions only when they affect scope, behavior, data, architecture, security, cost, or a difficult-to-reverse decision.
- Tasks authorize verifiable work; they do not track every micro-action or document-only administration.
- A bug fix that restores specified behavior, or an internal refactor, does not require artifact changes by default.
