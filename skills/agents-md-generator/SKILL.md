---
name: agents-md-generator
description: Create or refactor root AGENTS.md/CLAUDE.md as a lean, high-signal agent rules file grounded in real repository evidence (scripts, CI, tooling), with progressive disclosure and stack-specific conventions. Use this whenever the user asks to create, rewrite, standardize, audit contradictions, or reduce token cost in AGENTS.md/CLAUDE.md, or to define agent commands, boundaries, and project conventions for stacks like Java, Go, React, Next.js, Python, or .NET, even if they do not name this skill explicitly. Before exploring deeply or asking questions, first check whether `.agents/ai-scaffolding-context.md` already contains the needed context.
---

# Agents MD Generator

Create a practical, lightweight `AGENTS.md` in the project root that improves coding-agent reliability without bloating context.

Use this skill when the user wants to:

- create `AGENTS.md` or `CLAUDE.md` from scratch,
- refactor an existing one,
- define repository conventions for agents,
- adapt guidance to a target stack (Java, Go, React, Next.js, and others),
- reduce token usage while keeping strong guidance.

`CLAUDE.md` and `AGENTS.md` are equivalent for this workflow. If the user asks for one, satisfy the request; optionally note cross-tool compatibility.

## Outcome

Produce:

1. A root `AGENTS.md` with high-signal rules that apply to most tasks.
2. Progressive-disclosure links to deeper docs when needed.
3. Stack-aware guidance that combines shared context + repo evidence + strong defaults.

If no docs tree exists, create one only when it adds value.

## Operating Principles

1. Keep root `AGENTS.md` concise and durable.
2. Prefer capability-level guidance over brittle file-path details.
3. Include concrete commands for build/test/lint/typecheck.
4. Separate global rules from domain-specific conventions.
5. Encode boundaries clearly (`always`, `ask first`, `never`).
6. Resolve ambiguity from shared context and repository evidence before asking questions.
7. Do not invent commands; prefer commands verified from repo scripts/config/CI.

## Workflow

### Step 0 - Hydrate from shared context first

If `.agents/ai-scaffolding-context.md` exists:

1. Read it before exploring deeply.
2. Reuse any valid information about stack, maturity, commands, constraints, and priorities.
3. Only explore or ask questions for gaps, contradictions, or validation.

If the shared context conflicts with current repository evidence, prefer repository evidence and note the conflict.

### Step 1 - Detect project context

Inspect the repository to infer:

- Primary and secondary stacks.
- Build/test/lint/typecheck commands.
- Package manager/tooling.
- Existing docs and rule files.
- Repo structure style (monorepo, single package, services, apps/packages).

Use lightweight discovery first (file names/config markers) before deep reads.

### Step 2 - Extract conventions from evidence

Look for conventions in:

- lint/format configs,
- test setup,
- CI workflows,
- representative source modules,
- existing onboarding docs.

Promote only stable conventions. Avoid copying transient or redundant instructions.

Command reliability rules:

- Prefer commands found in `package.json`, `Makefile`, Maven/Gradle files, task runners, or CI workflows.
- If a command is uncertain, mark it explicitly as "if configured" instead of presenting it as guaranteed.
- Avoid generic placeholders when concrete repo commands are available.

### Step 3 - Ask only missing questions

Ask a focused question only when a missing or conflicting detail materially changes `AGENTS.md` behavior.

Before asking:

1. check `.agents/ai-scaffolding-context.md`,
2. check repository evidence,
3. check existing `AGENTS.md`/docs.

If you ask anything and receive an answer, update `.agents/ai-scaffolding-context.md` when that file exists.

### Step 4 - Build minimal root AGENTS.md

Use this shape unless the user requests another format:

1. Project purpose (one sentence).
2. Stack + package manager.
3. Standard commands (build/test/lint/typecheck/dev).
4. Working norms (small, practical, repo-specific).
5. Boundaries (`always`, `ask first`, `never`).
6. Progressive disclosure links to detailed docs.

Keep it short. Move detailed policies to docs when they are not globally required.

### Step 5 - Add stack-specific conventions

If the user provides a target stack, or the repository strongly indicates one, include a stack section with practical defaults.

Use `references/STACK_CONVENTIONS.md` for starter guidance, then adapt to repo reality.

### Step 6 - Handle monorepos correctly

For monorepos:

- Keep root `AGENTS.md` focused on shared policies and navigation.
- Recommend package-level `AGENTS.md` files only when necessary.
- Avoid duplicating detailed package rules in root.
- Remember scope precedence: the closest `AGENTS.md` to the working directory should be treated as most specific.

### Step 7 - Validate quality before finalizing

Before writing final output, check:

- No contradictory instructions.
- Commands are executable and stack-correct.
- No stale hardcoded paths unless clearly stable.
- No obvious low-value filler.
- Root file remains concise.
- If PR/release/deploy instructions are included, ensure they are evidence-based and not invented.

## Output Contract

When generating or updating `AGENTS.md`, ensure the file includes:

- clear project context,
- command references,
- coding/testing expectations,
- safety boundaries,
- links to deeper docs when needed.

Use the template in `references/AGENTS_MD_TEMPLATE.md` as baseline, then customize.

If `.agents/ai-scaffolding-context.md` exists, update it with:

- commands confirmed,
- stack decisions used,
- files created or updated,
- any assumptions or conflicts.

## Stack Adaptation Policy

Blend three sources:

1. Repository evidence (highest priority).
2. Shared AI scaffolding context.
3. Best-practice defaults for the requested stack.

If they conflict, prefer repository evidence and explain tradeoffs briefly.

## Conflict Handling

If existing docs disagree:

1. Preserve behavior closest to current repo tooling.
2. Flag the conflict in a short note.
3. Ask one focused question only if it materially changes behavior.

## Anti-patterns to avoid

- Dumping full framework documentation into root `AGENTS.md`.
- Listing too many hard rules with no rationale.
- Generic filler like "write clean code".
- Massive static path maps that rot quickly.
- Forcing one stack pattern when repo clearly uses another.
- Re-asking questions already answered in `.agents/ai-scaffolding-context.md`.

## Internal references for this skill

Use the bundled references in this skill folder as the baseline:

- `references/AGENTS_MD_TEMPLATE.md`
- `references/STACK_CONVENTIONS.md`

Combine them with repository evidence to keep outputs concise, context-efficient, and reliable.
