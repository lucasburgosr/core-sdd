# CoreSDD

CoreSDD is a minimal specification-driven development framework for coding agents. It keeps decisions, authorized work, implementation, and verification connected without being coupled to an agent vendor or operating system.

> **Status: experimental.** The portable core is defined. Codex, Claude Code, Pi, and Antigravity are supported.

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

## Multi-agent workflow

CoreSDD supports a workflow where a more capable model plans and defines tasks, a lower-cost model implements them, and the planning agent reviews the result. The user assigns the models; the handoff works across sessions without an orchestrator.

1. The planner defines identifiable tasks with scope, acceptance criteria, and verification.
2. The implementer executes the authorized tasks and records verification evidence.
3. On a requested review, the reviewer inspects the implementation and writes `reviews/review-t12-t15.md` for a scope such as `T-12` through `T-15`, even if no findings are identified.
4. On a correction request, the implementer fixes the findings and records the corrections in that same file. The reviewer adds the next review round there, preserving history and finding IDs. Subsequent corrections and reviews keep using the same file.
5. Tasks become `DONE` after verification and the required review pass; blocking findings or fixes awaiting review keep them pending.

The canonical [review protocol](skills/minimal-sdd/SKILL.md#multi-agent-implementation-and-review) defines report contents and the correction loop. Review reports are verification evidence; the SDD artifacts remain the sources of decisions and authorized work.

### Example: implement and review T-12 through T-15

Use these requests in the corresponding agent sessions:

| Step | Agent | Request |
| --- | --- | --- |
| Plan | More capable model | Define the plan and tasks with scope, dependencies, acceptance criteria, and verification. |
| Implement | Lower-cost model | Implement T-12 through T-15 and record the verification results. |
| Review | Planning agent | Review T-12 through T-15 and document the findings in `reviews/review-t12-t15.md`. |
| Correct | Implementer | Apply the required corrections from `reviews/review-t12-t15.md` and document the changes and checks in that same file. |
| Re-review | Planning agent | Verify the corrections and append the next review round to `reviews/review-t12-t15.md`. |

Repeat correction and re-review as needed. **All rounds for this scope stay in the same file**: do not create numbered review files or replace earlier entries. Keep stable finding IDs, such as `R-01`, so each correction can be traced to its finding. The implementer marks fixes as awaiting review; the reviewer confirms their resolution.

Each review records the task scope, implementation reference, verdict, findings with severity and code references, and checks performed or omitted. Append dated correction and re-review entries, preserving history while updating the current verdict and finding statuses. A review with no findings still produces a report. Link that report from the relevant tasks in `TASKS.md`.

## Compatibility and installation

CoreSDD itself lives in [`skills/`](skills/). Integrations are thin adapters: they make the canonical skills discoverable and can add agent-specific metadata or packaging, but they do not redefine the method. See the adapter contract in [`PLAN.md`](PLAN.md).

Install the two canonical skills through the project-scoped CLI guide for your agent. The CLI selects the target agent's project directory; manual copying is intentionally not documented.

| Agent | Status | Installation guide |
| --- | --- | --- |
| Codex | Supported | [Codex](integrations/codex/) |
| Claude Code | Supported | [Claude Code](integrations/claude-code/) |
| Pi | Supported | [Pi](integrations/pi/) |
| Antigravity | Supported | [Antigravity](integrations/antigravity/) |

The full adapter material can be found in [`integrations/`](integrations/).

## Principles

- Update a source of truth only when the decision changes.
- For a functional change, follow `CONSTITUTION.md` → `SPEC.md` → `PLAN.md` → `TASKS.md` → code and tests.
- Repository instructions govern operation; they do not duplicate product or architecture decisions.
- Ask questions only when they affect scope, behavior, data, architecture, security, cost, or a difficult-to-reverse decision.
- Tasks authorize verifiable work; they do not track every micro-action or document-only administration.
- A bug fix that restores specified behavior, or an internal refactor, does not require artifact changes by default.
