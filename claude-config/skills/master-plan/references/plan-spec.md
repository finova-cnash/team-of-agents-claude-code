# Master plan specification

The single source of truth for what a generated master plan must look like. The folder layout, file requirements, plan content rules, and formatting bind every role. The design requirements bind the planning agent that designs the plan. The assessor checks the design against all of it. The build agent writes complete file contents.

## Folder layout

A master plan is a folder containing:

- `MASTER_PLAN.md` — navigational hub
- `master-plan-checklist.md` — top-level completion index
- `BLOCKER_REGISTER.md` — living blocker log
- One sub-plan folder per top-level task, named `ID-task-slug/`, each containing `SUB_PLAN.md` and `sub-plan-checklist.md`
- Optional cross-cutting files

## MASTER_PLAN.md

Navigational hub. Customise every section to the project.

- §1 Mission — what the project is, what it delivers, and explicit out-of-scope items.
- §2 Agent Protocol — step-by-step workflow for agents executing sub-plans. Adapt it to the project's toolchain, build process, and testing methodology. Include read order, prerequisite checking, bottom-up work order, checklist discipline, blocker handling, and continuous execution with no check-ins or approval gates (Rule B).
  - Add a clearly labelled "Checklist purpose and usage" subsection. State that `master-plan-checklist.md` is the shallow status index for top-level task completion, not a place for implementation detail. Explain that agents should read `MASTER_PLAN.md` and the relevant `SUB_PLAN.md` before working, use the sub-plan checklist for step-level progress, and mark the master checkbox only when that sub-plan's acceptance criteria are met.
  - State the anti-drift rule explicitly: keep each master checklist line brief and stable (task ID, title, and completion state only, with at most a short pointer to the authoritative plan); put current position, dated progress notes, diagnostics, design decisions, command results, open questions, and next actions in the relevant `SUB_PLAN.md`, `MASTER_PLAN.md`, `BLOCKER_REGISTER.md`, or named cross-cutting ledger instead. Update those authoritative files rather than duplicating their contents in the checklist.
- §3 Top-level task index — table with columns ID | Title | Depends on | Key output. Assign task IDs appropriate to the project: invent a scheme that communicates task type at a glance (e.g., bootstrap, build, milestone, discovery, verification, cutover, cross-cutting).
- §4 Dependency DAG — ASCII art showing task dependencies and parallelism, with a note on recommended maximum parallelism.
- §5 Cross-cutting rules — project-wide constraints (e.g., testing requirements, naming conventions, determinism rules, style guides, review gates). These must be customised: do not copy rules from other projects unless they genuinely apply. Reference any cross-cutting files here.
- §6 Blocker Register entry template — adapt the BR-NNN template. Keep the core fields (Discovered, Symptom, Disposition, Decision, Cost) but add or remove fields as appropriate for the project type.
- §7 Escalation Protocol — steps when a work-around is impossible. Invoke it only when the agent is completely and unrecoverably blocked and nothing executable remains (Rule B); otherwise log the blocker, work around it, or move to the next unblocked task. Customise to the project's workflow (who to notify, where to file, what to do next).
- §8 AI Escalation Prompt Template — template for creating self-contained prompts. Adapt the sections (Context, Feature Required, Specification, etc.) to the project.
- §9 Conventions — testing methodology, coding standards, naming conventions, file organisation. Must be project-specific.
- §10 Definition of Done — concrete, verifiable criteria for "this project is complete".

## master-plan-checklist.md

A deliberately lightweight completion index. It is not a second plan, work log, decision record, or blocker register.

- One `[ ]` line per top-level task ID from §3 (later marked `[x]`).
- Only a brief, stable description per line; no running status narratives, dated progress logs, implementation details, diagnostics, decisions, or next-action lists.
- Include a comment noting which agent marks each item complete and where detailed progress belongs (the relevant `SUB_PLAN.md` or linked register).
- Mark an item `[x]` only after the corresponding sub-plan's acceptance criteria are met; never on partial step completion.
- If the dependency structure justifies pre-task fix sections, add them (e.g., "## Pre-P02 Fixes"), but keep them as checkbox-only completion items that point to the authoritative file for detail.

## BLOCKER_REGISTER.md

- Header explaining it is a living log of BR-NNN entries.
- The blocker entry template from MASTER_PLAN.md §6.
- A "Pre-populated known risks" section listing any blockers the user already mentioned in their draft or that are recorded in any source plan being rebuilt from.
- Otherwise empty, ready for entries.

## Sub-plan folders

One folder per top-level task, named `ID-task-slug/`.

`SUB_PLAN.md` contains:

- Goal — one paragraph: what this sub-plan accomplishes.
- Prerequisites — which other sub-plans or external preconditions must be complete.
- Inputs — specific files, documents, or resources needed.
- Outputs — specific deliverables (files created, systems built, documents produced).
- Task breakdown — numbered steps, each with `[prereqs: ...]` notation. Steps must be concrete enough that an agent can execute them one at a time. Order bottom-up: simplest and least-dependent first.
- Acceptance criteria — objective checks that prove the sub-plan is complete.
- Known risks / open questions — anticipated problems specific to the sub-plan.
- Reference files — paths to project files the agent must read (source code, specs, and other durable project inputs only; never older or source plan documents).

