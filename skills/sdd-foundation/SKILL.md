---
name: sdd-foundation
description: Collaborates on defining or recovering a global system foundation, requirement-specific SPEC.md, PLAN.md, TASKS.md, and repository operating instructions for a new or still-ambiguous project without inventing decisions or implementing code by default.
---

# SDD Foundation

Help turn an idea, request, or underdefined repository into a minimal, agreed global foundation and requirement specifications. Leave enough traceable, useful decisions for `minimal-sdd` to govern later implementation; do not produce documents ceremonially. This instruction is portable: do not assume a particular agent, command syntax, shell, filesystem location, or operating system.

## Scope and boundaries

Use this skill when starting a project, when governance artifacts are missing, or when existing artifacts do not make it possible to decide what to implement. Do not use it to implement an already-defined change; use `minimal-sdd` instead.

Do not implement code, run migrations, or change external services unless the user explicitly requests it after accepting the artifacts. Do not assume requirements, architecture, data, providers, or policies that are not in the request or repository.

## Discover first

1. Read the repository operating-instructions file recognized by the active agent, if present, and existing documentation; inspect the repository's actual state when one exists. When documentation is distributed, read the constitution, global specification and plan, and requirement index first, then the active requirement and its relevant dependencies. Do not load every requirement folder routinely.
2. Distinguish observable facts from hypotheses and pending decisions.
3. Ask only questions whose answers change scope, behavior, data, architecture, security, cost, or a difficult-to-reverse decision. Group them and offer brief alternatives when useful.
4. Do not block independent decisions on an open question. Record it in the relevant artifact together with its impact.

When equivalent documentation already exists — for example ADRs, a backlog, a distributed specification, or instructions in subdirectories — propose minimal consolidation or update those sources; do not impose duplicate artifacts. Existing projects may migrate gradually: move each decision or task once, replace its old content with a link, and preserve task IDs, pending work, review links, and verification evidence. During partial migration, identify the exact legacy sections that still own work in the root index; do not copy their tasks into it.

## Synthesize artifacts

Use a global foundation at the root and `specs/REQ-<number>-<name>/` for each independent requirement as the default layout. Create or adjust artifacts in this order. Keep each one brief and the single source of truth for its kind of decision.

1. `CONSTITUTION.md`: durable principles, quality priorities, document hierarchy, and change rules. Include only confirmed, reusable rules; it should change rarely.
2. Root `SPEC.md`: system problem, actors, goals, scope and exclusions, main modules and their relationships, global functional invariants, and system-wide requirements and constraints. State assumptions and open questions that affect behavior; keep detailed requirement flows and acceptance criteria local.
3. Root `PLAN.md`: shared architecture, module responsibilities, sources of truth, shared contracts, data, integrations, technical constraints, risks, and system-wide test strategy. Ground the plan in the repository when it exists; distinguish undecided alternatives from approved decisions.
4. For each agreed independent requirement, create `specs/REQ-001-name/SPEC.md`, `PLAN.md`, and `TASKS.md`: define scope, flows, business rules, and observable acceptance; then the approved technical solution and verification; then authorized work ordered by dependency. Use the next unused requirement number and a short descriptive name. Give tasks stable qualified IDs, such as `REQ-001/T-01`. Do not decompose micro-actions or create tasks solely for document edits.
5. Root `TASKS.md`: index with requirement ID, brief description, folder link, and dependencies. Link the local sources; do not duplicate executable tasks or their execution statuses. The index alone does not authorize implementation.
6. A repository instruction file: repository-specific operating instructions: selective reading order, real commands, conventions, verification, and stop conditions. Direct the agent to the global foundation and index, then the active requirement and relevant dependencies. Use the filename or format recognized by the active agent, or `AGENTS.md` when no adapter requires another name. Derive it from the project; do not present generic commands as verified.

Create requirement folders for work with its own scope and acceptance criteria, not automatically for every module or request. A cross-module requirement may use one folder and link the affected requirements. Do not create folders for modules with no agreed work. Keep the existing folder and its specifications current when a requirement evolves, adding new tasks without reopening completed ones; Git preserves history. Small adjustments belong in the existing requirement. Fixes that restore specified behavior and refactors without functional or architectural changes do not require new folders or routine artifact edits.

Use this precedence: `CONSTITUTION.md` → global `SPEC.md` and `PLAN.md` → requirement `SPEC.md` → requirement `PLAN.md` → requirement `TASKS.md` → code and tests. At each level, technical decisions must satisfy functional requirements. Local artifacts must satisfy global decisions and link them rather than copy them. If a requirement needs a global decision to change, resolve and update it first, then propagate the change to affected requirements. Repository instructions govern operation and do not duplicate product or architecture decisions.

When the user chooses a multi-agent workflow, record the agreed planning, implementation, and review roles in the repository operating instructions, and give tasks qualified IDs and sufficient acceptance criteria for handoff between sessions. A more capable model plans and reviews; a lower-cost model implements, according to the user's assignments. Do not hardcode model vendors or introduce an orchestrator. Use the review and correction protocol in `minimal-sdd`: one file in root `reviews/` per reviewed task scope, including requirement IDs to prevent collisions, with corrections and subsequent review rounds recorded in that same file, and task completion gated by the required review. Create review records when implementation is reviewed, not during foundation drafting.

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

After the artifacts are accepted, state that later changes should use `minimal-sdd`. Do not declare the project ready for implementation if an open question blocks scope, behavior, data, or design.
