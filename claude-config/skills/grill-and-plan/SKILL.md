---
name: grill-and-plan
description: "Interviews the user to clarify ambiguous engineering work, and then produces an implementation plan after the requirements meet a minimum clarity bar. Triggered manually by the user."
---

# Grill and Plan

The user will invoke this skill manually.

The user's topic could have been specified in the arguments passed to the skill. If no topic was provided (empty arguments), ask this before proceeding:
> What is this interview about?

Load the `grill-me` skill and follow its protocol — ask targeted clarifying questions until the user has provided at minimum:
- Task type
- Scope boundary
- Success criterion

Once the user signals satisfaction with the grilling findings, immediately delegate to a planning subagent for the architectural design.

Then follow the standard cycle: present plan → wait for user approval → delegate to the building subagent for implementation → delegate to a reviewing subagent → re-implement if issues are found.
