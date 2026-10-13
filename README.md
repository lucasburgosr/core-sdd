# CoreSDD

CoreSDD is a minimal specification-driven development framework for coding agents. It keeps decisions, authorized work, implementation, and verification connected without being coupled to an agent vendor or operating system.

## Core method

| Capability | Use it when | Outcome |
| --- | --- | --- |
| `sdd-foundation` | Starting a project, clarifying an ambiguous idea, or recovering insufficient artifacts | Establishes an agreed global foundation, requirement-specific specifications and tasks, and repository operating instructions when needed. |
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

## Global foundation and requirement specifications

The default layout keeps the system overview brief and puts independently scoped requirements in their own folders:

```text
CONSTITUTION.md
SPEC.md
PLAN.md
TASKS.md
AGENTS.md                 # or the active agent's equivalent
specs/
  REQ-001-enrollment/
    SPEC.md
    PLAN.md
    TASKS.md
  REQ-002-notifications/
    SPEC.md
    PLAN.md
    TASKS.md
reviews/                  # created when implementation is reviewed
```

| Artifact | Responsibility |
| --- | --- |
| Root `CONSTITUTION.md` | Durable principles, document hierarchy, quality expectations, and change rules. |
| Root `SPEC.md` | System problem, actors, goals, scope, main modules, their relationships, global functional invariants, and system-wide requirements and constraints. |
| Root `PLAN.md` | Shared architecture, module responsibilities, sources of truth, shared contracts, data, integrations, technical constraints, risks, and system-wide test strategy. |
| Root `TASKS.md` | Requirement index: stable ID, brief description, folder link, and dependencies. No duplicate executable tasks or execution statuses. |
| Requirement `SPEC.md` | Scope, flows, business rules, observable behavior, and acceptance criteria. Links to applicable global decisions and requirement dependencies. |
| Requirement `PLAN.md` | Approved technical solution, affected components, contracts, data, integrations, risks, and verification. |
| Requirement `TASKS.md` | Authorized tasks, acceptance criteria, dependencies, and verification evidence, with qualified IDs such as `REQ-001/T-01`. |
| Repository instructions | Selective reading order, commands, conventions, verification, and operational boundaries. |

For example, the root index links `REQ-001` (enrollment) to its folder and `REQ-002` (notifications) to its folder, recording that notifications depend on enrollment. The notification specification links the relevant enrollment contract; its tasks and their evidence live only in `specs/REQ-002-notifications/TASKS.md`.

Create `specs/REQ-<number>-<name>/` for a requirement with its own scope and acceptance criteria, using the next unused number and a short descriptive name. A requirement may span several modules; modules with no agreed work do not need folders. When a requirement evolves, update its existing specifications and add tasks without reopening completed ones. Git preserves the history. Small adjustments belong in the existing requirement; fixes and internal refactors retain the exceptions below.

Local decisions must satisfy the constitution and global foundation. A change to a global invariant or shared contract is resolved and documented globally first, then propagated to affected requirements. Link shared decisions rather than copying them. The root index alone does not authorize implementation: the active requirement's tasks define the authorized scope.

Agents read repository instructions, the constitution, global specification and plan, and root index first, then the active requirement and its relevant dependencies. They do not load all requirement folders routinely.

### Gradual adoption

Existing projects may migrate gradually and retain equivalent documentation structures. Move each decision or task once, replace its old content with a link, and preserve pending work, task IDs, review links, and verification evidence. During partial migration, the root index also identifies the exact legacy sections that still own work without copying their tasks. Keep existing task IDs and qualify references with requirement IDs where needed. A small change does not require a full migration.

## Multi-agent workflow

CoreSDD supports a workflow where a more capable model plans and defines tasks, a lower-cost model implements them, and the planning agent reviews the result. The user assigns the models; the handoff works across sessions without an orchestrator.

1. The planner defines identifiable tasks with scope, acceptance criteria, and verification.
2. The implementer executes the authorized tasks and records verification evidence.
3. On a requested review, the reviewer inspects the implementation and writes `reviews/review-req-001-t12-t15.md` for a scope such as `REQ-001/T-12` through `REQ-001/T-15`, even if no findings are identified.
4. On a correction request, the implementer fixes the findings and records the corrections in that same file. The reviewer adds the next review round there, preserving history and finding IDs. Subsequent corrections and reviews keep using the same file.
5. Tasks become `DONE` after verification and the required review pass; blocking findings or fixes awaiting review keep them pending.

The canonical [review protocol](skills/minimal-sdd/SKILL.md#multi-agent-implementation-and-review) defines report contents and the correction loop. Review reports are verification evidence; the SDD artifacts remain the sources of decisions and authorized work.

### Example: implement and review REQ-001/T-12 through REQ-001/T-15

Use these requests in the corresponding agent sessions:

| Step | Agent | Request |
| --- | --- | --- |
| Plan | More capable model | Define the requirement plan and tasks with scope, dependencies, acceptance criteria, and verification. |
| Implement | Lower-cost model | Implement REQ-001/T-12 through REQ-001/T-15 and record the verification results in the requirement's tasks. |
| Review | Planning agent | Review REQ-001/T-12 through REQ-001/T-15 and document the findings in `reviews/review-req-001-t12-t15.md`. |
| Correct | Implementer | Apply the required corrections from `reviews/review-req-001-t12-t15.md` and document the changes and checks in that same file. |
| Re-review | Planning agent | Verify the corrections and append the next review round to `reviews/review-req-001-t12-t15.md`. |

Repeat correction and re-review as needed. **All rounds for this scope stay in the same file**: do not create numbered review files or replace earlier entries. Keep stable finding IDs, such as `R-01`, so each correction can be traced to its finding. The implementer marks fixes as awaiting review; the reviewer confirms their resolution.

Each review records the qualified task scope, implementation reference, verdict, findings with severity and code references, and checks performed or omitted. Append dated correction and re-review entries, preserving history while updating the current verdict and finding statuses. A review with no findings still produces a report. Link that report from the relevant requirement's `TASKS.md`.

For reviews across requirements, include every requirement ID in the filename, for example `reviews/review-req-001-t01-req-002-t01.md`, and list the qualified tasks in the report. Reuse existing legacy review files for their correction rounds.

## Compatibility and installation

CoreSDD itself lives in [`skills/`](skills/). Integrations are thin adapters: they make the canonical skills discoverable and can add agent-specific metadata or packaging, but they do not redefine the method. See the adapter contract in the [integrations overview](integrations/README.md).

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
- For a functional change, follow `CONSTITUTION.md` → global `SPEC.md` and `PLAN.md` → requirement `SPEC.md` → requirement `PLAN.md` → requirement `TASKS.md` → code and tests. At each level, technical decisions satisfy functional requirements.
- Repository instructions govern operation; they do not duplicate product or architecture decisions.
- Ask questions only when they affect scope, behavior, data, architecture, security, cost, or a difficult-to-reverse decision.
- Tasks authorize verifiable work; they do not track every micro-action or document-only administration.
- A bug fix that restores specified behavior, or an internal refactor, does not require artifact changes by default.
