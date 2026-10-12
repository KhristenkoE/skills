# Skills

My collection of reusable agent skills. I maintain and expand it as I find useful workflows, conventions, and tools to share across projects.

Skills use the portable `SKILL.md` format for Claude Code, Codex, and other compatible agent hosts. Each skill describes its own purpose, usage, and required capabilities.

## Install

### Claude Code, Codex, and other hosts

```sh
npx skills@latest add KhristenkoE/skills
```

Choose the skills and hosts you want during installation. To target Claude Code and Codex:

```sh
npx skills@latest add KhristenkoE/skills --agent claude-code codex
```

Add `--global` to install for your user across projects. Use `--skill <name>` to select a specific skill.

```sh
npx skills@latest add KhristenkoE/skills --list
npx skills@latest check
npx skills@latest update
```

### Claude Code plugin

Run inside Claude Code:

```text
/plugin marketplace add KhristenkoE/skills
/plugin install khristenko-skills@khristenko
```

Invoke a bundled skill as `/khristenko-skills:<skill-name>`. The plugin includes the collection; the cross-agent installer lets you select individual skills. Choose one installation method per Claude Code environment to avoid duplicates.

GitHub installation requires these files to be published to the repository. For a local checkout, use `npx skills@latest add /absolute/path/to/repo` or `claude --plugin-dir /absolute/path/to/repo`.

### Manual installation

Copy a complete skill directory, including its supporting files, into your host's skill directory.

| Host | Project directory | Personal directory |
| --- | --- | --- |
| Codex | `.agents/skills/` | `~/.agents/skills/` |
| Claude Code | `.claude/skills/` | `~/.claude/skills/` |

Other hosts have their own discovery locations. Installation makes instructions available; tools, subagents, and model selection depend on the host and the individual skill.

## Available skills

| Skill | Purpose |
| --- | --- |
| [orchestrator](skills/orchestrator/SKILL.md) | Coordinate project execution from requirements and saved progress. |
| [implementor](skills/implementor/SKILL.md) | Complete a bounded implementation or repair task. |
| [reviewer](skills/reviewer/SKILL.md) | Independently review a task or milestone against requirements. |
| [tester](skills/tester/SKILL.md) | Verify behavior and acceptance criteria with reproducible evidence. |
| [challenge-ideas](skills/challenge-ideas/SKILL.md) | Evaluate proposals across domains with independent judgment and constructive disagreement. |

These four can work together; see [the orchestration workflow](docs/orchestration.md). Future skills may cover unrelated tasks and do not need to participate in this workflow.

## Repository layout

```text
skills/<skill-name>/SKILL.md   Skill instructions and metadata
skills/<skill-name>/          Optional references, scripts, and assets
docs/                        Additional usage and host setup guidance
evals/                       Behavioral evaluation scenarios
.claude-plugin/              Claude Code plugin and marketplace metadata
```

## Adding a skill

Create `skills/<skill-name>/SKILL.md` with a matching name and a concise description of when to use it:

```yaml
---
name: skill-name
description: Describe the task this skill handles and when it applies.
---
```

Follow the frontmatter with the instructions. Keep supporting resources inside the skill directory so it can be installed independently. Document any required companion skills or host capabilities. Add an entry to the table above and evaluation scenarios when useful.

The installer and Claude plugin discover folders under `skills/` automatically. Increment the plugin version when publishing an updated collection.

## References

- [Agent skills in OpenAI products](https://learn.chatgpt.com/docs/build-skills)
- [Claude Code skills](https://code.claude.com/docs/en/skills)
- [Cross-agent skills installer](https://github.com/vercel-labs/skills)
