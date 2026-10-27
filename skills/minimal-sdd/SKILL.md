---
name: minimal-sdd
description: Keeps global system decisions, requirement-specific SPEC.md, PLAN.md, TASKS.md, and repository operating instructions aligned while evolving, implementing, or reviewing already-defined software changes without unnecessary bureaucracy.
---

# Minimal SDD

Keep global system decisions, requirement specifications, authorized work, and implementation aligned. Write artifacts and respond in the language established by the user or repository. This instruction is portable: do not assume a particular agent, command syntax, shell, filesystem location, or operating system.

## Guiding principle

When a requirement or decision changes, update the affected artifacts before implementation. Reviewing a file does not require editing it: change it only when its source of truth changes.

For functional changes, use this precedence:

`CONSTITUTION.md` → global `SPEC.md` and `PLAN.md` → requirement `SPEC.md` → requirement `PLAN.md` → requirement `TASKS.md` → code and tests

At each level, technical decisions must satisfy the functional specification. Local decisions must satisfy the global foundation. If a requirement needs a global decision to change, resolve and update that decision first, then propagate the change to affected requirements before implementation. A local plan cannot silently override a global rule.

The repository operating-instructions file governs how work is performed and is updated when those instructions change; it is not part of the default chain. Use the filename or format recognized by the active agent, or `AGENTS.md` when no adapter requires another name. If artifacts contradict each other, do not implement until the conflict is resolved with the user or through an explicit decision.

## Artifact responsibilities

- `CONSTITUTION.md`: stable principles, hierarchy, quality expectations, and reusable decision rules. Keep it brief and change it rarely.
- Root `SPEC.md`: system problem, actors, goals, scope, main modules, their relationships, functional invariants, and system-wide requirements and constraints. Keep requirement details in their own specifications.
- Root `PLAN.md`: approved shared architecture, module responsibilities, sources of truth, shared contracts, persistence, integrations, technical constraints, risks, and system-wide test strategy.
- Root `TASKS.md`: requirement index with stable ID, brief description, link to its folder, and dependencies. Do not duplicate executable tasks or their execution statuses here.
- `specs/REQ-001-name/SPEC.md`: the requirement's scope, flows, business rules, observable behavior, and acceptance criteria. Link relevant global decisions and requirement dependencies; do not copy them. Do not include technical design unless it is a real requirement.
- Requirement `PLAN.md`: approved technical solution, affected components, contracts, data, integrations, risks, and verification for that requirement.
- Requirement `TASKS.md`: authorized executable work, dependencies, acceptance criteria, and verification evidence. Use stable IDs qualified by requirement, such as `REQ-001/T-01`.
- A repository instruction file: operating instructions such as reading order, commands, conventions, boundaries, and stop conditions. Use the filename or format recognized by the active agent, or `AGENTS.md` when no adapter requires another name. Do not duplicate functional or technical decisions.

Respect equivalent artifacts that already exist — for example ADRs, a `tasks/` directory, or distributed documentation — instead of imposing names or duplicating sources of truth.

## Agent roles and permissions

Roles define permissions, not model quality, vendor, or cost. One model may serve different roles in separate sessions; record assignments in the repository operating-instructions file when useful. In a single-agent workflow, explicitly identify the active role before acting; switching roles does not waive authorization or required independent review.

- **Designer:** defines or recovers specifications, technical plans, and authorized tasks; may edit `SPEC.md`, `PLAN.md`, and task definitions within approved decisions. Does not implement by default.
- **Implementer:** changes code and tests only within authorized tasks; may update task status and concise implementation notes/evidence, and append correction evidence to an existing review. Must not edit `SPEC.md`, `PLAN.md`, task scope, or acceptance criteria. Escalate material contradictions or new decisions instead of silently resolving them.
- **Reviewer:** independently checks implementation against each applicable acceptance criterion, plans, and regression risks; maintains review findings and verdicts and updates task review status. May revise specifications or task definitions only to capture an authorized decision or rectify a verified inconsistency; escalate new product/architecture choices to the user or designer. Does not modify implementation by default.

Editing permission is not decision authority. New or changed requirements, material architectural choices, and difficult-to-reverse policies require authorization regardless of role. Role exceptions must be explicit; an implementer cannot self-approve a review required to close a task.

## Requirement lifecycle and compatibility

The default layout is a global foundation at the root and `SPEC.md`, `PLAN.md`, and `TASKS.md` under `specs/REQ-<number>-<name>/` for each independent requirement. Choose the next unused requirement number and a short descriptive name; keep its ID stable. Create a folder when the work has its own scope and acceptance criteria, not for every request or module. A cross-module requirement has its own folder and links the other requirements it affects.

