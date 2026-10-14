# Repository Instructions

This repository contains my own coding agent skills. Follow the reference
repository's format, without copying its catalog or instructions verbatim.

## Writing

- Before writing a skill, establish its target task, triggers, and expected
  outcome. If any of these are missing, ask the user a focused question.
- To create a skill, start from [templates/skill/SKILL.md.template](templates/skill/SKILL.md.template).
- Write instructions in English, with lowercase, hyphen-separated folder names.
- Place coding workflows in `skills/engineering/` and cross-cutting workflows
  in `skills/productivity/`.
- Keep one objective per skill, ordered steps, and observable completion criteria.
- Load references only at the step where they are useful.
- Add scripts or resources only to meet a concrete need of the skill.
- Keep dependencies on other skills explicit and available; prefer a standalone
  skill when sharing adds no value.

## Verification

- Check the frontmatter, name/folder consistency, and relative links.
- Replace all template placeholders before publishing a skill.
- Test an intended request and an out-of-scope request with the target agent.
- Report which tests were actually performed and which remain to be done.
- Update the catalog in [README.md](README.md) when a skill becomes usable.