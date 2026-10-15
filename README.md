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

Copy a skill folder into `~/.claude/skills/` (personal) or `.claude/skills/` (per project).

## Skills

| Skill | Description |
| --- | --- |
| [tdd](skills/tdd) | Test-driven development: use-case-driven, red → green tests. Adapted from [mattpocock/skills](https://github.com/mattpocock/skills) (MIT). |