Keep the existing folder and its specifications current when that requirement evolves. Add new tasks for later work without reopening completed tasks; Git preserves specification history. Fold small adjustments into the existing requirement. Bug fixes that restore specified behavior and refactors without functional or architectural changes retain the exceptions below and do not require a new folder or routine artifact edits.

Existing projects may migrate gradually. Move each decision or task once, replacing its former content with a link. Preserve pending work, task IDs, review links, and verification evidence. During partial migration, the root index must identify both migrated requirements and the exact legacy sections that still own work; it must not duplicate their tasks. Qualify legacy task references with the new requirement ID when needed for disambiguation without renumbering the original tasks. Never discard completed-work evidence or force a migration to perform a small change.

## Compact tasks and states

The requirement `TASKS.md` starts with a compact index; use qualified IDs in references (`REQ-001/T-03`) and short local IDs in its own table when unambiguous:

```markdown
| ID | Task | Status | Depends on |
| --- | --- | --- | --- |
| T-01 | Create user persistence | DONE | — |
| T-02 | Implement login endpoint | IN_REVIEW | T-01 |
| T-03 | Validate tokens | TODO | T-02 |
```

Expand only tasks needing more direction, using a concise `## T-03 — Validate tokens` section with `Scope`, `Acceptance` (links to criteria in `SPEC.md`), and `Verification` (how to demonstrate completion). Add implementation notes or review links only when relevant. Do not restate specifications, plans, or long histories. If acceptance criteria lack IDs, cite a stable heading or short description; do not manufacture IDs solely for formatting. Preserve existing equivalent task conventions when migrating.

Task states: `TODO` (authorized, not started), `IN_PROGRESS` (implementing), `IN_REVIEW` (submitted for required review), `BLOCKED` (material impediment or blocking findings), `DONE` (verification complete and required review passed). An implementer may move tasks among the first four states and record evidence, but cannot mark a review-required task `DONE` before reviewer approval. Without required independent review, mark `DONE` after agreed verification. A reviewer may set `BLOCKED`, `IN_REVIEW`, or `DONE` according to the verdict. Keep status in the requirement task record; the root index does not track execution.

## Workflow

1. Read the repository operating instructions, constitution, global specification and plan, and root requirement index. Use the index to locate the active requirement, then read its artifacts and the relevant dependencies. Do not load every requirement folder routinely. Inspect the relevant implementation as well.
2. Classify the change by its effect, not by the wording of the request; select the existing requirement or create one for independently scoped work.
3. State which sources of truth are affected and why.
4. Before implementation, update only the affected sources of truth, resolving global changes before their local consequences. Check linked requirements when shared behavior or contracts change.
5. Create or adjust executable tasks in the requirement's `TASKS.md`; update the root index only when its entries or dependencies change.
6. Implement the smallest change consistent with what was approved.
7. Run relevant verification and compare the artifacts with the code's actual behavior.

Ask the user for a decision only when ambiguity changes scope, behavior, data, architecture, or a difficult-to-reverse policy. Resolve reversible internal details directly.

## Multi-agent implementation and review

Assign Designer, Implementer, and Reviewer explicitly when using multiple agents; respect user assignments without assuming that model capability or price determines role. Roles may operate in separate sessions; no orchestrator is required.

The designer leaves authorized qualified task IDs, dependencies, links to applicable acceptance criteria, and verification guidance. The implementer reads the applicable artifacts, implements only authorized work, and records concise evidence and limitations in the task entry or existing review. Implementation alone does not complete a task whose review is required.

### Review record

When asked to review implemented tasks, inspect the actual code, its callers, and relevant tests against the applicable artifacts. Create `reviews/` in the target project and write the review there, including when no findings are identified. Respect an existing review convention if the repository already defines one.

Use one stable file per reviewed scope: `reviews/REQ-001-T03.md` for a small review or `reviews/REV-004.md` for a broad/nonconsecutive or cross-requirement scope. Explicitly list every qualified task in the report; a filename never implies that intervening tasks were reviewed. Reuse existing legacy review files and the same file for corrections and subsequent rounds; never erase earlier evidence.

Keep the report concise and actionable:

