# CLAUDE.md

This repo is a collection of prompt files (skills, subagents, slash commands) for Claude Code and Gemini, plus Ansible playbooks that install them into the user's home directory. There is no application code, build, or test suite. The "source" is Markdown with YAML frontmatter.

## Layout

- `files/<skill>/SKILL.md`: one directory per skill (advisor, analyzer, coder, pm, tester, tl). The directory name must match the `name:` in frontmatter.
- `files/agents/*.md`: Claude Code subagents (`architect`, `engineer`). Frontmatter sets `tools`, `model`, and preloaded `skills`.
- `files/commands/*.md`: Claude Code slash commands (`feature`). Uses `$ARGUMENTS`.
- `install_skills.yml`, `install_agents.yml`, `install_commands.yml`: copy the files above into `~/.claude/...` (and `~/.gemini/antigravity/skills` for skills).

## Installing / testing changes

```bash
ansible-playbook install_skills.yml
ansible-playbook install_agents.yml
ansible-playbook install_commands.yml
```

The only way to "test" a change is to install it and use it in a Claude Code session. Editing files here does nothing until the playbook is re-run.

## Adding or renaming things

Each playbook has an explicit list (`skills`, `agents`, `commands` vars). When you add, rename, or remove a file under `files/`, update the matching list in the playbook **and** the tables in `README.md`. If a skill is renamed, also update any agent `skills:` lists that reference it.

## How the pieces connect

- Skill pipeline: advisor → pm → tl → coder → tester. analyzer is standalone.
- `architect` agent preloads `pm` + `tl`; `engineer` agent preloads `coder` + `tester`.
- `/feature` orchestrates architect (plan) → user approval → engineer (implement) → architect (review).
- Output locations defined in the skills: advisor writes `design-doc-<name>.md`, pm writes `features/YYYY-MM-DD-<idea>.md`, tl writes `specs/YYYY-MM-DD-<idea>.md`. Keep these consistent if you change one, since downstream roles look for them.

## Writing conventions for skills

- Frontmatter: `name`, `description` (this is what triggers the skill, so state when to use it and when not to), and usually `allowed-tools`.
- Body: a role/persona, a numbered process, a concrete output format, and ground rules. Follow the shape of the existing skills.
- Keep each role in its lane: product skills define *what* and never design or code; tl designs but never codes; coder/tester implement.
- Prefer direct, plain prose. Ask clarifying questions in small batches (2–3) via AskUserQuestion, and fall back to labeled assumptions when the user is unavailable.

## Known quirks

- The `architect` agent's `tools` list has no `Write`/`Edit`, yet its preloaded pm and tl skills write files. It can only write through `Bash`.
- Agents and commands are installed for Claude only; Gemini receives skills only.
