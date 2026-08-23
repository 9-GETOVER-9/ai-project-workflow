# AI Project Workflow

A Codex skill for building software with AI without letting the AI sprint into code too early.

![AI Project Workflow social card](assets/social-card.png)

It turns a vague project idea into a staged workflow:

1. Grill the idea
2. Check it against reality
3. Research comparable projects and fast-moving changes
4. Cut the plan down to the simplest useful MVP
5. Build in small slices
6. Review the architecture and code before calling it done

## Why

AI coding often fails before the first line of code: unclear requirements, copied architecture, over-engineering, and late review.

This skill gives Codex a simple collaboration protocol so it knows when to ask, when to research, when to simplify, and when to build.

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

The skill does not force every stage every time. It chooses the lightest process that fits the project:

- New project: full workflow
- Small feature: compressed clarification and review
- Bug fix: narrow investigation, fix, verify

## Included Files

- `SKILL.md`: the actual Codex skill
- `agents/openai.yaml`: display metadata for Codex

## License

MIT
