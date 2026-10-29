---
name: master-plan
description: "Creates a project master plan folder from draft requirements: a MASTER_PLAN.md navigational hub, a master completion checklist, a BLOCKER_REGISTER.md, and one sub-plan folder per top-level task, each with its own SUB_PLAN.md and checklist. Use when the user asks for a master plan, project plan, build plan, or multi-phase plan, asks to break a project into sub-plans, or wants to rebuild or recreate a plan from an older plan or high-level plan."
---

# master-plan

## Goal
Turn the user's draft requirements into a complete master plan folder, designed by a planning agent, independently assessed, approved by the user, then authored by a build agent. The full specification of the output is in `references/plan-spec.md`.

## Protocol

Read `references/plan-spec.md` (relative to this skill's base directory) in full, and paste its full contents into every planning, assessment, and build delegation. Never summarise it or pass only its path.

### Phase 1: Gather inputs
Execute this skill as the main/primary orchestrator/manage agent; subagents cannot ask the user, so all clarification happens here.

Ask the user two questions:
1. "Where is your draft requirements document, or would you like to describe the project now?"
2. "Where should I create the master plan folder?" (Default: a new folder in the current workspace.)

Read the draft. If inline text, capture it verbatim. Note project name, technologies, any pre-identified sub-tasks.

### Phase 2: Assess clarity
The draft is ready for planning if it answers at least:
- **What** is being built (concrete deliverables)
- **Scope** boundaries (what is in and out)
- **Done when** (success criteria — tests pass, deployment, milestone, etc.)

If any are missing, ask 2–4 focused clarifying questions (a short interrogation) rather than a long clarifying session.

### Phase 3: Delegate to a planning agent
Delegate using this template, replacing bracketed sections.

```
Design a complete master plan for the project below. Follow the specification below for every file's structure, the plan content rules, the design requirements, and the formatting rules.

## Project Brief
[USER'S DRAFT REQUIREMENTS — VERBATIM OR FAITHFULLY SUMMARIZED]

## Output Directory
[PATH — e.g., /home/user/projects/myproject/MyPlan/]

## Specification
[CONTENTS OF references/plan-spec.md, VERBATIM]

If blocking questions exist, return only the questions, with no file content. Do not delegate to other subagents. Return the complete implementation design in your response for user approval; a build agent will author the files after approval.
```

If the project has more than 8–10 top-level tasks, split generation across multiple delegations to stay within output limits: delegation A emits MASTER_PLAN.md + master checklist + BLOCKER_REGISTER.md; subsequent delegations B..N emit sub-plan folders in batches, each re-including the §3 task index so task IDs stay consistent.

Every revision delegation (Phase 4 and Phase 5) is stateless and must re-include: the project brief, the specification, the output directory, the current implementation design, and the assessment or revision notes. Do not assume the agent remembers prior context.

### Phase 4: Independent assessment
If the planner returned blocking questions, resolve them with the user first and obtain a revised design before assessing.

Before presenting the design to the user, have an independent, read-only agent critique it:

1. Delegate to a `strategize` subagent. If no `strategize` agent is defined, delegate to a `plan` subagent. Both are read-only and must not modify files; only the planning agent designs and only a build agent later authors files.
2. Give the assessor the implementation design (file paths, structure, decisions, dependencies, acceptance criteria) plus the specification, and the paths of any source documents (draft, older plans) so it can check carry-over. Ask it to evaluate:
   - Conformance to every structural requirement and plan content rule in the specification (self-contained, Rules A, B, C), including that every MASTER_PLAN.md section is present (subject to the specification's Scaling down allowance) and genuinely customised, including a task-ID scheme that communicates task type at a glance
   - Dependency DAG soundness: correct ordering, realistic parallelism, clear critical path
   - Sub-plan quality: all required fields, 5–15 step bottom-up breakdown, objective acceptance criteria, real risks
   - Cross-cutting coverage: concerns spanning multiple sub-plans are captured; missing cross-cutting files are flagged
   - Definition of Done: concrete and verifiable
   - Strategic gaps, risks, or better alternatives the planner missed
3. Require the assessment as three labelled sections: `Strengths`, `Gaps & Weaknesses` (prioritised), and `Recommended Changes` (concrete, actionable).
4. Route the full assessment back to the planning agent with instructions to produce the improved design (refined paths, structure, decisions, dependencies, acceptance criteria).
5. Proceed to Phase 5 with the revised design. One assessment pass is normally sufficient; repeat the assess → revise loop only if the revision introduced material new issues.

### Phase 5: Present for review
1. Extract any questions the planning agent raised at the top of its response and present them to the user first.
2. Present a summary: number of sub-plans, dependency structure, key architectural decisions.
3. Note that the planning agent has produced an implementation-ready design identifying every file, its required structure, constraints, rationale, and acceptance criteria.
4. Ask: "Does this plan look right? Any modifications before I create the files?"
5. If the user requests changes, route them back to the planning agent with specific revision notes. Repeat until approved.

### Phase 6: Create files
On user approval, delegate to a build agent with the approved implementation design (objective, rationale, exact paths, required structure, dependencies, constraints, source paths, acceptance criteria) and the specification. Split into multiple build delegations only when dependency, scope, or context boundaries justify it.

Instruct the build agent to inspect sources, author the files, then verify every generated file against the specification's plan content rules (self-contained, Rules A, B, C) and the formatting requirements, and fix any violations.

### Phase 7: Report completion
Tell the user:
- Path to the master plan folder
- Number of sub-plans generated
- The dependency DAG (which tasks can start immediately, which are gated)
- First step: "Agents should start by reading `MASTER_PLAN.md` in full."

## Edge cases

**User provides no draft at all** (e.g., "I want to build a web app.") — Ask targeted clarifying questions to extract scope, tech stack, deliverables, and success criteria before proceeding to Phase 3.

**Draft is extremely detailed** (50+ pages) — Summarize key points in the project brief. Note: "Full draft at [path] — planning agent, read this file for complete context." The planner may read the full draft at that path, but the generated plan must not depend on the path; copy needed content into the plan files.

**Plan is rebuilt from an older plan, high-level plan, or similar** — Read the source(s) in full, carry across everything still relevant as the specification's Self-contained rule and BLOCKER_REGISTER.md section require, and assume the sources will be deleted. The new plan must stand alone.

**Project is tiny** (one task) — Follow the specification's Scaling down section.

**User wants the plan in an existing folder with other files** — Warn the user that MASTER_PLAN.md and other files will be created alongside existing content. Get explicit confirmation.
