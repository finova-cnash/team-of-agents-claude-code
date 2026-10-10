---
name: master-plan
description: "Creates a comprehensive project master plan from draft requirements, which is a methodology of creating an overarching plan file and checklist, with multiple sub-plans and their checklists. Generates MASTER_PLAN.md, sub-plans, blocker register, and checklists. Use when the user wants to create a master plan, project plan, build plan, or multi-phase plan for any project."
---

# master-plan — Project Master Plan Generator

## Goal
Transform the user's draft requirements into a complete master plan folder. It is a methodology of creating an overarching plan file and checklist, with multiple sub-plans and their checklists: a navigational hub (MASTER_PLAN.md), a completion tracker, a blocker register, and one sub-plan folder per top-level task — each with its own SUB_PLAN.md and checklist.

## Master Plan Structure

The master plan methodology produces a standard folder layout with these files:

**MASTER_PLAN.md** — Navigational hub with these sections:
- §1 Mission — what the project delivers, including explicit out-of-scope items
- §2 Agent Protocol — step-by-step workflow for agents executing sub-plans, including the purpose and maintenance rules for the checklists
- §3 Top-level task index — table mapping task IDs to titles, dependencies, and key outputs
- §4 Dependency DAG — ASCII art showing task dependencies and parallelism
- §5 Cross-cutting Rules — project-wide constraints (testing, naming, style, etc.)
- §6 Blocker Register entry template — entry format for the blocker register
- §7 Escalation Protocol — steps when a work-around is impossible
- §8 AI Escalation Prompt Template — self-contained prompt template
- §9 Conventions — testing methodology, coding standards, file organization
- §10 Definition of Done — concrete, verifiable completion criteria

**master-plan-checklist.md** — A deliberately lightweight completion index: one checkbox per top-level sub-plan (which can be written as `[ ]` and later marked as `[x]`), plus optional pre-fix sections. It is not a second plan, work log, decision record, or blocker register.

**BLOCKER_REGISTER.md** — Living log of BR-NNN entries with template and known risks.

**Sub-plan folders** — One folder per top-level task, named `ID-task-slug/`. Each contains:
- **SUB_PLAN.md** — Goal, Prerequisites, Inputs, Outputs, Task breakdown (numbered steps with `[prereqs: ...]` notations), Acceptance criteria, Known risks, Reference files
- **sub-plan-checklist.md** — One `[ ]` per step from the task breakdown

When delegating to a planning agent, describe this structure in the delegation brief.

## Protocol

### Phase 1: Gather Inputs
Execute this skill as the main/primary orchestrator/manage agent; subagents cannot use the `question` tool, so all user clarification happens here.

Ask the user two questions:
1. "Where is your draft requirements document, or would you like to describe the project now?"
2. "Where should I create the master plan folder?" (Default: a new folder in the current workspace.)

Read the draft. If inline text, capture it verbatim. Note project name, technologies, any pre-identified sub-tasks.

### Phase 2: Assess Clarity
The draft is ready for planning if it answers at least:
- **What** is being built (concrete deliverables)
- **Scope** boundaries (what is IN and OUT)
- **Done when** (success criteria — tests pass, deployment, milestone, etc.)

If the draft is missing these, ask 2–4 focused clarifying questions (grill the user with a short interrogation) rather than a long clarifying session.

### Phase 3: Delegate to a Planning Agent
Construct a delegation to a planning agent using this template. Replace bracketed sections.

---

**Delegate to: a planning agent**

