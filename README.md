# skills

A collection of Agent Skills for Claude Code and other agents that support the `SKILL.md` format.

## Layout

Each skill lives in its own folder:

```
skills/
  <skill-name>/
    SKILL.md        # frontmatter (name, description) + instructions
    ...             # optional scripts, references, assets
```

## Using a skill

Install with the [skills CLI](https://github.com/vercel-labs/skills):

```sh
npx skills add ayoubben18/skills --skill tdd
```

Or copy a skill folder into `~/.claude/skills/` (personal) or `.claude/skills/` (per project).

## Skills

| Skill | Description |
| --- | --- |
| [tdd](skills/tdd) | Test-driven development: use-case-driven, red → green tests. |