`sub-plan-checklist.md` contains:

- One `[ ]` line per task step from `SUB_PLAN.md`, with a brief description matching the step.
- Use it only for step-level completion tracking; implementation detail and dated progress stay in `SUB_PLAN.md`.

## Optional cross-cutting files

If the project brief explicitly mentions cross-cutting concerns spanning multiple sub-plans (e.g., port-parity ledger, migration tracker, API compatibility matrix), design those files too. Name them clearly and reference them in MASTER_PLAN.md §5. If a cross-cutting file seems needed but the user did not state it, the planning agent lists it as a non-blocking question at the top of its response and does not design that file until the user confirms.

## Plan content rules

These rules apply to every generated file: MASTER_PLAN.md, sub-plans, checklists, BLOCKER_REGISTER.md, cross-cutting files, and any other output.

### Self-contained

A generated master plan must not reference older plans or other source planning documents (older master plans, high-level plans, draft plans, handover notes, or similar) that it was built from. Anything relevant from those sources must be copied into the new plan's files. The user may delete the older plans once the new master plan exists, believing them obsolete, so any reference to them would dangle, and any content left only in them would be lost.

The planner and build agent may read source documents while designing and authoring. The generated files must not link to, cite, or say "see the old plan" or "as in the previous plan". Relevant decisions, requirements, constraints, risks, completed-work status, and open questions from sources must be copied, verbatim or faithfully restated, into the appropriate new file, with the source path omitted. Completed-work status goes into the relevant checklists as pre-checked `[x]` items, with a status note in the relevant `SUB_PLAN.md`; blockers recorded in source plans go into BLOCKER_REGISTER.md.

This rule does not ban references to the project's own source code, specs, or other non-plan artifacts the project keeps as durable inputs.

### Rule A: no generic preflight boilerplate

Generated plans must not contain generic preflight, environment-audit, or defensive-ceremony steps. Example of what must not appear (in this or any similar form):

`Preflight: record working-tree baseline and unrelated changes, declared runtime/platform support, configured roots, live jobs, tool versions, credential availability and data-retention permissions. Recheck actual source paths/line positions. Work only within configured repositories/caches.`

The banned category covers recording baselines or working-tree state, inventorying tool or runtime versions, checking credentials or permissions, auditing live jobs, re-verifying file paths or line positions, scope-fencing statements ("work only within…"), and similar ritual checks that produce no project deliverable. Allowed: a concrete, project-specific setup step that produces an output the plan needs (e.g., "install dependency X required by step 4"), written as an ordinary task step.

### Rule B: continuous execution

Implementation must run continuously, sub-plan after sub-plan, without pausing for check-ins, approvals, progress reports, or session boundaries. An agent stops only when it is completely and unrecoverably blocked: no work-around exists and no other unblocked task in any sub-plan remains. For a blocker on one task, log it in BLOCKER_REGISTER.md, apply a work-around if possible, and otherwise move on to the next unblocked task; escalate per §7 only when nothing executable remains. Generated plans must not contain "stop here", "pause for review", "check in with the user", or per-phase approval gates unless the user's brief explicitly requires them.

### Rule C: no time estimates

Generated plans must not include or allude to execution time or effort: no durations, hours/days/weeks, dates or deadlines, sprints, timelines, story points, T-shirt sizes, "quick"/"lengthy" qualifiers, or session counts.

## Scaling down

A one-task project still produces the full folder structure. MASTER_PLAN.md may contain only §1, §3 (one row), §5, and §10, and may skip §4 if there are no dependencies. If the single task has more than one step, keep the continuous-execution guidance (§2) and the escalation guidance (§7) in condensed form, for example within §5.

## Design requirements (planning agent)

- Analyse the project brief: identify natural top-level tasks, their dependencies, and a logical execution order.
- Assign task IDs that communicate task type at a glance.
- Customise every section to the project. Do not copy project-specific rules from other plans unless the project genuinely requires them.
- Break each sub-plan into 5–15 ordered, concrete steps, bottom-up, each small enough to execute and verify as a single unit, with `[prereqs: ...]` notation.
- Completeness over brevity: for each file, give its exact output path, purpose, required sections/headings/checklist/tables/relationships, project-specific facts/decisions/constraints/acceptance criteria, relevant content from any source plan (copied into the file, with the source path omitted), and dependencies. Describe what each file must contain and why; do not emit complete file bodies unless the user explicitly requires a byte- or wording-exact artifact.

## Formatting (all generated files)

- Do not manually word-wrap generated markdown. Write each paragraph as a single continuous line; use newlines only to separate paragraphs, list items, headings, and fenced code blocks. Users can enable their editor's soft word-wrap.