```
Design a complete master plan for the project below. Follow the master plan structure described in the skill.

## Project Brief
[INSERT USER'S DRAFT REQUIREMENTS — VERBATIM OR FAITHFULLY SUMMARIZED]

## Output Directory
[INSERT PATH — e.g., /home/user/projects/myproject/MyPlan/]

## Files to Design — Specify Deliverables and Authoring Requirements

### MASTER_PLAN.md
Navigational hub. Customize ALL sections to this project:
- **§1 Mission** — What this project is, what it delivers, explicit out-of-scope items
- **§2 Agent Protocol** — Step-by-step workflow for agents executing sub-plans. Adapt to this project's toolchain, build process, and testing methodology. Include: read order, prerequisite checking, bottom-up work order, checklist discipline, blocker handling
  - Add a clearly labelled **Checklist purpose and usage** subsection. State that `master-plan-checklist.md` is the shallow status index for top-level task completion, not a place for implementation detail. Explain that agents should read `MASTER_PLAN.md` and the relevant `SUB_PLAN.md` before working, use the sub-plan checklist for step-level progress, and mark the master checkbox only when that sub-plan's acceptance criteria are met.
  - State the anti-drift rule explicitly: keep each master checklist line brief and stable (task ID, title, and completion state only, with at most a short pointer to the authoritative plan); put current position, dated progress notes, diagnostics, design decisions, command results, open questions, and next actions in the relevant `SUB_PLAN.md`, `MASTER_PLAN.md`, `BLOCKER_REGISTER.md`, or named cross-cutting ledger instead. Update those authoritative files rather than duplicating their contents in the checklist.
- **§3 Top-level task index** — Table: ID | Title | Depends on | Key output. Assign task IDs appropriate to this project — invent a scheme that communicates task type at a glance (e.g., bootstrap, build, milestone, discovery, verification, cutover, cross-cutting)
- **§4 Dependency DAG** — ASCII art showing task dependencies. Include a note on recommended maximum parallelism
- **§5 Cross-cutting rules** — Project-wide constraints. These MUST be customized: don't copy rules from other projects unless they genuinely apply. Examples: testing requirements, naming conventions, determinism rules, style guides, review gates
- **§6 Blocker Register entry template** — Adapt the BR-NNN template. Keep the core fields (Discovered, Symptom, Disposition, Decision, Cost) but add/remove fields as appropriate for this project type
- **§7 Escalation Protocol** — Steps when a work-around is impossible. Customize to this project's workflow (who to notify, where to file, what to do next)
- **§8 AI escalation prompt template** — Template for creating self-contained prompts. Adapt the sections (Context, Feature Required, Specification, etc.) to this project
- **§9 Conventions** — Testing methodology, coding standards, naming conventions, file organization. Must be project-specific
- **§10 Definition of Done** — Concrete, verifiable criteria for "this project is complete"

### master-plan-checklist.md
- One `[ ]` line per top-level task ID from §3
- Include only a brief, stable description per line; do not add running status narratives, dated progress logs, implementation details, diagnostics, decisions, or next-action lists
- Add a comment noting which agent marks each item complete and where detailed progress belongs (the relevant `SUB_PLAN.md` or linked register)
- Mark an item `[x]` only after the corresponding sub-plan's acceptance criteria are met; do not mark it based on partial step completion
- If the dependency structure justifies pre-task fix sections, add them (e.g., "## Pre-P02 Fixes"), but keep them as checkbox-only completion items and point to the authoritative file for detail

### BLOCKER_REGISTER.md
- Header explaining this is a living log
- Include the blocker entry template (from MASTER_PLAN §6)
- Add a "Pre-populated known risks" section listing any blockers the user already mentioned in their draft
- Otherwise leave empty, ready for entries

### Sub-plan folders
One folder per top-level task, named `ID-task-slug/`. Each contains:

**SUB_PLAN.md:**
- **Goal** — One paragraph: what this sub-plan accomplishes
- **Prerequisites** — Which other sub-plans or external preconditions must be complete
- **Inputs** — Specific files, documents, or resources needed
- **Outputs** — Specific deliverables (files created, systems built, documents produced)
- **Task breakdown** — Numbered steps, each with `[prereqs: ...]` notation. Steps must be concrete enough that an agent can execute them one at a time. Order bottom-up: simplest/least-dependent first
- **Acceptance criteria** — Objective checks that prove this sub-plan is complete
- **Known risks / open questions** — Anticipated problems specific to this sub-plan
- **Reference files** — Paths to files the agent must read

**sub-plan-checklist.md:**
- One `[ ]` line per task step from SUB_PLAN.md
- Brief description per line matching the step; use it only for step-level completion tracking, with implementation detail and dated progress kept in SUB_PLAN.md

### Optional cross-cutting files
If the project brief explicitly mentions cross-cutting concerns spanning multiple sub-plans (e.g., port-parity ledger, migration tracker, API compatibility matrix), design those files too. Name them clearly and reference them in MASTER_PLAN.md §5. If you identify a need for a cross-cutting file that the user did not state, ask questions before designing it.

## Instructions
Completeness over brevity: specify every required file's deliverable in full — its exact path, purpose, required structure, constraints, and acceptance criteria. Describe what each file must contain and why; do not emit complete file bodies unless the user explicitly requires a byte- or wording-exact artifact.

---

If the project has more than 8–10 top-level tasks, split generation across multiple delegations to stay within output limits: delegation A emits MASTER_PLAN.md + master checklist + BLOCKER_REGISTER.md; subsequent delegations B..N emit sub-plan folders in batches, each re-including the §3 task index so task IDs stay consistent.
1. Understand the master plan structure before designing anything — refer to the format described in this delegation brief.
2. Analyze the project brief — identify natural top-level tasks, their dependencies, and a logical execution order.
3. Assign task IDs that communicate task type at a glance.
4. Customize EVERY section to the project. Do NOT copy project-specific rules from other plans unless the project genuinely requires them.
5. For each sub-plan, break the task into 5–15 ordered, concrete steps. Steps should hit the "one working session" granularity sweet spot.
6. If blocking questions exist, return ONLY the questions with no file content. Otherwise include files.
7. For each file, give its exact output path, purpose, required sections/headings/checklist/tables/relationships, project-specific facts/decisions/constraints/acceptance criteria, content to preserve from a named source path, and dependencies. Describe what the file must contain and why. Do not emit complete file bodies unless the user explicitly requires a byte- or wording-exact artifact.

Do NOT delegate to other subagents. Return the complete implementation design in your response. The build agent will author the files after user approval.

Do not manually word-wrap the generated markdown. Write each paragraph as a single continuous line; use newlines only to separate paragraphs, list items, headings, and fenced code blocks. Users can enable their editor's soft word-wrap.

---
```

