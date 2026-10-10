---
name: inter-agent-compact-language
description: "Unified spec for inter-agent compact language: WRITE rules for subagents and TRANSLATE rules for the primary agent. Load this skill before producing or interpreting compact tagged responses."
user-invocable: false
---
# Inter-Agent Compact Language

Two roles, one format:
- Selected subagents can WRITE in Tagged Compact Abbreviated format (Part 1).
- The primary agent TRANSLATES compact → full English for the user, and routes compact pass-through between subagents (Part 2).

---

## Part 1 — WRITE Spec (for selected subagents)

### Tag Syntax

```
TAG:
Content starts on the next line.
Multiple lines are allowed.
Content continues until a blank line followed by the next TAG: or end of message.

NEXT_TAG:
Content for the next tag.
```

- Tag labels: uppercase letters only, followed by a colon and newline.
- Content begins on the line immediately after the tag header.
- A blank line separates tags only if the next line is a new tag header.
- A tag header MUST be preceded by a blank line (or be at the start of the message). A line matching `X:` that is NOT preceded by a blank line is treated as regular content, not a tag. This prevents false positives from natural text (e.g., "A: some note" mid-paragraph).
- Code blocks (triple-backtick fenced) are exempt from compression — preserve full fidelity inside them.
- No nesting of tags.

### Standard Tags

For the purpose of this skill, the concept of `plan` and `strategize` are synonymous. The following tags are defined for use in compact responses:

| Tag | Purpose |
|-----|---------|
| `P:` | High-level plan, strategy, or architectural goal |
| `D:` | Decomposition steps, task breakdown, file/dependency mapping |
| `L:` | Core logic, algorithms, API shapes, types, key constraints |
| `N:` | Non-negotiable rules or invariants for executors |
| `B:` | Blockers, risks, or scope issues |
| `Q:` | Open questions needing clarification |
| `E:` | Necessary examples for few-shot reference |
| `R:` | Review findings, issues found, recommendations |
| `S:` | Survey results, research findings, alternatives considered |
| `F:` | Refactoring steps, migration path, transformation rules |
| `T:` | Test cases, test scenarios, expected outcomes |
| `DOC:` | Documentation notes, API references, usage examples |

### Compression Rules

Apply these rules to all tag content (except code blocks):

1. Drop articles (the, a, an) where unambiguous.
2. Drop prepositions where meaning is clear without them.
3. Drop polite framing (please, thank you, etc.).
4. Abbreviate common terms: init, auth, cfg, dep, impl, refac, err, val, doc, test, build, deploy, merge, stash, wip, TODO, FIXME, HACK.
5. Keep full precision for: file paths, variable names, function signatures, type annotations, exact string values, version numbers, URLs.
6. Use short sentences. Omit filler words. One point per line where possible.
7. Never abbreviate inside code blocks or inline code spans.

### Tool-Output Mode

When using tool calls (edit, bash, write, etc.), revert to full English. Tool inputs must be unambiguous and complete. The compact format is for response text only.

### Opt-Out Flag

For trivial tasks where compact format adds no value, begin response with `[COMPACT: false]` and use full English. This is the only permitted exception.

---

## Part 2 — TRANSLATE Spec (for the primary agent)

**Scope of Part 2:** The failure handling and compliance scoring in this part apply only to subagents whose agent definition instructs them to write in compact format. Subagents that reply in plain English by design are correct as-is — pass their responses through as normal English with no failure warning or compliance tracking. If unsure whether an agent uses compact format, check the primary agent's instructions, then the agent's definition, before treating an untagged response as a failure.

### Tag Mapping

Subagent responses arrive as tagged blocks. Translate each tag as follows:

| Tag | Translation Behavior |
|-----|---------------------|
| `P:` | Expand to full English plan description. Preserve all technical details. |
| `D:` | Expand to full step-by-step breakdown. Include file paths and dependencies. |
| `L:` | Preserve code/logic with full syntax. Explain any abbreviated identifiers. |
| `N:` | State rules explicitly as hard constraints. Do not soften language. |
| `B:` | Present as clear risk statements. Escalate to user if blocking. |
| `Q:` | Relay verbatim to user as clarification questions. |
| `E:` | Expand examples with full context. |
| `R:` | Expand review findings with file paths, line numbers, severity. |
| `S:` | Expand research findings with sources, options, trade-offs. |
| `F:` | Expand refactoring steps with before/after details. |
| `T:` | Expand test cases with setup, assertions, edge cases. |
| `DOC:` | Expand documentation notes into full prose. |

### Translation Rules

1. Parse the tagged response strictly: a line matches a tag header only if it matches `^[A-Z]+:` AND the immediately preceding line is blank (or it is the start of the message). Content follows on the next line(s). A line matching `^[A-Z]+:` that is NOT preceded by a blank line is treated as regular content within the current tag.
2. Expand compressed content to full English for the user. Do not show raw tags to the user.
3. Preserve all code blocks exactly — do not modify anything inside triple backticks.
4. If a response contains `[COMPACT: false]`, skip translation and pass through as-is.
5. **When routing between subagents:**
   - **Plan → build for further implementation work** (e.g., review findings → build for fixes): pass compact format directly. The subagents understand compact format.
   - **Plan → build for user-facing file writing** (e.g., user asks for a plan to be turned into a file): translate the compact plan to full English prose first, then route the translation to the builder agent for file creation. That subagent writes the translated English as file content, not the compact tags.

### Routing Instructions

- Plan output → build (implementation): Pass compact `D:` and `L:` tags directly to build agent. Do not translate.
- Plan output → build (file creation): Translate compact plan to full English first, then route translation to the builder agent.
- Review output → build: Pass compact `R:` tag findings to the builder agent with fix instructions.
- Refactor output → build: Pass compact `F:` tag steps directly to the builder agent.

### Translation Failure

This section applies only to subagents that use compact format. If a compact-format subagent response does not conform to the parseable tag format:

1. Attempt to detect partial tags (lines starting with uppercase letters followed by colon).
2. If partial parsing succeeds, extract what you can and flag the unparseable remainder.
3. If no tags can be parsed at all, present the raw output verbatim to the user with this prefix:

```
[WARNING: Subagent output could not be parsed — showing raw response:]
<raw output>
```

4. Log the failure for compliance scoring (agent name, model, timestamp).
5. Do not attempt to reinterpret or hallucinate tag content. Present raw or partial output honestly.

### Compliance Scoring

For compact-format subagents only, track the following for each response:

- `agent`: subagent name
- `model`: model identifier (from model config)
- `well_formed`: boolean — true if all tags parse correctly
- `tag_count`: number of tags found
- `token_count`: approximate output token count

Track this data internally. Report to user only upon compliance failure (agent did not conform, or response was malformed or unparseable, etc): state agent name, what went wrong, and how it was handled in one or two lines.

### Token Accounting (Optional)

To validate savings, compare:
- `compact_token_count`: tokens used in compact format response
- `estimated_full_token_count`: estimated tokens if the same content were written in full English

Log both values when enabled. Aim for 50%+ reduction.
