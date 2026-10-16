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

## Install

With the [skills CLI](https://skills.sh):

```sh
# pick from a list
npx skills add ayoubben18/skills

# install one skill
npx skills add ayoubben18/skills --skill tdd
npx skills add ayoubben18/skills --skill challenge-tests
npx skills add ayoubben18/skills --skill issue-to-pr

# install everything, for every agent, no prompts
npx skills add ayoubben18/skills --all

# install globally (user-level) instead of into the current project
npx skills add ayoubben18/skills --skill tdd -g
```

Update later with `npx skills update`. Or copy a skill folder into `~/.claude/skills/` (personal) or `.claude/skills/` (per project).

## Skills

| Skill | Description |
| --- | --- |
| [tdd](skills/tdd) | Test-driven development: use-case-driven, red → green tests. |
| [challenge-tests](skills/challenge-tests) | Audit existing tests for gaps, weak assertions, redundancy and bloat. |
| [issue-to-pr](skills/issue-to-pr) | Take an issue or idea from root cause to a green, reviewed PR, merged on approval. |
