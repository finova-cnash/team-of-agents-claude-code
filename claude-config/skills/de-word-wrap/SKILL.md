---
name: de-word-wrap
description: "Remove manual hard line-wrapping from Markdown or plain-text files while preserving structural elements (fenced code blocks, front matter, headings, lists, tables, blockquotes, blank lines). Use when the user asks to unwrap, reflow, or de-word-wrap a file."
---

# De-word-wrap

Read the target text file and rewrite it to remove manual hard line-wrapping, merging broken prose lines into single physical lines per paragraph.

Perform the transform directly using your language capabilities  --  do NOT attempt to write code to modify the file.

### Detection — how to find wrapped lines (scan the WHOLE file first)

Do NOT assume a file is already unwrapped just because some lines are long.
Hard-wrapped prose often still looks "long." Scan every line to the end.

A line is **STRUCTURAL** (starts a new block) if it is:
- blank
- a fence delimiter (``` or ~~~) or a line inside protected front matter / fenced code / table
- a heading (`#`, `##`, …)
- a list item: starts with `-`, `*`, `+`, or `N.` (optionally indented)
- a blockquote line (starts with `>`)
- a table row (starts with `|`, or a `|---|` separator row)

A line is a **WRAP-CONTINUATION** if it is non-blank, non-structural, and
immediately follows (no blank line between) a prose line or a list item.
Such lines MUST be joined (single space) to the preceding line.

Special cases:
- **List-item continuation**: a list item often spans several physical lines;
  any following line that is indented and is NOT itself a new list item /
  heading / blockquote is a continuation of that item — merge it into the
  item's single line.
- **Indented paragraph**: a non-marker indented line following a paragraph
  (no blank between) is a continuation — merge.

### Reflow Rules

- **Unwrap Prose**: Merge multi-line prose paragraphs into a single line per paragraph, joining split lines with a single space.
- **Preserve Structure**: Keep each STRUCTURAL BLOCK on its own line — do not
  merge a list item into the preceding paragraph, or across a blank line /
  heading / `>` blockquote / table row. This does NOT mean keeping a list
  item's own wrapped continuation lines separate: merge those into the single
  item. (See §0 for detection.)
- **Preserve Indentation & Blank Lines**: Maintain existing paragraph spacing, blank lines, and semantic list/blockquote indentation.

### Verbatim Preservation

Never touch, join, or alter the following sections:

- YAML front matter (`---` blocks at the start of the file)
- Fenced code blocks (````` ```` ```` `` or `~~~` blocks)
- Tables (keep every row separate)
- Indented code blocks

### Verification (mandatory)

After rewriting, you MUST:
- Report the before/after line counts.
- Enumerate every merge you performed (original line range → resulting single
  line, or explicitly "none found").
- Confirm that no prose paragraph and no list item spans more than one
  physical line, and that protected sections (front matter, fenced code,
  tables, blockquotes) are byte-for-byte unchanged.

If you cannot enumerate merges, you did not actually scan — go back and do §0.