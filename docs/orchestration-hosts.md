# Host integration notes

Core role instructions are identical across hosts. Adapt execution at the boundary: create an agent, load its role skill, select its model if supported, provide a workspace and assignment, then collect its report. Check the actual host's tools and permissions rather than assuming a particular API.

## Codex / OpenAI agents

Install the four role folders under `.agents/skills/` for Codex. Supply the child's skill path explicitly and ask it to read the file, or preload the content when the runtime supports it. Use native create/message/wait capabilities where available. Map role models through the host's supported configuration; a `SKILL.md` instruction alone does not select a model.

For an API-based agent, register or attach skills through that runtime's documented mechanism and enable delegation separately. This repository does not implement an API runner. Ordinary ChatGPT chat is not assumed to expose these capabilities.

## Claude Code

Install the same four role folders under `.claude/skills/` through the cross-agent installer or manual copying, or install the repository's native plugin. Invoke `/orchestrator` for directly installed skills or `/khristenko-skills:orchestrator` for the plugin in the main conversation so it owns the loop. The orchestrator can use the host's native agent capabilities or project-defined subagents.

Optionally define project subagents in `.claude/agents/` with a matching role skill preloaded. For example:

```yaml
---
name: project-implementor
description: Implement one bounded assignment from the project orchestrator.
model: haiku
skills:
  - implementor
---
Follow the preloaded implementor skill and the supplied task assignment.
```

`haiku` is an example lower-cost alias, not a portable requirement. Select a supported model appropriate to your project. Analogous reviewer/tester definitions can preload `reviewer`/`tester`; choose their models separately. The example assumes directly installed skills. For plugin installation, use the host's resolved plugin-qualified skill name (such as `khristenko-skills:implementor`) or supply the accessible skill file explicitly. Check that the selected model and skill are actually available. Do not set permissive tool/permission modes merely to make the loop run.

Keep Claude-specific agent frontmatter here or in host configuration, outside the portable skill manifests. There is no need to duplicate skill bodies into subagent prompts.

## Unsupported capabilities

No subagents: report the limitation and obtain agreement before substituting sequential roles in one context; independence is reduced.

No model override: disclose inherited models. Do not claim cost savings or violate a strict cheap-model requirement.

No persistent host: save a checkpoint and resume on a later invocation. For automatic restarts, configure an external runner separately.

No isolated workspaces: serialize overlapping edits and freeze relevant artifacts while reviewed/tested. Do not run conflicting workers concurrently.
