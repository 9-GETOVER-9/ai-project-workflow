# AI Project Workflow

A Codex skill for building software with AI without letting the AI sprint into code too early.

![AI Project Workflow social card](assets/social-card.png)

It turns a vague project idea into a staged workflow:

0. Set up project memory when entering a real repo
1. Grill the idea
2. Check it against reality
3. Research comparable projects and fast-moving changes
4. Cut the plan down to the simplest useful MVP
5. Prototype the uncertain parts before locking the MVP
6. Plan the MVP as vertical slices
7. Build one slice at a time
8. Review both code quality and requirement fit before calling it done

## Why

AI coding often fails before the first line of code: unclear requirements, copied architecture, over-engineering, and late review.

This skill gives Codex a simple collaboration protocol so it knows when to ask, when to research, when to simplify, and when to build.

It also adds a prototype checkpoint when the user needs to see whether the product matches their intent before implementation hardens.

It can also establish lightweight project memory:

- `docs/agents/issue-tracker.md` for where work items live
- `CONTEXT.md` for shared project vocabulary
- `docs/adr/` for important architecture decisions

For larger MVPs, it prefers vertical slices over layer-only task lists, so each slice delivers one verifiable user behavior instead of only "database", "backend", or "frontend" work.

## Install

Copy this folder into your Codex skills directory:

```text
~/.codex/skills/ai-project-workflow
```

Then start a project with:

```text
$ai-project-workflow
```

## Workflow

For a new idea, the first substantive response identifies the current stage and
asks about the product decisions that are still missing. The agent waits for
those answers before implementing anything that depends on them. Choosing a
technology stack for you does not mean choosing the product's purpose for you.

For example:

```text
$ai-project-workflow I want to build an AI notes tool. Help me clarify the MVP.
```

Expect questions about who will use it and what AI should do with a real note.
After the answers, the agent summarizes the scope and acceptance criteria and
continues the authorized work. If you request a prototype to review first,
it presents a preview and waits for your feedback before production work.

An already confirmed brief or precise small edit can proceed directly. You
can also explicitly delegate choices:

```text
$ai-project-workflow Skip the interview. Choose the scope for a local todo demo.
```

In that case, the agent labels its assumptions and proceeds. Continuing a
project resumes its last confirmed stage instead of restarting the interview.

The skill does not force every stage every time. It chooses the lightest process that fits the project:

- New project: full workflow
- Small feature: compressed clarification and review
- Bug fix: narrow investigation, fix, verify

`$ai-project-workflow` remains the main entrypoint. Future stage-specific skills can be added underneath it without forcing the user to remember every helper name.

## Included Files

- `SKILL.md`: the actual Codex skill
- `agents/openai.yaml`: display metadata for Codex
- `docs/BEHAVIOR_CHECKS.md`: behavioral regression scenarios and validation notes

## License

MIT