---

### Phase 4: Independent Blueprint Assessment (Strategize / Plan)

If the Phase 3 planner response contains blocking questions, resolve them with the user FIRST, obtain a revised blueprint, THEN proceed to assessment.

Before presenting the design to the user, have an independent, read-only agent critique the blueprint so the final master plan is the best it can be.

1. Take the implementation design returned by the planning agent (the "design") from Phase 3.
2. Delegate an assessment to a **`strategize`** subagent, which performs deep, strategic review. **If the user's setup does not define a `strategize` agent, delegate to a `plan` subagent instead.** Both are read-only and must not modify files.
3. In the assessment delegation, give the assessor the implementation design (file paths, structure, decisions, dependencies, acceptance criteria) plus this skill's Master Plan Structure (§1–§10, checklists, blocker register, sub-plan fields from earlier in this skill). Ask it to evaluate and return a structured assessment covering:
    - **Structural completeness** — every MASTER_PLAN.md section (§1–§10) is present and genuinely customized to the project (no copy-pasted rules).
    - **Dependency DAG soundness** — ordering is correct, parallelism is realistic, and the critical path is clear.
    - **Task ID scheme** — IDs communicate task type at a glance.
    - **Sub-plan quality** — each has all required fields, a 5–15 step bottom-up breakdown, objective acceptance criteria, and real risks.
    - **Cross-cutting coverage** — concerns spanning multiple sub-plans are captured; missing cross-cutting files are flagged.
    - **Definition of Done** — concrete and verifiable.
    - **Strategic gaps / risks / better alternatives** the planner missed.
   Return it as three labelled sections: `Strengths`, `Gaps & Weaknesses` (prioritized), and `Recommended Changes` (concrete, actionable).
4. Route the assessment back to the **planning agent** with a revision brief: include the full assessment and instruct it to produce the improved implementation design (refined paths, structure, decisions, dependencies, acceptance criteria). The planner designs the revision; the assessor never edits.

Every revision delegation is stateless — re-include in the prompt: the full project brief, the Master Plan Structure spec (§3), the output directory, the current implementation design, AND the assessment/revision notes. Do not assume the agent remembers prior context.

5. Proceed to Phase 5 with the revised, assessed blueprint. One assessment pass is normally sufficient; repeat the assess → revise loop only if the revision introduced material new issues.

> Note: the assessor is strictly read-only. Do not ask it to write files —
> only the planning agent designs the implementation plan, and a build agent
> later authors the files (Phase 6).

### Phase 5: Present Plan for Review
When the planning agent returns its design:
1. Extract any questions the planning agent raised at the top of its response and present them to the user first.
2. Present a summary: number of sub-plans, dependency structure, key architectural decisions.
3. Note that the planning agent has produced an implementation-ready design identifying every file, its required structure, constraints, rationale, and acceptance criteria.
4. Ask: "Does this plan look right? Any modifications before I create the files?"
5. If the user requests changes, route them back to the planning agent with specific revision notes. Repeat until approved.

Every revision delegation is stateless — re-include in the prompt: the full project brief, the Master Plan Structure spec (§3), the output directory, the current implementation design, AND the assessment/revision notes. Do not assume the agent remembers prior context.

### Phase 6: Create Files
On user approval:
1. Delegate to a build agent.
2. Provide the approved implementation design (objective, rationale, exact paths, required structure, dependencies, constraints, source paths, acceptance criteria); instruct the build agent to inspect sources, author the structure/files, and verify.
3. Split into multiple build delegations only when dependency, scope, or context boundaries justify it.
4. Use the build agent for implementation steps.

### Phase 7: Report Completion
Tell the user:
- Path to the master plan folder
- Number of sub-plans generated
- The dependency DAG (which tasks can start immediately, which are gated)
- First step: "Agents should start by reading `MASTER_PLAN.md` in full."

## Edge Cases

**User provides no draft at all** — "I want to build a web app." Grill the user by asking targeted clarifying questions to extract scope, tech stack, deliverables, and success criteria before proceeding to Phase 3.

**Draft is extremely detailed** (50+ pages) — Summarize key points in the project brief. Note: "Full draft at [path] — planning agent, read this file for complete context."

**Project is tiny** (one task) — Still produce the full structure. MASTER_PLAN.md may have only §1, §3 (one row), §5, §10. Skip the DAG if there are no dependencies. The methodology scales down.

**User wants plan in an existing folder with other files** — Warn the user that MASTER_PLAN.md and other files will be created alongside existing content. Get explicit confirmation.