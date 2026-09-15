---
name: sdd-foundation
description: Collaborates on defining or recovering CONSTITUTION.md, SPEC.md, PLAN.md, TASKS.md, and AGENTS.md for a new or still-ambiguous project without inventing decisions or implementing code by default.
---

# SDD Foundation

Help turn an idea, request, or underdefined repository into a minimal, agreed SDD foundation. The goal is not to produce five documents ceremonially: leave enough traceable, useful decisions for `minimal-sdd` to govern later implementation.

## Scope and boundaries

Use this skill when starting a project, when governance artifacts are missing, or when existing artifacts do not make it possible to decide what to implement. Do not use it to implement an already-defined change; use `minimal-sdd` instead.

Do not implement code, run migrations, or change external services unless the user explicitly requests it after accepting the artifacts. Do not assume requirements, architecture, data, providers, or policies that are not in the request or repository.

## Discover first

1. Read `AGENTS.md` and existing documentation; inspect the repository's actual state when one exists.
2. Distinguish observable facts from hypotheses and pending decisions.
3. Ask only questions whose answers change scope, behavior, data, architecture, security, cost, or a difficult-to-reverse decision. Group them and offer brief alternatives when useful.
4. Do not block independent decisions on an open question. Record it in the relevant artifact together with its impact.

When equivalent documentation already exists — for example ADRs, a backlog, a distributed specification, or instructions in subdirectories — propose minimal consolidation or update those sources; do not impose duplicates with the five names.

## Synthesize artifacts

Create or adjust artifacts in this order. Keep each one brief and the single source of truth for its kind of decision.

1. `CONSTITUTION.md`: durable principles, quality priorities, document hierarchy, and change rules. Include only confirmed, reusable rules; it should change rarely.
2. `SPEC.md`: problem, users or actors, goals, scope and exclusions, flows, business rules, requirements, and observable acceptance criteria. State assumptions and open questions that affect behavior.
3. `PLAN.md`: technical approach for meeting the specification: components, contracts, data, integrations, risks, design decisions, and test strategy. Ground the plan in the repository when it exists; distinguish undecided alternatives from approved decisions.
4. `TASKS.md`: authorized work units, ordered by dependency when relevant, with observable verification. Do not decompose micro-actions or create tasks solely for document edits.
5. `AGENTS.md`: repository-specific operating instructions: reading order, real commands, conventions, verification, and stop conditions. Derive them from the project; do not present generic commands as verified.

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
