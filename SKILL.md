---
name: ai-project-workflow
description: Use when starting, shaping, or building a software project where requirements, MVP scope, research needs, prototype validation, or implementation order need disciplined coordination.
---

# AI Project Workflow

Use this skill when the user wants to start, shape, or build a software project with a disciplined AI development workflow. The goal is to keep Codex from jumping into production code too early while still moving toward a shippable MVP that the user has had a chance to see, feel, and validate.

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
- `prototype`: Build the smallest disposable or isolated prototype needed to validate the user experience, interaction model, data flow, or technical risk before locking the MVP.
- `code-reviewer` or `thermo-nuclear-code-quality-review`: Review architecture, implementation, security, maintainability, tests, and unnecessary complexity.

## Default Order

1. `grill-me`
2. `idea-reality-mcp`
3. Research: `Autosearch`, `last30days-skill`, or both
4. `caveman`
5. Prototype validation, when seeing or trying the idea would materially change the MVP
6. MVP plan and implementation
7. `code-reviewer` / `thermo-nuclear-code-quality-review`

Research comes after requirements are clear enough to search well. `caveman` comes after research because simplification is stronger when it can reject specific tempting but unnecessary architecture choices. Prototype work comes after a first simplification pass so the prototype stays small, and before the final MVP plan so user feedback can still reshape the build.

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

### 5. Prototype Validation

Use when the user needs to see, try, or compare a rough version before committing to the MVP.

Good prototype triggers:

- The main uncertainty is UI, interaction, information architecture, or workflow feel.
- The user says they want to see whether the product matches their imagination.
- A technical approach is risky and a small proof would settle it.
- Requirements are still abstract even after grilling.
- Multiple product directions remain plausible.

Keep prototypes deliberately narrow:

- Build only what answers the current question.
- Prefer throwaway, isolated, or clearly marked prototype code.
- Do not treat prototype code as production unless the user explicitly approves hardening it.
- Avoid auth, persistence, payments, deployment, and complex integrations unless they are the thing being tested.
- Capture what the prototype proved, disproved, and changed about the MVP.

After the user reviews the prototype, route based on the result:

- Matches the intent: fold the learning into the MVP plan.
- Partially matches: revise requirements, then prototype again only if the remaining uncertainty is visual or interactive.
- Does not match: return to `grill-me` or `idea-reality-mcp` before planning implementation.
- Feedback expands scope: run `caveman` again before accepting the expansion.

When useful, write the result to `docs/PROTOTYPE.md`: what was tested, what the user confirmed, what changed, and what should not be carried into production.

### 6. Plan and Build

Create artifacts only when useful for the project or requested by the user. Good defaults for a new project are:

- `docs/PRD.md`
- `docs/ARCHITECTURE.md`
- `docs/MVP.md`
- `docs/PROTOTYPE.md` when prototype learning affects the MVP
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

### 7. Review

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
- Use prototype validation when the user needs to see the idea, the UX is uncertain, or a small proof would reduce risk
- Always use review before completion

For a small feature:

- Use a compressed `grill-me`
- Skip broad research unless current facts matter
- Use `caveman` only if the solution is expanding
- Use a tiny prototype only when the interaction or visual direction is unclear
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
