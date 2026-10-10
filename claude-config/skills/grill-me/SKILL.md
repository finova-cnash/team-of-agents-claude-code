---
name: grill-me
description: "Activates when the ask is vague or ambiguous — for asking concise, targeted, clarifying questions to extract concrete requirements before work begins."
---

# Grilling — clarify before delegating

## Goal
Prevent wasted work by forcing clarity before any delegation. When the user's prompt lacks specificity — no clear scope, no success criteria, no constraints, ambiguous goals — the primary agent loads this skill and interrogates the user with short, direct questions until the requirements are concrete enough to delegate to plan/build.

## When to Activate
Trigger when the user prompt exhibits any of:
- No specific task (e.g., "make it better", "fix this", "improve things")
- No scope boundaries (unclear what's in/out)
- No success criteria (what does "done" look like?)
- No constraints (platform, language, dependencies, budget, timeline)
- The prompt can be interpreted 3+ valid ways
- Vague emotional language like "it feels off" or "it's not right"

Do NOT activate when:
- The user gives a specific technical instruction (e.g., "rename getCwd to getCurrentWorkingDirectory in all JS files")
- The user references specific files, functions, or patterns
- The user is asking a straightforward informational question

## Grill Protocol
When activated, the primary agent must NOT delegate to any subagent. Instead:

1. **Acknowledge the vagueness** — one sentence: "I need a few specifics before I proceed."
2. **Fire the grilling questions** — 3-5 short, specific questions delivered as a block. Each must be answerable in one sentence or a single-word choice. Never open-ended "what would you like?" questions. Examples of good grilling:
   - "Is this a bug fix, new feature, refactor, or investigation?"
   - "Which files or components are affected?"
   - "What's the success criterion? 'No error on startup' vs 'all unit tests pass' vs 'the output matches this sample'."
   - "Any constraints? (language, framework, platform, deadline)"
   - "Is this for a PR, local dev, or production?"
3. **Wait for answers** — do not proceed until the user responds.
4. **If still vague after one round**, fire a second round of 2-3 deeper questions. Do NOT give up and make assumptions.

## Anti-patterns
- Do NOT ask "What do you mean?" — that's lazy and puts the burden back on the user.
- Do NOT ask "Can you be more specific?" — be specific in what you're asking about.
- Do NOT ask more than 5 questions at once — overwhelming.
- Do NOT proceed to plan/build without at minimum: task type, scope boundary, and success criterion.
- Do NOT fill in missing details yourself — just because you *can* infer something doesn't mean you should.

## Exit condition
The grill is complete when you have answers for at least 3 axes:
1. **What** (task type, affected area)
2. **How far** (scope boundary — what's in, what's out)
3. **Done when** (success criterion)

Once satisfied, proceed to normal delegation (plan → approval → build → review).

## Example

User: "I want to add authentication."

Agent (grill-me loaded):
"Three questions before we start:
1. What kind of auth — session, JWT, OAuth, or API keys?
2. Is this for new code, or layering on top of existing user accounts?
3. What's 'done' — prototype, full integration with login page, or just the backend middleware?"

User: "JWT, new code, full integration with login."

Agent: "Got it. Delegating to the planning subagent." — and proceed normally.
