# Agent Skills

My coding agent skills: short, focused, reusable workflows organized following
the pattern used by
[mattpocock/skills](https://github.com/mattpocock/skills).

This repository includes focused skills for reliable tool use, MCP tool design,
and agent evaluation, plus a template for creating further skills.

## Structure

```text
agent-skills/
	AGENTS.md
	README.md
	skills/
		engineering/
			<skill-name>/
				SKILL.md
		productivity/
			<skill-name>/
				SKILL.md
	templates/
		skill/
			SKILL.md.template
```

- `engineering`: coding, testing, debugging, review, or architecture workflows.
- `productivity`: clarification, context handoff, or organization workflows.
- [templates/skill/SKILL.md.template](templates/skill/SKILL.md.template): a starting
	point to copy, not a skill to install.
- [AGENTS.md](AGENTS.md): instructions for agents working in this repository.

Within each skill, add `references/`, `scripts/`, or `assets/` only when a concrete
resource calls for it. The `SKILL.md` file must explain when to read or run each
resource and provide a relative path.

## Create A Skill

1. Choose a repeatable task and a category.
2. Create `skills/<category>/<skill-name>/`.
3. Copy the template into that folder as `SKILL.md`.
4. Replace the angle-bracket placeholders and set `name` to match the folder name.
5. Write the steps, expected outcome, and verification criteria in English.
6. Add the skill to the catalog below with its path and intended use.

Use names that are unique across the repository, lowercase, and hyphen-separated:
`diagnose-bug`, for example. The `name` field must exactly match the folder name,
contain 1 to 64 characters, and neither start nor end with a hyphen. The category
is not part of the skill name.

The `description` explains what the skill does and when to use it; keep it under
1024 characters. A useful description identifies concrete situations rather than
a generic label such as "development help".

### Choose The Invocation Mode

By default, a skill can be selected by the agent or invoked by the user, depending
on the tool's capabilities. For a workflow that should only be explicitly invoked
by the user, add this field to the frontmatter:

```yaml
disable-model-invocation: true
```

Support for this option depends on the agent. The template includes only the
required, portable fields: `name` and `description`.

### Verify Before Use

- The YAML frontmatter is valid and the name matches the folder.
- The description distinguishes this skill from others.
- The instructions form an executable procedure with an observable endpoint.
- Links and resources exist, and all template placeholders have been replaced.
- A trial with an intended request produces the expected outcome.
- An out-of-scope request does not cause unintended automatic invocation.

## Usage

The `skills/` folder is the source catalog, not a location automatically loaded
by every agent.

From the project where you want to use a skill, install it with the
[skills CLI](https://skills.sh/docs/cli):

```bash
npx skills add /path/to/agent-skills --skill skill-name
```

Replace the path and name with their actual values. This command requires
Node.js/npm and access to the npm registry. The CLI lets you select the target
agents and manages their installation locations.

For a manual installation compatible with GitHub Copilot, copy the skill folder
into `.github/skills/<skill-name>/` in the target project, keeping its resources.
Other agents may use different locations.

## Catalog

- [agent-good-practices](skills/productivity/agent-good-practices/SKILL.md): use
	existing tools reliably when capabilities overlap, state diverges, results grow,
	or errors are ambiguous.
- [mcp-tool-design](skills/engineering/mcp-tool-design/SKILL.md): design or audit
	MCP tool contracts, resource access, failure behavior, and compatibility cards.
- [agent-evals](skills/engineering/agent-evals/SKILL.md): build and diagnose agent
	evaluations with representative tasks, trustworthy graders, isolated trials,
	and meaningful capability and regression metrics.

The tool-use and MCP design skills build on Microsoft Research's
[tool-space interference article](https://www.microsoft.com/en-us/research/blog/tool-space-interference-in-the-mcp-era-designing-for-agent-compatibility-at-scale/).
The design skill also points to the official MCP specification for version-specific
behavior. The evaluation skill draws on Anthropic's
[Demystifying evals for AI agents](https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents).
Each skill is standalone and has a separate scope.

Validation performed: YAML frontmatter, name/folder consistency, placeholder and
local-link checks, and simulated intended and out-of-scope requests with GitHub
Copilot. Automatic client discovery, live MCP compatibility, and real
agent-evaluation runs remain untested.

## References

- [Reference repository](https://github.com/mattpocock/skills)
- [Agent Skills specification](https://agentskills.io/specification)
- [Skills in VS Code](https://code.visualstudio.com/docs/copilot/customization/agent-skills)