---
name: refine-skill
description: "Audits and refactors an existing SKILL.md for bloat, weak triggers, and missing progressive disclosure, then rewrites it in place. Use when the user asks to refine, improve, tighten, clean up, or review an existing skill, or reports that a skill isn't being loaded when it should be."
---

# Refine Skill

Take an existing `<skill-name>/SKILL.md` and rewrite it to be leaner, more reliably triggered, and better structured — without changing what the skill is for. This is an edit pass, not a rewrite-from-scratch: preserve the skill's actual behavior and any project-specific facts it encodes.

### Locate and Read First
Find the target skill's directory. Read the full `SKILL.md` plus anything in its `references/`, `scripts/`, or `assets/` subdirectories before touching it — you need the whole picture, not just the frontmatter.

### Audit Checklist
Apply each check; fix in place as you go rather than producing a separate report unless the user asked for a review only.

- **Frontmatter correctness**: `name` matches the directory exactly, lowercase-hyphenated, ≤64 chars. `description` is third person, states *what* it does and *when* to load it (explicit trigger words/phrases a router would match on), ≤1024 chars.
- **Trigger strength**: Would an agent scanning only the description know to load this for the phrasing users actually use? Weak/generic descriptions ("helps with skills") get rewritten with concrete trigger words pulled from real invocation phrasing.
- **Length and bloat**: If the body exceeds ~500 lines, or contains material only needed occasionally, move it out — reference docs to `references/`, executable helpers to `scripts/`, boilerplate/templates to `assets/`. The core file should read like a checklist an agent executes, not a document a human studies.
- **Rules vs. judgment**: Replace hard, enumerated constraints with goal-oriented criteria where a capable model would make the right call anyway (see the rule of thumb below). Keep hard constraints only for non-inferable thresholds, project-specific gotchas, or things that are cheap to state and expensive to get wrong.
- **Redundancy**: Delete restated general knowledge, persona theater ("You are an elite..."), emphasis scaffolding (ALL CAPS, `CRITICAL:`, excessive emoji headers), and any instruction duplicated elsewhere in the same file or across its `references/`.
- **Standalone-ness**: Unless the user wants modular chaining, the skill should work without requiring another skill to already be loaded. Cross-references to other skills are fine as pointers, not as load-bearing dependencies.
- **Examples**: Prefer a few sharp examples or a clean parameter/interface shape over long few-shot blocks. Cut examples that just restate the rule in different words.

### Rule of Thumb
If a strong model would make the right call without a line of prompt text, delete that line. Every remaining line should be doing one of: encoding a fact the model can't infer, setting a hard threshold, or fixing a documented failure mode.

### Preserve, Don't Regress
Never drop project-specific facts, file paths, environment quirks, or hard-won constraints (e.g. "always use `references/`, never inline API specs") while trimming prose around them. When unsure whether a line is load-bearing, keep it and tighten the wording instead of deleting it.

### Report the Diff
After rewriting, summarize what changed in a few lines: what was cut, what moved to subdirectories, what the description now says, and any judgment call the user should sanity-check. Don't reproduce the whole file in chat unless asked.
