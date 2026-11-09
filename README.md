# Agent Bullpen

My reusable team of Claude Code subagents. I built these while learning multi-agent development on my [reading tracker](../reading-tracker) project, then pulled them out here so every new project can start with the same team.

The point of this repo is learning. I wanted to understand how to split software work across specialized AI agents, how they hand off to each other, and where a human stays in the loop.

## The team

| Agent | Job | Can edit files? |
| --- | --- | --- |
| `planner` | Breaks a feature into small tasks with checkable acceptance criteria | No |
| `implementer` | Writes app code for one task | Yes, app code only |
| `test-writer` | Writes tests that prove the criteria and probe edge cases | Yes, tests only |
| `reviewer` | Reviews the diff and returns APPROVE or CHANGES REQUESTED | No |
| `debugger` | Reproduces a bug, finds the root cause and makes the smallest fix | Yes |
| `researcher` | Answers a question from docs or code, with sources. Runs on a cheap model | No |

`commands/build-feature.md` is the orchestrator. Running `/build-feature <description>` walks one feature through the planner, implementer, test-writer and reviewer and logs the run in `docs/agent-log/`. The `debugger` and `researcher` are called on their own when needed.

No agent ever commits, pushes or opens a pull request. I review every change in my IDE and make every commit myself.

## Installing

Make the agents and the command available in every project:

```sh
mkdir -p ~/.claude/agents ~/.claude/commands && cp ~/code/agent-bullpen/agents/*.md ~/.claude/agents/ && cp ~/code/agent-bullpen/commands/*.md ~/.claude/commands/
```

Rerun it after editing anything here. This repo is the source of truth and `~/.claude` holds copies.

If a project has its own `.claude/agents/planner.md`, that project version wins over the global one. That is how a project can customize an agent.

## What each project needs

The agents are generic. They learn the project from its `CLAUDE.md`, so each project's `CLAUDE.md` should list:

- the stack and test framework
- commands for lint, typecheck, tests and a full check
- where tests live and how they are named
- coding conventions
- any framework quirks, like a version newer than the model's training data
