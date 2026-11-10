# emstack

A simple stack of agent skills, subagents, and commands for software engineering managers. Each piece plays one role on a small virtual team (product, tech lead, engineer, tester) so a feature can go from a rough idea to reviewed code with a human approving each handoff.

## How it fits together

```
idea ──► advisor ──► pm ──► tl ──► coder ──► tester
         (design doc) (PRD)  (tech spec) (code)  (tests)
```

The `/feature` command runs the same flow automatically with two subagents:

```
/feature <idea>
  ├─ architect (pm + tl)      writes the PRD and tech spec, waits for your "go"
  ├─ engineer  (coder + tester) implements on a branch with passing tests
  └─ architect                reviews the diff against the plan; loops until clean
```

## Skills

Installed for both Claude Code and Gemini (Antigravity).

| Skill | Role | Produces |
|---|---|---|
| **advisor** | Product advisor. Pressure-tests an idea before any spec exists: digs into the real pain, challenges premises, and finds the narrowest testable version. | `design-doc-<name>.md` (Problem, Approach, Key Decisions, Risks, Open Questions, handoff notes) |
| **pm** | Senior Product Manager. Turns an ambiguous request into a precise, implementable PRD. Defines *what*, not *how*. | `features/YYYY-MM-DD-<idea>.md` |
| **tl** | Principal Engineer / Architect. Turns a PRD or design doc into a pragmatic technical design. Does not write code. | `specs/YYYY-MM-DD-<idea>.md` (components, data model, API contracts, tradeoff log, ops concerns) |
| **coder** | Senior Software Engineer. Implements a technical design as production-grade, mergeable code. | Code |
| **tester** | Senior Test Engineer. Writes an adversarial, behavior-focused test suite (unit, integration, edge cases) against the code and its spec. | Tests |
| **analyzer** | Senior engineer code reviewer. Recommends concrete ways to make a codebase smaller and simpler (boilerplate, needless abstraction, dead code). Read-only by default; Java-leaning but language-agnostic. | A simplification report |

## Subagents (Claude Code only)

| Agent | Model | Preloaded skills | Job |
|---|---|---|---|
| **architect** | opus | pm, tl | Plans the work before implementation and reviews it afterward. Never implements. |
| **engineer** | sonnet | coder, tester | Implements the tech spec step by step on a feature branch, with tests, then hands off for review. |

## Commands (Claude Code only)

| Command | What it does |
|---|---|
| `/feature <description>` | Runs the plan → approve → implement → review loop using the architect and engineer subagents. Stops for your approval after planning and before merging. |

## Installation

Requires [Ansible](https://docs.ansible.com/). Run the playbooks from the repo root:

```bash
ansible-playbook install_skills.yml    # skills -> ~/.claude/skills and ~/.gemini/antigravity/skills
ansible-playbook install_agents.yml    # subagents -> ~/.claude/agents
ansible-playbook install_commands.yml  # commands -> ~/.claude/commands
```

The playbooks copy files and overwrite any existing copies, so re-run them after editing anything in `files/`.

## Repository layout

```
files/
  <skill>/SKILL.md     one folder per skill
  agents/*.md          Claude Code subagent definitions
  commands/*.md        Claude Code slash commands
install_skills.yml     installs skills (Claude + Gemini)
install_agents.yml     installs subagents (Claude)
install_commands.yml   installs commands (Claude)
```

## License

MIT. See [LICENSE](LICENSE).
