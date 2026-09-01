---
name: ai-project-workflow
description: Use when starting, shaping, or building a software project where requirements, MVP scope, research needs, prototype validation, or implementation order need disciplined coordination.
---

# AI Project Workflow

Use this skill when the user wants to start, shape, or build a software project with a disciplined AI development workflow. The goal is to keep Codex from jumping into production code too early while still moving toward a shippable MVP that the user has had a chance to see, feel, and validate.

## Core Rule

Do not run every stage by default. Select the lightest workflow that fits the user's project risk, uncertainty, and requested depth.

If the user explicitly asks for a full workflow, use all relevant stages. If the request is small or already well specified, compress or skip stages whose questions are already answered.

## Invocation Modes

Use this skill as the user-facing entrypoint for a project workflow. The user may invoke it directly with `$ai-project-workflow` when starting a new idea, planning an MVP, or asking Codex to slow down before coding.

Inside the workflow, call stage skills automatically when the situation needs them. The user should not need to remember every sub-skill name. Tell the user which stage is active and why before doing substantial work.

- User-invoked: the user explicitly starts the overall workflow, such as `$ai-project-workflow I want to build...`.
- Model-invoked: Codex detects that a stage is needed, such as setup for a new repo, prototype for unclear UX, or review before completion.
- Manual override: if the user asks to skip, repeat, pause, or focus a stage, follow that instruction unless it would make the work unsafe or impossible.

## Skill Roles

When separately installed skills with these names exist, load and follow them at the appropriate stage. If they are not installed, perform the equivalent role described here.

- `setup`: Establish the project workspace conventions that future AI sessions should read and update.
- `grill-me`: Pressure-test the user's idea and extract hidden requirements before implementation.
- `idea-reality-mcp`: Compare the idea against real user needs, constraints, market/workflow reality, and implementation feasibility.
- `Autosearch`: Search for comparable products, open-source repositories, architecture examples, or implementation precedents.
- `last30days-skill`: Check current developments from roughly the last 30 days when the project depends on fast-changing tools, APIs, frameworks, models, pricing, or platform rules.
- `caveman`: Reduce the plan to the simplest working version and attack over-engineering.
- `prototype`: Build the smallest disposable or isolated prototype needed to validate the user experience, interaction model, data flow, or technical risk before locking the MVP.
- `to-slices` or `to-tickets`: Break the approved MVP into vertical slices that each deliver a verifiable user behavior.
- `code-reviewer` or `thermo-nuclear-code-quality-review`: Review architecture, implementation, security, maintainability, tests, and unnecessary complexity.

## Default Order

0. `setup`, when entering a real project repo for the first time
1. `grill-me`
2. `idea-reality-mcp`
3. Research: `Autosearch`, `last30days-skill`, or both
4. `caveman`
5. Prototype validation, when seeing or trying the idea would materially change the MVP
6. MVP plan and vertical slices
7. Implementation
8. `code-reviewer` / `thermo-nuclear-code-quality-review`

Research comes after requirements are clear enough to search well. `caveman` comes after research because simplification is stronger when it can reject specific tempting but unnecessary architecture choices. Prototype work comes after a first simplification pass so the prototype stays small, and before the final MVP plan so user feedback can still reshape the build.

## Stage Guide

### 0. Project Setup

Use when this is the first time the workflow enters a real project repo, or when the repo lacks stable AI-facing project conventions.

Setup is project foundation work, not product implementation. Establish only the lightweight files that help future sessions continue without rediscovering basics:

- `docs/agents/issue-tracker.md`: where work items live, such as GitHub Issues, Linear, or local markdown files.
- `CONTEXT.md`: the project glossary and durable product/domain language. Keep it about meanings, not implementation details.
- `docs/adr/`: short architecture decision records for important hard-to-reverse technical choices.

If these files already exist, read and respect them. Update them only when the current conversation resolves a real term or decision. Do not create heavy process documents before the project needs them.

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

### 6. MVP Plan and Vertical Slices

