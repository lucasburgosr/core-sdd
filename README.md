# CoreSDD

CoreSDD is a minimal specification-driven development framework for coding agents. It keeps decisions, authorized work, implementation, and verification connected without being coupled to an agent vendor or operating system.

## Core method

| Capability | Use it when | Outcome |
| --- | --- | --- |
| `sdd-foundation` | Starting a project, clarifying an ambiguous idea, or recovering insufficient artifacts | Defines or reconstructs an agreed foundation for the affected scope, distinguishing confirmed decisions, inferred behavior, and open questions. |
| `minimal-sdd` | Evolving a project with a sufficiently defined affected scope | Coordinates authorized changes, implementation, acceptance-based reviews, and alignment with code. |

The canonical instructions are under [`skills/`](skills/). They define the method; they do not require a particular command syntax, filesystem location, shell, or operating system.

```text
idea or ambiguous repository
        │
        ▼
sdd-foundation
        │  accepted, verifiable affected scope
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
| Requirement `TASKS.md` | Compact index of authorized tasks, states, dependencies, and links to acceptance criteria in `SPEC.md`; add concise scope and verification details only as needed. Use qualified IDs such as `REQ-001/T-01`. |
| Repository instructions | Selective reading order, commands, conventions, verification, and operational boundaries. |

For example, the root index links `REQ-001` (enrollment) to its folder and `REQ-002` (notifications) to its folder, recording that notifications depend on enrollment. The notification specification links the relevant enrollment contract; its tasks and their evidence live only in `specs/REQ-002-notifications/TASKS.md`.

Create `specs/REQ-<number>-<name>/` for a requirement with its own scope and acceptance criteria, using the next unused number and a short descriptive name. A requirement may span several modules; modules with no agreed work do not need folders. When a requirement evolves, update its existing specifications and add tasks without reopening completed ones. Git preserves the history. Small adjustments belong in the existing requirement; fixes and internal refactors retain the exceptions below.

Local decisions must satisfy the constitution and global foundation. A change to a global invariant or shared contract is resolved and documented globally first, then propagated to affected requirements. Link shared decisions rather than copying them. The root index alone does not authorize implementation: the active requirement's tasks define the authorized scope.

Agents read repository instructions, the constitution, global specification and plan, and root index first, then the active requirement and its relevant dependencies. They do not load all requirement folders routinely. For existing codebases, `sdd-foundation` distinguishes `CONFIRMED` decisions from `INFERRED` behavior and `OPEN` questions. It can hand off a sufficiently defined and verifiable scope without reconstructing the entire project; material unanswered questions about that scope still block implementation.

### Gradual adoption

Existing projects may migrate gradually and retain equivalent documentation structures. Move each decision or task once, replace its old content with a link, and preserve pending work, task IDs, review links, and verification evidence. During partial migration, the root index also identifies the exact legacy sections that still own work without copying their tasks. Keep existing task IDs and qualify references with requirement IDs where needed. A small change does not require a full migration.

## Agent roles and permissions

Roles are independent of model provider, cost, or capability. The same model can serve different roles in separate sessions. Record assignments in the repository operating instructions when useful; switching roles does not waive authorization or an independently required review.

| Role | Responsibilities and boundaries |
| --- | --- |
| **Designer** | Defines and maintains requirements, technical plans, and authorized tasks within approved decisions. Does not implement code by default. |
| **Implementer** | Implements authorized code and tests, updates task states and concise execution evidence, and records corrections in an existing review. Cannot change `SPEC.md`, `PLAN.md`, task scope, or acceptance criteria. Escalates material ambiguities. |
| **Reviewer** | Checks the implementation against **each applicable acceptance criterion**, relevant design, and regression risks; records findings, verdicts, and review status. May adjust specifications or task definitions only for approved decisions or verified inconsistencies. Does not implement code by default. |

**Permission to edit is not permission to invent decisions.** Material requirement, architecture, and policy changes require authorization regardless of the active role.

## Compact tasks and states

Keep the requirement `TASKS.md` readable as a compact table first. Expand only tasks that need additional direction; link acceptance criteria rather than repeating specifications or technical plans.

```markdown
| ID | Task | Status | Depends on |
| --- | --- | --- | --- |
| T-01 | Create user persistence | DONE | — |
| T-02 | Implement login endpoint | IN_REVIEW | T-01 |
| T-03 | Validate tokens | TODO | T-02 |

## T-03 — Validate tokens

Scope:
- Validate token signature and expiration.

Acceptance:
- SPEC.md → Authentication / token validation

Verification:
- Unit and integration tests
```

Use qualified IDs such as `REQ-001/T-03` across files; short IDs are sufficient inside their own requirement. Task states are `TODO`, `IN_PROGRESS`, `IN_REVIEW`, `BLOCKED`, and `DONE`. The implementer can update progress and evidence but cannot self-approve a required review. `DONE` requires completed verification and reviewer approval when review is required. The root `TASKS.md` stays an index and does not track execution states.

## Multi-agent workflow

1. The **Designer** defines authorized tasks, dependencies, acceptance references, and verification guidance.
2. The **Implementer** executes only the authorized work, runs relevant checks, and records concise evidence. Any material conflict returns to the Designer or user for a decision.
3. When review is requested, the **Reviewer** inspects code, callers, and relevant tests, assesses each applicable acceptance criterion as `PASS`, `FAIL`, or `NOT_VERIFIED`, and writes a review file even when no findings exist.
4. For blocking findings, the Implementer makes authorized corrections and appends correction evidence to the **same** review file; the Reviewer verifies and records the next round.
5. Mark tasks `DONE` only after agreed verification and required review approval. Blocking findings retain `BLOCKED`; corrected findings awaiting review retain `IN_REVIEW`.

Use one stable review file per scope, e.g. `reviews/REQ-001-T03.md` for a small task or `reviews/REV-004.md` for a broad or cross-requirement review. Explicitly list the qualified task IDs inside the report. Keep existing legacy review files for subsequent rounds; do not erase earlier evidence. A review records its implementation reference, scope, verdict (`PASS`, `CHANGES_REQUIRED`, `NOT_VERIFIED`), acceptance evidence, findings, and checks performed or omitted. A `PASS` requires all applicable criteria to pass and no open blocking findings; passing tests alone is insufficient.

Findings use stable IDs such as `R-01` and statuses `OPEN`, `AWAITING_REVIEW`, and `CLOSED`. The Implementer can mark fixes `AWAITING_REVIEW` but cannot close findings or change the review verdict. Link the review from the relevant requirement tasks instead of copying findings into `TASKS.md`.

See the canonical [review and correction protocol](skills/minimal-sdd/SKILL.md#multi-agent-implementation-and-review).

### Example: implement and review a task

| Step | Role | Request |
| --- | --- | --- |
| Design | Designer | Define `REQ-001/T-03`, its acceptance references, and verification. |
| Implement | Implementer | Implement `REQ-001/T-03` and record checks and limitations. |
| Review | Reviewer | Review `REQ-001/T-03` against its acceptance criteria; record the verdict in `reviews/REQ-001-T03.md`. |
| Correct | Implementer | Address blocking findings in scope and append evidence to the same review. |
| Re-review | Reviewer | Verify fixes, close resolved findings, and append a new round to the same review. |

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
- Tasks authorize bounded, verifiable work and link to acceptance criteria; they do not track every micro-action or document-only administration.
- Update only the sources of truth that changed, respecting role permissions and review gates.
- A bug fix that restores specified behavior, or an internal refactor, does not require artifact changes by default.
