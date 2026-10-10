---
name: refine-system-prompt
description: "Rules and guidance for refactoring system prompts, agent/persona instructions, and sub-agent definitions — trimming toward subtraction, progressive disclosure, and delegated judgment. Use when the user asks to refine, tighten, shorten, or refactor a system prompt, agent instructions, or a sub-agent definition, or when one has grown bloated, repetitive, or too long."
---

# Refine System Prompt

## Core Philosophy
Modern LLMs have strong contextual reasoning, implicit intent detection, and precise tool comprehension. Older context-engineering habits — prompt bloat, rigid rules, negative constraints, explicit verification loops — now degrade performance and waste token budget.

**Rule of Thumb:** If a strong model would make the right call without a line of prompt context, delete that line.

---

## Structural Shifts

| Shift | Old Pattern | New Pattern |
| :--- | :--- | :--- |
| **1. Rules → Judgment** | Hard constraints (`"NEVER write multi-line comments"`) | Goal-oriented criteria (`"Match surrounding comment density and idiom"`) |
| **2. Examples → Interfaces** | Long input/output few-shot example blocks | Expressive parameter names, typed Enums, and clean API signatures |
| **3. Upfront → Progressive** | Stuffing all specs into the system prompt | Tree-structured rules, dynamic skills, and just-in-time context loading |
| **4. Repetition → Single Source** | Duplicating constraints in prompts AND tools | Precise descriptions attached only to the tool/file where used |
| **5. Manual Logs → Auto Memory** | Hand-rolled conversation transcripts/logs duplicating what the harness already maintains | Native auto-summarization / agentic memory. Does not apply to deliberately authored persistent memory files (project docs, config, system prompt) — those stay manual by design |
| **6. Simple Specs → Rich References** | Generic instructions without local context | Reference existing canonical files (`"follow @src/app/api/users"` pattern) |

---

## What to Retain vs. What to Delete

### Keep in Context Window (High Value)
- **Operator Preferences:** Non-inferable preferences (e.g., preference for brevity vs. detail in PRs).
- **Project Gotchas:** Non-obvious build commands, environment traps, legacy edge cases.
- **Hard Decision Thresholds:** Quantitative boundaries that cannot be inferred by "vibe" alone.
- **Integrations & Names:** Specific service identifiers, endpoint paths, and environment variables.

### Delete Immediately (Low Value / Counterproductive)
- **Persona Theater:** *"You are an elite principal engineer with 15+ years of experience..."*
- **Restated General Knowledge:** Explaining basic language syntax or framework fundamentals.
- **Emphasis Scaffolding:** `CRITICAL: THINK HARD`, `IMPORTANT!!!`, emoji section headers.
- **Explicit Verification Demands:** Demanding explicit self-validation cycles on basic low-risk tasks.

---

## Execution Checklist for System Prompts

1. **Audit Length:** If the prompt exceeds **50 lines**, split specialized instructions into dedicated `.md` skill files.
2. **Remove Verification Nudges:** Cut automated instructions telling the model to "double-check every output." Delegate verification *only* to deterministic linters/tests or declared high-risk boundaries (e.g., data mutations, production deployments).
3. **Refactor Tool Definitions:** Strip inline few-shot examples from tool parameters. Rename fields to be self-descriptive (e.g., use `target_environment: "staging" | "production"` instead of a plain string with an example comment).
4. **Deduplicate:** Ensure no rule exists in both the system prompt and individual agent/tool definitions.