Create artifacts only when useful for the project or requested by the user. Good defaults for a new project are:

- `docs/PRD.md`
- `docs/ARCHITECTURE.md`
- `docs/MVP.md`
- `docs/PROTOTYPE.md` when prototype learning affects the MVP
- `docs/DECISIONS.md`
- `docs/TODO.md`
- `docs/agents/issue-tracker.md` when setup decides where work items live
- `CONTEXT.md` when stable project terms have been agreed
- `docs/adr/` entries when an architecture decision is hard to reverse

Keep these documents short enough to remain operational. Do not turn planning into a substitute for building.

Prefer vertical slices over layer-by-layer task lists. A vertical slice is one narrow user-visible behavior that cuts through every layer needed to make it real, such as database, API, UI, and tests. It should be independently demoable or verifiable.

Avoid horizontal slices such as "build database", "build backend", then "build frontend" unless a wide technical migration genuinely requires it. For product work, rewrite tasks as behavior:

- Good: "User can create one note and see it appear in the list."
- Good: "User can search saved notes by keyword."
- Weak: "Create database tables."
- Weak: "Build all API endpoints."

For larger MVPs, turn `docs/TODO.md` into slice-based tickets. Each slice should include what it delivers, acceptance criteria, and blockers if another slice must land first.

Before coding, state:

- Current phase
- What is locked
- What remains uncertain
- Files or modules likely to change
- Verification plan

Implement in small, reviewable slices.

### 7. Implementation

Implement one approved vertical slice at a time. For production behavior, prefer test-first work when the codebase supports it. Throwaway prototypes may skip production-level tests, but production code should have verification before completion.

### 8. Review

Use review at more than one point when risk justifies it:

- Architecture review before coding
- Module review after important slices
- Final review before calling the work complete

Use two review axes when there is an agreed requirement, spec, ticket, or prototype decision:

- Quality axis: does the code follow the repo's standards, stay maintainable, avoid unnecessary complexity, and handle important edge cases?
- Requirement axis: does the result actually match the PRD, MVP scope, ticket, prototype feedback, and user-confirmed decisions?

Keep the two axes separate. Good code that solves the wrong problem is still a failure, and correct behavior implemented in fragile code still needs attention.

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

## Modularization Path

This skill is currently the router and default fallback for the whole workflow. If the workflow grows, split stages into smaller skills only when a stage has enough detail to justify its own file.

Suggested future split:

- `project-setup`
- `project-grill`
- `idea-reality-check`
- `project-research`
- `mvp-caveman`
- `project-prototype`
- `to-mvp-spec`
- `to-slices`
- `workflow-review`

Keep `$ai-project-workflow` as the user-facing entrypoint. The smaller skills should be model-invoked helpers, so the user can still start with one command while Codex loads only the detailed stage guidance it needs.

## Which Stages Are Mandatory?

For a new serious project:

- Use setup once when entering a real repo that lacks AI-facing project conventions
- Always use `grill-me`
- Usually use `idea-reality-mcp`
- Usually use either `Autosearch` or `last30days-skill`
- Always use `caveman` before implementation
- Use prototype validation when the user needs to see the idea, the UX is uncertain, or a small proof would reduce risk
- Break the MVP into vertical slices before production implementation
- Always use review before completion

For a small feature:

- Use a compressed `grill-me`
- Skip broad research unless current facts matter
- Use `caveman` only if the solution is expanding
- Use a tiny prototype only when the interaction or visual direction is unclear
- Use at least one behavior-shaped slice instead of a layer-only TODO
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

## Project Memory

Use `CONTEXT.md` and ADRs to stop future sessions from forgetting settled meaning and decisions.

- `CONTEXT.md` records stable vocabulary: what important product and domain words mean.
- ADRs record important decisions: what was chosen, why, rejected alternatives, and consequences.
- Do not put every idea into ADRs. Create one only when the decision is meaningful, hard to reverse, and surprising without context.
- If a later user instruction changes a settled decision, update or supersede the relevant ADR instead of silently contradicting it.
