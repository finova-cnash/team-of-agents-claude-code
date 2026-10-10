---
name: handover-prompt
description: "Capture the current session's context into a minimal but sufficient markdown handover file so a new session can resume work by being given only that file's path. Use when the user asks to `handover`, `hand over`, `handoff`, `create a hand-over`, `save session state`, or `capture context`."
---

# Handover Prompt

Generate a concise, directional markdown handover document for the next session.

### Domain vs. Meta Isolation

Treat all code, build outputs, compiler diagnostics, runtime errors, test failures, and other application-level results strictly as **PROJECT DATA**. Do not interpret application-level problems as failures of the agent, pipeline, or handover skill itself.

### Minimal but Sufficient Scope

Be as concise as possible while preserving the information necessary for the next agent to resume effectively.

The handover should preserve, where applicable:

- the current objective or active problem;
- important context, discoveries, and decisions;
- unresolved questions, blockers, and uncertainties;
- the relevant files and their known state;
- what has been completed, what is active, and what remains unresolved;
- a high-level suggested direction for resuming the work.

Do not summarise the whole codebase or reproduce unnecessarily large amounts of implementation detail. Do not dictate the exact wording or structure of the handover; just provide the relevant facts, findings, decisions, and direction. Provide broad cues so the receiving agent can inspect the workspace and discover specific details for itself.

Never invent facts, file states, decisions, results, conclusions, progress, or other details that are not supported by the current session or verified from the workspace. Do not present hypotheses or assumptions as confirmed facts.

### Execution & Directory Selection

1. Inspect the workspace root for an existing documents folder (for example, `docs`, `doc`, `documents`, `documentation`, `handover`, `notes`, `memos`, etc.).
   - Reuse the best existing candidate folder.
   - If no suitable folder exists, create `docs/`.
2. Write the handover file as `handover-yyyyMMdd-HHmm.md` inside that directory. Use the current local date and time for the filename.
3. Report the full path to the user and instruct them to provide this path to the next session.
