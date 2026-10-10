---
name: create-skill
description: "Guides the agent in creating a portable, standalone skill (SKILL.md) and, where the harness needs one, a command trigger. Use when the user asks to create, write, author, or package a new skill, add a new SKILL.md, or turn a workflow/prompt into a reusable skill."
---

# Create Skill

Create a new skill directory containing a valid `SKILL.md` and, only if the target harness requires it, wire an invocation entry into that harness's config.

### The Standalone Rule
Skills should be self-contained. Avoid unnecessary dependencies or cross-references to other skills unless the user explicitly requests modular chaining. Inline core instructions directly into `SKILL.md`.

### Frontmatter
- **Directory Structure**: Create a directory matching the skill name containing `SKILL.md` (e.g., `<skill-name>/SKILL.md`).
- **Naming Constraints**: Lowercase, alphanumeric, hyphenated strings only (regex: `^[a-z0-9]+(-[a-z0-9]+)*$`). The frontmatter `name` must match the parent directory name exactly. Max 64 characters.
- **Required fields**: only `name` and `description`. Everything else in frontmatter is optional.
- **Optional fields**: `allowed-tools` restricts which tools the skill may invoke when loaded; add it only if the skill should be limited to a subset of the available toolset. Other valid optional fields include `model` and `disable-model-invocation`.
- **Description Field**:
  - Third person (e.g., "Extracts data and generates reports" — avoid "I can help you").
  - States *what* the skill does and includes explicit trigger words/phrases for *when* an agent should load it.
  - Under 1,024 characters.

### Progressive Disclosure
Keep the core `SKILL.md` lean:
- Aim to keep `SKILL.md` under 500 lines.
- Move heavy assets, large reference docs, or API specs into a `references/` subdirectory.
- Put executable helper tools or automation logic into a `scripts/` subdirectory.
- Place static boilerplate or file templates into an `assets/` subdirectory.

### Target Path Resolution
- **Project-Scoped**: Look for existing local skill folders (e.g., `.agents/skills/`, `.claude/skills/`, or `.opencode/skills/`).
- **Global / User-Scoped**: Default to user-level config directories (e.g., `~/.agents/skills/`, `~/.claude/skills/`, or `~/.config/opencode/skills/`).

### Optional Command & Harness Integration
Command integration depends on how the target agent harness discovers skills:

- **Auto-Discovery Environments:** Many modern agents (in the harnesses, custom project-level skill directories, etc.) automatically index skill files located in standard paths (e.g., `.agents/skills/` or `~/.agents/skills/`). No configuration file edits are required for these environments.
- **Explicit-Mapping Environments:** Some harnesses (e.g., OpenCode) require explicit registration via a central manifest (such as `opencode.json`).
- **Action:** Only edit configuration files if the target harness explicitly requires manual routing or registration. When updating a manifest, preserve existing structure and maintain valid formatting/syntax.