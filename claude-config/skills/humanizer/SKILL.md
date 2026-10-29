---
name: humanizer
description: Identifies AI writing patterns and helps revise text into a natural, specific human voice. Use when the user asks to humanize writing, remove AI-like phrasing, or critique text for signs of generated language.
---

## Workflow

1. Review the submitted text for formulaic patterns using the checklist below. Use the [28-pattern guide and examples](references/structural-patterns.md) for the complete taxonomy across content, language, style, and performed authenticity. Consult the [vocabulary and screening reference](references/ai-vocabulary.md) for contextual word choices and optional screening prompts.
2. For critique, name the patterns, quote the affected passages, and suggest fixes with brief reasoning. For editing, return the revised text and explain material changes when useful.
3. Strengthen voice using the editing instructions below while preserving meaning, factual claims, technical terminology, audience, and established style.
4. Compare the original and revised text before returning it: check facts, certainty, emphasis, substantive omissions, technical terminology, and voice. Restore unintended changes; put unresolved sourcing questions and editorial notes outside the revised prose. Final sweep before returning: scan the revised text outside the exceptions for any remaining digit-dash-digit sequence or number followed by an abbreviated unit, and fix each per the number formatting rules.

Patterns are stylistic prompts, not reliable proof of AI authorship. Use judgment rather than checkbox compliance: evaluate clusters in context; intentional choices need not be removed (except under the number formatting rules). This skill requires no external tools, APIs, or dependencies.

Flag unsupported claims or ask for sources; do not fabricate studies, numbers, quotations, personal experiences, or opinions to make a passage sound specific. Do not replace a real range with an invented midpoint. First-person perspective and uncertainty must reflect the author's supplied perspective, not a persona you create.

## Detection Checklist

Use these as contextual review questions, not bans. The number formatting rules below are the exception. Keep legitimate rhetorical questions, balanced contrasts, brand statements, lists, and technical terms when they serve the audience and author's purpose.

**Content level:**
- [ ] Check sourcing behind vague attributions ("research suggests", "many people", "experts agree")
- [ ] Use supplied concrete examples where useful; general principles can stand on their own
- [ ] Preserve the author's perspective or balanced argument; question false balance
- [ ] Review formulaic future closings for substantive purpose and supported certainty

**Language level:**
- [ ] Prefer direct words where equally precise; preserve technical distinctions
- [ ] Use sentence construction that suits the format; fragments can work in labels
- [ ] Review artificial three-item groupings; retain legitimate items
- [ ] Consistent vocabulary for repeated concepts
- [ ] Vary sentence length and structure where it improves readability
- [ ] Review redundant qualifiers using the vocabulary reference; retain necessary uncertainty

**Style level:**
- [ ] Review repeated em dashes for disruptive rhythm, without quotas
- [ ] Use boldface for meaningful emphasis or navigation
- [ ] Keep "**First:** explanation" lists when they aid scanning or sequence
- [ ] Headings in sentence case (unless title case is established)
- [ ] Review 🚀 ✨ 💡 emoji filler; retain purposeful symbols
- [ ] Use quote style appropriate to code, publication, and established conventions
- [ ] Choose lists or prose for readability; preserve steps and distinct items

**Authenticity level:**
- [ ] Keep rhetorical questions that genuinely frame an open issue
- [ ] Preserve relevant complexity without manufacturing uncertainty or imbalance
- [ ] Keep purposeful "We believe" or brand statements; flag unsupported promises
- [ ] Preserve authentic personality; review forced parenthetical asides
- [ ] Consistent voice throughout
- [ ] No sudden code-switching to slang

## Number formatting (always apply)

These are mandatory edits, not contextual review questions. Apply them in both editing and critique modes (in critique, quote each instance and give the fix).

1. Outside the exceptions (rule 6), rewrite every numeric range written with a hyphen, en dash, or em dash into words. Use "or" for small adjacent whole-number counts of discrete things; use "to" otherwise. "3–4 times" → "3 or 4 times"; "9.6–10.5 s" → "9.6 to 10.5 seconds"; "13–15" → "13 to 15"; "24–66" → "24 to 66". Never change the values.
2. Keep suffixes on both ends: "13–14k" → "13k to 14k".
3. Convert date ranges too, including in headings, using ordinal day numbers and full month names in the author's day/month order: "21–23 Oct" → "21st to 23rd October"; "30 Sep–2 Oct" → "30th September to 2nd October"; "Oct 21–23" → "October 21st to 23rd". Ordinal suffixes: 1st, 2nd, 3rd, 11th to 13th (all "th"), 21st, 22nd, 23rd, 31st.
4. Spell out abbreviated units after numbers everywhere (prose, bullets, headings), not only in ranges: "12.6 s" → "12.6 seconds"; "117 ms" → "117 milliseconds"; min → minutes; hr/h → hours; yrs → years. Use singular for exactly 1 ("1 second"). Leave domain metric labels such as "p50" and "p95" as written.
5. Judge context: convert only when the dash means a range. Leave arithmetic or formulas ("13–14k=-1k" is subtraction, unchanged), negative numbers, identifiers, versions, phone or ID numbers, and compounds such as "COVID-19".
6. Exceptions, left untouched: fenced code blocks, inline code spans, tables, and citations or page ranges. Performance figures in prose are not exceptions.

## Editing Instructions

**For each pattern found:**

1. **Identify** the specific type (from the 28)
2. **Explain** its stylistic effect and why it reads as inauthentic in context, without claiming authorship
3. **Suggest** revision with reasoning
4. **Show** before/after if helpful

**Voice, where warranted:**

Do not add personality, uncertainty, informality, or imbalance unless it benefits the passage and is supported by the author's supplied voice and perspective. Consider:
- Expressing supplied opinions or findings clearly
- Retaining the author's actual uncertainty
- Clarifying supplied stakes: what matters to the author and why
- Using supplied specifics instead of abstractions
- Varying sentence length and rhythm where useful
- Keeping first-person perspective where authentic
- Preserving personality through word choice without forcing it
- Allowing contractions, fragments, and informal phrasing when appropriate to the author's voice; avoid overcorrecting into performed casualness

## Example Workflow

**User submits:** "The rise of AI is fundamentally reshaping the workplace. Research suggests that remote work increases productivity by up to 40%. Experts agree that flexibility is crucial. As we evolve our systems, we can expect unprecedented transformation."

**Your response:**

Pattern 1 — "Inflated significance": "Fundamentally reshaping" is broad; state the specific workplace change if the author can substantiate it.

Pattern 5 — "Vague attribution": "Research suggests" and "Experts agree" lack sources. Flag the productivity figure and consensus claim for verification rather than replacing them with a different study or statistic.

Pattern 6 — "Formulaic future": "As we evolve... unprecedented transformation" is template language.

**Retained pending verification:** "The rise of AI is fundamentally reshaping the workplace. Research suggests that remote work increases productivity by up to 40%. Experts agree that flexibility is crucial. As we evolve our systems, we can expect unprecedented transformation."

**Editorial note:** Verify the scale of the workplace change, productivity figure, consensus claim, and predicted transformation before publication. Ask for supported specifics before rewriting these claims; weakening them or deleting the closing would change substance or emphasis. Add an author's viewpoint only when supplied.
