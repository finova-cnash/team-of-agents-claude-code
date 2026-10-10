---
name: high-level-plan
description: "Produce a concise, minimal, high-level plan the user can read, verify, and amend before further review. Use when the user asks to `sketch a plan`, `outline a plan`, `rough plan`, `high-level plan`, `high level plan`, or wants a short plan they can review and edit rather than a detailed implementation plan."
---

# High-Level Plan

Produce a short agreement artifact: the shape of the work and why, not the detailed steps.

### What a High-Level Plan Is

Cover, in a few short headed sections:

1. **Goal** — what outcome is being pursued and why.
2. **Approach** — the shape of the solution and the key decisions behind it, with rationale.
3. **Constraints** — what shapes or limits the approach (existing systems, conventions, non-negotiables).

No code, no file-by-file task lists, no line-level detail — that belongs in a detailed plan later (see below).

### Minimal but Sufficient

Short enough to read in one pass. Prefer prose and short sections over deep nesting or long bullet chains. Omit anything the user already knows or stated themselves.

### Built for Verification and Amendment

This is a draft, not a spec. Surface assumptions explicitly rather than folding them into the prose as fact. Call out decisions the user should confirm rather than silently deciding them. End with 1-3 explicit open questions or decision points aimed at the user. Include a short **Deliberately out of scope** section listing what was excluded, so the user can see it and push back.

### Ground It in Verified Facts

Never invent file paths, config contents, tool behaviour, or project state. Investigate the workspace first where feasible. If something can't be verified, say so plainly rather than presenting it as confirmed. Do not treat an untested assumption as an established fact.

### What Happens Next

State plainly that once the user has reviewed and agreed the high-level plan, the work may proceed to a detailed master plan or straight to implementation — but only at the user's explicit discretion. Do not proceed to a detailed plan or implementation without that agreement.

### Execution & Directory Selection

1. Inspect the workspace root for an existing documents folder (for example, `docs`, `doc`, `documents`, `documentation`, `notes`, `plans`, etc.).
   - Reuse the best existing candidate folder.
   - If no suitable folder exists, create `docs/`.
2. Write the plan as `High-level plan - <topic>.md` inside that directory, where `<topic>` is a short descriptive name for the work.
3. Report the full path to the user. Note that they may edit the file directly, and that the edited file — not the chat transcript — is the source of truth for any follow-up work.
