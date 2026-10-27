---
name: sdd-foundation
description: Collaborates on defining or recovering a global system foundation, requirement-specific SPEC.md, PLAN.md, TASKS.md, and repository operating instructions for a new or still-ambiguous project without inventing decisions or implementing code by default.
---

# SDD Foundation

Help turn an idea, request, or underdefined repository into a minimal, agreed global foundation and requirement specifications. Leave enough traceable, useful decisions for `minimal-sdd` to govern later implementation; do not produce documents ceremonially. This instruction is portable: do not assume a particular agent, command syntax, shell, filesystem location, or operating system.

## Scope and boundaries

Use this skill when starting a project, when governance artifacts are missing, or when existing artifacts do not make it possible to decide what to implement. Do not use it to implement an already-defined change; use `minimal-sdd` instead.

Do not implement code, run migrations, or change external services unless the user explicitly requests it after accepting the artifacts. Do not assume requirements, architecture, data, providers, or policies that are not in the request or repository.

## Agent roles and permissions

Roles are independent of model vendor, price, or capability. A model can take different roles in separate sessions; record the project's role assignments in its operating-instructions file when applicable. Use the same permissions throughout foundation and later implementation:

- **Designer:** investigates, defines, and maintains `SPEC.md`, `PLAN.md`, and task definitions within approved decisions. Foundation work normally runs in this role.
- **Implementer:** executes authorized tasks and updates only implementation, tests, task states, and concise execution evidence; must not edit `SPEC.md`, `PLAN.md`, task scope, or acceptance criteria.
- **Reviewer:** independently checks acceptance, records findings, and may correct specification or task artifacts only to reflect approved decisions or verified inconsistencies. New material decisions must be escalated; review does not automatically authorize implementation edits.

Permission to edit does not imply permission to invent decisions. Role exceptions must be explicit; required reviews cannot be self-approved by the implementer. `minimal-sdd` owns the detailed review/correction procedure.

## Discover first

1. Read the repository operating-instructions file recognized by the active agent, if present, and existing documentation; inspect the repository's actual state when one exists. When documentation is distributed, read the constitution, global specification and plan, and requirement index first, then the active requirement and its relevant dependencies. Do not load every requirement folder routinely.
2. For an existing codebase, classify material discoveries as `CONFIRMED` (validated intended behavior/decision), `INFERRED` (observed in code but not approved as desired behavior), or `OPEN` (unresolved decision). Code behavior alone does not prove product intent. Attach these labels only where uncertainty materially matters, not to every statement.
3. Ask only questions whose answers change scope, behavior, data, architecture, security, cost, or a difficult-to-reverse decision. Group them and offer brief alternatives when useful.
4. Do not block independent decisions on an open question. Record it in the relevant artifact together with its impact. A foundation is sufficient for handoff **per affected scope** when its requirements, acceptance criteria, and material design choices are clear enough to authorize and verify tasks; unrelated inferred behavior or open questions may remain. Do not present inferred behavior as approved requirements.

When equivalent documentation already exists — for example ADRs, a backlog, a distributed specification, or instructions in subdirectories — propose minimal consolidation or update those sources; do not impose duplicate artifacts. Existing projects may migrate gradually: move each decision or task once, replace its old content with a link, and preserve task IDs, pending work, review links, and verification evidence. During partial migration, identify the exact legacy sections that still own work in the root index; do not copy their tasks into it.

## Synthesize artifacts

Use a global foundation at the root and `specs/REQ-<number>-<name>/` for each independent requirement as the default layout. Create or adjust artifacts in this order. Keep each one brief and the single source of truth for its kind of decision.

1. `CONSTITUTION.md`: durable principles, quality priorities, document hierarchy, and change rules. Include only confirmed, reusable rules; it should change rarely.
2. Root `SPEC.md`: system problem, actors, goals, scope and exclusions, main modules and their relationships, global functional invariants, and system-wide requirements and constraints. State assumptions and open questions that affect behavior; keep detailed requirement flows and acceptance criteria local.
3. Root `PLAN.md`: shared architecture, module responsibilities, sources of truth, shared contracts, data, integrations, technical constraints, risks, and system-wide test strategy. Ground the plan in the repository when it exists; distinguish undecided alternatives from approved decisions.
4. For each agreed independent requirement, create `specs/REQ-001-name/SPEC.md`, `PLAN.md`, and `TASKS.md` as needed: specify scope, flows, business rules, observable acceptance; then approved technical solution and verification; then authorized work ordered by dependency. Use the next unused requirement number, a short name, and stable qualified task IDs such as `REQ-001/T-01`. Start `TASKS.md` with a compact `ID | Task | Status | Depends on` table. Expand a task only when necessary with `Scope`, `Acceptance` (links to `SPEC.md` criteria), and `Verification`; do not copy the criteria or decompose micro-actions. Use `TODO` initially; `minimal-sdd` defines transitions and review states. Do not create tasks for document edits alone.
5. Root `TASKS.md`: index with requirement ID, brief description, folder link, and dependencies. Link the local sources; do not duplicate executable tasks or their execution statuses. The index alone does not authorize implementation.
6. A repository instruction file: repository-specific operating instructions: selective reading order, real commands, conventions, verification, and stop conditions. Direct the agent to the global foundation and index, then the active requirement and relevant dependencies. Use the filename or format recognized by the active agent, or `AGENTS.md` when no adapter requires another name. Derive it from the project; do not present generic commands as verified.

Create requirement folders for work with its own scope and acceptance criteria, not automatically for every module or request. A cross-module requirement may use one folder and link the affected requirements. Do not create folders for modules with no agreed work. Keep the existing folder and its specifications current when a requirement evolves, adding new tasks without reopening completed ones; Git preserves history. Small adjustments belong in the existing requirement. Fixes that restore specified behavior and refactors without functional or architectural changes do not require new folders or routine artifact edits.

Use this precedence: `CONSTITUTION.md` → global `SPEC.md` and `PLAN.md` → requirement `SPEC.md` → requirement `PLAN.md` → requirement `TASKS.md` → code and tests. At each level, technical decisions must satisfy functional requirements. Local artifacts must satisfy global decisions and link them rather than copy them. If a requirement needs a global decision to change, resolve and update it first, then propagate the change to affected requirements. Repository instructions govern operation and do not duplicate product or architecture decisions.

When using multiple agents, record which model/session acts as Designer, Implementer, and Reviewer in repository operating instructions, respecting the user's choices without tying roles to cost or capability. Give tasks sufficient references and verification guidance for handoff between sessions. Do not introduce an orchestrator. `minimal-sdd` governs review records, correction rounds, and task completion; do not create reviews during foundation drafting.

## Agreement points

Before moving from functional definition to the technical plan, confirm the decisions that determine the design. Before turning the plan into tasks, confirm that the scope and strategy are sufficient. If the user prefers incremental progress, provide a draft clearly marked as a proposal and request validation only for material decisions.

Do not turn a technical suggestion into an approved decision. When the user changes a requirement during the conversation, return to the highest-precedence affected artifact and propagate only what is necessary downstream.

## Delivery and handoff

At completion, report concisely:

1. inspected context;
2. confirmed decisions and open questions;
3. artifacts created or updated;
4. first authorized task set;
5. recommended next step.

After the affected foundation is accepted, hand that scope to `minimal-sdd`. State which requirements/decisions are `CONFIRMED`, which are still `INFERRED` or `OPEN`, and whether any materially blocks the specific authorized tasks. Do not require a perfect reconstruction of the whole codebase or declare an unresolved affected scope ready for implementation.
