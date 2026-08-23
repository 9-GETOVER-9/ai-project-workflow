---
name: ai-project-workflow
description: Run an AI-assisted project development workflow from idea clarification through research, MVP planning, implementation, and code review.
---

# AI Project Workflow

Use this skill when the user wants to start, shape, or build a software project with a disciplined AI development workflow. The goal is to keep Codex from jumping into code too early while still moving toward a shippable MVP.

## Core Rule

Do not run every stage by default. Select the lightest workflow that fits the user's project risk, uncertainty, and requested depth.

If the user explicitly asks for a full workflow, use all relevant stages. If the request is small or already well specified, compress or skip stages whose questions are already answered.

## Skill Roles

When separately installed skills with these names exist, load and follow them at the appropriate stage. If they are not installed, perform the equivalent role described here.

- `grill-me`: Pressure-test the user's idea and extract hidden requirements before implementation.
- `idea-reality-mcp`: Compare the idea against real user needs, constraints, market/workflow reality, and implementation feasibility.
- `Autosearch`: Search for comparable products, open-source repositories, architecture examples, or implementation precedents.
- `last30days-skill`: Check current developments from roughly the last 30 days when the project depends on fast-changing tools, APIs, frameworks, models, pricing, or platform rules.
- `caveman`: Reduce the plan to the simplest working version and attack over-engineering.
- `code-reviewer` or `thermo-nuclear-code-quality-review`: Review architecture, implementation, security, maintainability, tests, and unnecessary complexity.

## Default Order

1. `grill-me`
2. `idea-reality-mcp`
3. Research: `Autosearch`, `last30days-skill`, or both
4. `caveman`
5. MVP plan and implementation
6. `code-reviewer` / `thermo-nuclear-code-quality-review`

Research comes after requirements are clear enough to search well. `caveman` comes after research because simplification is stronger when it can reject specific tempting but unnecessary architecture choices.

## Stage Guide

### 1. Grill-Me

Use first unless the user has already provided a clear PRD or asks for a narrow code change.

Extract:

- Target user
- Real problem
- Current workaround
- Core jobs-to-be-done
- Must-have features
- Non-goals
- Constraints
- Success criteria
- MVP acceptance tests

Ask only the questions that materially change the plan. Avoid long questionnaires when a few pointed questions are enough.

### 2. Idea-Reality Check

Use before research or architecture when the product idea may be vague, ambitious, or based on untested assumptions.

Test:

- Is the problem frequent and painful enough?
- Why would the user not use an existing tool?
- What is the smallest behavior that proves value?
- What assumptions could kill the project?
- Which constraints are real versus imagined?
- Is the user asking for a product, an internal tool, an automation, a research prototype, or a learning project?

Output a clear recommendation: continue, narrow, pivot, or pause for more evidence.

### 3. Research

Use `Autosearch` when the project benefits from learning from existing products or open-source repositories.

Use `last30days-skill` when the answer may have changed recently, especially for:

- AI models and agent frameworks
- API changes
- pricing and rate limits
- platform policies
- fast-moving JavaScript, Python, or mobile frameworks
- security advisories
- newly released competing tools

Use both when the project is in a fast-moving space and architectural precedent matters.

Do not research endlessly. Start with a small candidate set that covers:

- Directly similar projects
- Architecture reference projects
- Tools or libraries worth reusing

For each important reference, capture what to borrow, what to avoid, and why it matters for this project.

### 4. Caveman Review

Use before implementation and again when the plan grows complicated.

Attack:

- premature microservices
- unnecessary queues, caches, workers, or databases
- avoidable auth, permissions, and multi-tenant complexity
- decorative abstractions
- technology choices that only look impressive
- features not required to prove the MVP

Prefer the simplest version that can validate the core promise.

### 5. Plan and Build

Create artifacts only when useful for the project or requested by the user. Good defaults for a new project are:

- `docs/PRD.md`
- `docs/ARCHITECTURE.md`
- `docs/MVP.md`
- `docs/DECISIONS.md`
- `docs/TODO.md`

Keep these documents short enough to remain operational. Do not turn planning into a substitute for building.

Before coding, state:

- Current phase
- What is locked
- What remains uncertain
- Files or modules likely to change
- Verification plan

Implement in small, reviewable slices.

### 6. Review

Use review at more than one point when risk justifies it:

- Architecture review before coding
- Module review after important slices
- Final review before calling the work complete

Look for:

- incorrect behavior
- missing edge cases
- security problems
- data loss risks
- test gaps
- over-engineering
- unclear ownership boundaries
- fragile abstractions
- performance traps

Findings should lead. Keep praise and summaries secondary.

## Which Stages Are Mandatory?

For a new serious project:

- Always use `grill-me`
- Usually use `idea-reality-mcp`
- Usually use either `Autosearch` or `last30days-skill`
- Always use `caveman` before implementation
- Always use review before completion

For a small feature:

- Use a compressed `grill-me`
- Skip broad research unless current facts matter
- Use `caveman` only if the solution is expanding
- Review the diff before completion

For a bug fix:

- Do not run the full product workflow
- Clarify expected behavior
- Investigate root cause
- Fix narrowly
- Review and verify

## Output Style

In chat, be concise and phase-aware. When the user asks for the full workflow, show the current stage and the next gate. When writing documents, separate product requirements, architecture decisions, MVP scope, and task lists instead of merging everything into one long plan.

Never silently move from planning or research into implementation when the user explicitly said not to code yet.