- **Scope and verdict:** qualified task IDs, implementation reference (commit when available, otherwise working tree), and `PASS`, `CHANGES_REQUIRED`, or `NOT_VERIFIED`.
- **Acceptance:** evaluate **each applicable acceptance criterion** as `PASS`, `FAIL`, or `NOT_VERIFIED`, with concise evidence or a reason it could not be checked. Reference stable criterion IDs if present, otherwise headings or descriptions. `PASS` requires every applicable criterion to pass and no open blocking findings. Passing tests alone is insufficient. An unverified applicable criterion prevents `PASS` unless an authorized decision explicitly changes verification expectations.
- **Findings:** stable ID such as `R-01`, affected task/criterion, blocking or optional severity, location, problem, impact, required correction, and state `OPEN`, `AWAITING_REVIEW`, or `CLOSED`. By default, unmet acceptance criteria and correctness, security, or data-loss defects are blocking.
- **Verification:** checks actually performed and results, including regression risks and limitations relevant to the change; distinguish test execution from evidence that observable acceptance is met.
- **Rounds:** append dated correction and re-review entries with the implementation reference, finding IDs addressed, evidence, remaining findings, and current verdict. Preserve prior entries; update the current verdict and finding statuses to reflect the latest reviewed state. State explicitly when there are no findings.

### Correction loop and completion

When asked to apply corrections, the implementer reads the review and fixes the blocking findings within the authorized task scope. Append changes and verification to the same review file without changing the reviewer's verdict or closing findings; mark addressed findings `AWAITING_REVIEW`. The reviewer checks those changes, closes resolved findings or explains why they remain open, and appends the next review round to that file. Keep finding IDs stable across rounds and assign new IDs only to new findings.

Repeat this handoff until the required review passes. Keep affected tasks `BLOCKED` while blocking findings remain, or `IN_REVIEW` while fixes await reviewer verification. Link the review from the relevant requirement task entries rather than duplicating its findings in `TASKS.md`. If a correction requires a new product or architecture decision, update the affected source of truth before implementation; a review does not authorize unrelated work.

## Minimal update matrix

| Change effect | Update before implementation |
| --- | --- |
| Requirement behavior, scope, business rule, or acceptance | Requirement `SPEC.md`; review its `PLAN.md`; adjust its `TASKS.md` |
| Requirement contract, persistence, provider, or technical design | Requirement `PLAN.md`; adjust its `TASKS.md`; review its `SPEC.md` |
| Global behavior, invariant, architecture, or shared contract | Affected root `SPEC.md` and/or `PLAN.md` first; propagate to affected requirement artifacts and tasks |
| Breakdown or order of approved work | Requirement `TASKS.md` |
| New independent requirement or changed requirement dependencies | Root index and applicable requirement artifacts; no duplicated tasks |
| Governance or methodology rule | `CONSTITUTION.md`; reflect it in the repository operating-instructions file only if operating behavior changes |
| Repository-specific convention or instruction | Repository operating-instructions file |
| Bug fix that restores specified behavior | Code and tests; do not change artifacts by default |
| Refactor or internal detail with no functional or architectural change | Code and tests; adjust a task only when it affects pending work |

Rows are cumulative: update every artifact whose source of truth changes, not only the first matching row. Apply the same responsibilities to legacy or equivalent artifacts during migration.

## Anti-bureaucracy policy

- Use the global foundation and requirement artifacts for their defined responsibilities; do not add documents that duplicate them. Requested task reviews belong in `reviews/` as verification evidence, not as another source of requirements.
- Do not copy content across artifacts; link to or summarize the relevant source of truth.
- Do not log every code edit or turn history into specification.
- Do not create tasks solely for document administration; update documents as part of the change that requires them.
- Fold small adjustments into the active task. Create a new task only when the work is independent, valuable on its own, and separately verifiable.
- Do not reopen completed tasks for later evolution; create a new one and modify only affected pending tasks.
- Do not fill empty sections ceremonially. Omit anything without a useful decision.
- Scale depth to project size and risk; an MVP may use very short documents.

## Boundaries and completion

This skill governs changes on top of a sufficiently defined foundation for the **affected scope**. If a project begins from an idea or its affected requirements cannot be identified or verified, use `sdd-foundation` to define or recover that scope first; unrelated unknowns need not block well-defined work. Do not fill artifacts by routine to unblock a small change in a mature repository: preserve existing equivalents and ask only for the material decision that is missing.

Use the active requirement's `TASKS.md`, or its identified legacy equivalent, as the boundary of authorized work. An entry in the root index alone does not authorize implementation. Do not introduce unrequested features, architecture decisions, or unrelated improvements. Mark a task as `DONE` only after its agreed verification and any required review pass. Follow the role permissions above when updating documents or status.

At completion, report concisely:

1. change classification;
2. artifacts edited and reviewed without changes;
3. implementation and verification;
4. pending decisions or contradictions.

Do not claim alignment merely because files agree with one another: also compare them with the code's actual behavior.
