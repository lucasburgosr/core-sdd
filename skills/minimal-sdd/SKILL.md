---
name: minimal-sdd
description: Keeps CONSTITUTION.md, SPEC.md, PLAN.md, TASKS.md, and repository operating instructions aligned while evolving, implementing, or reviewing already-defined software changes without unnecessary bureaucracy.
---

# Minimal SDD

Keep decisions, work, and implementation aligned through five small artifacts. Write artifacts and respond in the language established by the user or repository. This instruction is portable: do not assume a particular agent, command syntax, shell, filesystem location, or operating system.

## Guiding principle

When a requirement or decision changes, update the affected artifacts before implementation. Reviewing a file does not require editing it: change it only when its source of truth changes.

For functional changes, use this precedence:

`CONSTITUTION.md` → `SPEC.md` → `PLAN.md` → `TASKS.md` → code and tests

The repository operating-instructions file governs how work is performed and is updated when those instructions change; it is not part of the default chain. Use the filename or format recognized by the active agent, or `AGENTS.md` when no adapter requires another name. If artifacts contradict each other, do not implement until the conflict is resolved with the user or through an explicit decision.

## Artifact responsibilities

- `CONSTITUTION.md`: stable principles, hierarchy, quality expectations, and reusable decision rules. Keep it brief and change it rarely.
- `SPEC.md`: functional source of truth: problem, goals, scope, flows, business rules, requirements, and acceptance criteria. Do not include technical design unless it is a real requirement.
- `PLAN.md`: approved technical source of truth: architecture, components, contracts, persistence, integrations, design decisions, risks, and test strategy.
- `TASKS.md`: authorized executable work, with scope and observable verification where appropriate.
- A repository instruction file: operating instructions such as reading order, commands, conventions, boundaries, and stop conditions. Use the filename or format recognized by the active agent, or `AGENTS.md` when no adapter requires another name. Do not duplicate functional or technical decisions.

Respect equivalent artifacts that already exist — for example ADRs, a `tasks/` directory, or distributed documentation — instead of imposing names or duplicating sources of truth.

## Workflow

1. Read the applicable repository operating-instructions file, existing SDD artifacts, and the repository's actual state.
2. Classify the change by its effect, not by the wording of the request.
3. State which artifacts you will update, which you will review without changing, and why.
4. Before implementation, update only the affected sources of truth.
5. Create or adjust the necessary executable tasks.
6. Implement the smallest change consistent with what was approved.
7. Run relevant verification and compare the artifacts with the code's actual behavior.

Ask the user for a decision only when ambiguity changes scope, behavior, data, architecture, or a difficult-to-reverse policy. Resolve reversible internal details directly.

## Minimal update matrix

| Change effect | Update before implementation |
| --- | --- |
| Observable behavior, scope, business rule, or acceptance | `SPEC.md`; review `PLAN.md`; adjust `TASKS.md` |
| Architecture, contract, persistence, provider, or technical design | `PLAN.md`; adjust `TASKS.md`; review `SPEC.md` |
| Breakdown, order, or dependency of approved work | `TASKS.md` |
| Governance or methodology rule | `CONSTITUTION.md`; reflect it in the repository operating-instructions file only if operating behavior changes |
| Repository-specific convention or instruction | Repository operating-instructions file |
| Bug fix that restores specified behavior | Code and tests; do not change artifacts by default |
| Refactor or internal detail with no functional or architectural change | Code and tests; adjust a task only when it affects pending work |

Rows are cumulative: update every artifact whose source of truth changes, not only the first matching row.

## Anti-bureaucracy policy

- Do not create additional documents when one of the five already serves that purpose.
- Do not copy content across artifacts; link to or summarize the relevant source of truth.
- Do not log every code edit or turn history into specification.
- Do not create tasks solely for document administration; update documents as part of the change that requires them.
- Fold small adjustments into the active task. Create a new task only when the work is independent, valuable on its own, and separately verifiable.
- Do not reopen completed tasks for later evolution; create a new one and modify only affected pending tasks.
- Do not fill empty sections ceremonially. Omit anything without a useful decision.
- Scale depth to project size and risk; an MVP may use very short documents.

## Boundaries and completion

This skill governs changes on top of an existing artifact foundation. If the project lacks a sufficient definition or begins from an idea, use `sdd-foundation` before implementation. Do not fill all five artifacts by routine to unblock a small change in a mature repository: preserve existing equivalents and ask only for the material decision that is missing.

Use `TASKS.md` as the boundary of authorized work. Do not introduce unrequested features, architecture decisions, or unrelated improvements. Mark a task as `DONE` only after its agreed verification passes.

At completion, report concisely:

1. change classification;
2. artifacts edited and reviewed without changes;
3. implementation and verification;
4. pending decisions or contradictions.

Do not claim alignment merely because files agree with one another: also compare them with the code's actual behavior.
