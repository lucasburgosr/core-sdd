# CoreSDD

A minimal specification-driven development framework for agents. It combines two complementary skills to move from an idea to implementation without losing the connection between decisions, governance artifacts, and code.

> **Status: experimental.** CoreSDD is being validated and its skills may change before the first usable release. The repository has no tags or releases yet.

## The two skills

| Skill | Use it when | Outcome |
| --- | --- | --- |
| `sdd-foundation` | Starting a project, clarifying an ambiguous idea, or recovering insufficient artifacts | Discovers decisions and creates or recovers a foundation using `CONSTITUTION.md`, `SPEC.md`, `PLAN.md`, `TASKS.md`, and `AGENTS.md`. |
| `minimal-sdd` | Evolving a project with an established foundation | Decides which artifacts to update before implementation and keeps them aligned with the code. |

The intended flow is:

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

## Principles

- Update a source of truth only when the decision it contains changes.
- For a functional change, follow `CONSTITUTION.md` → `SPEC.md` → `PLAN.md` → `TASKS.md` → code and tests.
- `AGENTS.md` contains repository operating instructions; it does not duplicate product or architecture decisions.
- Ask questions only when they affect scope, behavior, data, architecture, security, cost, or a difficult-to-reverse decision.
- Tasks authorize verifiable work; they do not track every micro-action or document-only administration.
- A bug fix that restores specified behavior, or an internal refactor, does not require artifact changes by default.

## Usage

To establish a project:

```text
$sdd-foundation I want to build an application for managing personal expenses.
```

To evolve it once its artifacts are defined:

```text
$minimal-sdd Add monthly budgets and update the necessary artifacts before implementation.
```

Clear task descriptions also allow implicit activation, but explicit invocation is more predictable.