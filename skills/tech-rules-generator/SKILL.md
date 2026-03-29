---
name: tech-rules-generator
description: Generate modular rule packs in `.agents/rules/[tech]/` from repository context and the existing root AGENTS.md. Use proactively when the user asks to create rules, conventions, standards, or AI scaffolding by technology (Java, Go, Python, JavaScript/TypeScript, .NET, etc.), especially when multiple rules files are needed. Always use this when the request includes creating a technology-specific rules folder or completing missing project rules. Before exploring deeply or asking questions, first check whether `.agents/ai-scaffolding-context.md` already contains the needed context.
---

# Tech Rules Generator

Create a complete, maintainable rules pack for a target technology under `.agents/rules/[tech]/`.

This skill is for modular project rules, not for writing one giant `AGENTS.md`.

## Primary objective

Given a target technology, generate focused rules files that:

1. Align with the existing root `AGENTS.md`.
2. Follow context-hygiene principles (short, specific, non-contradictory).
3. Use the naming convention `[tech]-<rule>.md`.
4. Form a practical rules baseline that can evolve without major rewrites.

## Required inputs

Before writing files:

1. Read root `AGENTS.md` (mandatory).
2. Read `.agents/ai-scaffolding-context.md` if it exists.
3. Detect or confirm target technology (`tech`).
4. Inspect existing `.agents/rules/` content to avoid duplication/conflicts.

If `AGENTS.md` does not exist, state that clearly, proceed with minimal assumptions, and recommend generating/refining `AGENTS.md` first.

## Output layout

Create:

```text
.agents/
  rules/
    [tech]/
      README.md
      [tech]-<rule-1>.md
      [tech]-<rule-2>.md
      ...
```

Naming is mandatory for rule files: `[tech]-<rule>.md`.

Examples:

- `java-concurrency.md`
- `java-exceptions.md`
- `java-testing.md`
- `java-logging.md`

## Rule authoring workflow

### Step 0 - Reuse shared context first

If `.agents/ai-scaffolding-context.md` exists:

1. Reuse its stack, architecture, maturity, constraints, and quality priorities.
2. Treat it as the first source for user-provided answers and accepted assumptions.
3. Only explore or ask questions for gaps or contradictions.

### Step 1 - Build context from AGENTS.md and shared context

Extract project constraints and priorities:

- architecture expectations,
- quality boundaries,
- workflow conventions,
- stack-specific goals,
- forbidden patterns.

Use this as the source of truth for what rules matter first.

### Step 2 - Determine rule topics for the tech

Start from `references/TECH_RULESET_MAP.md`, then adapt to repo evidence.

Prioritize topics that materially affect correctness, maintainability, and delivery quality.

### Step 3 - Ask only for unresolved gaps

Ask a focused question only if target technology, architecture, or rule strictness remains unclear after checking:

1. `.agents/ai-scaffolding-context.md`,
2. `AGENTS.md`,
3. repository evidence.

If you ask anything and the shared context file exists, append the answer there.

### Step 4 - Create a minimal complete pack

Target 6-12 rule files per technology (enough coverage, low noise).

Avoid over-fragmentation. If two tiny rules are tightly coupled, merge them.

### Step 5 - Write each rule with a stable structure

Use `references/RULE_FILE_TEMPLATE.md`.

Each rule must include:

- intent (why this rule exists),
- concrete do/dont guidance,
- short examples,
- verification checklist.

Write actionable statements; avoid generic filler.

### Step 6 - Create `.agents/rules/[tech]/README.md`

The README must:

1. Explain the scope of the ruleset.
2. List each rule file with one-line purpose.
3. Clarify how it complements root `AGENTS.md`.
4. Mention how to extend the ruleset safely.

## Quality gates

Before finalizing, check:

1. Every file follows `[tech]-<rule>.md` naming.
2. No contradiction against root `AGENTS.md`.
3. No duplicate rule content across files.
4. Rules are concise and technology-relevant.
5. No invented commands or workflows not supported by repo context.
6. README index matches actual files.

## Decision policy

When in tension, prioritize:

1. Existing root `AGENTS.md` intent.
2. Shared AI scaffolding context.
3. Repo evidence (scripts, CI, toolchain, source layout).
4. Conservative best-practice defaults for the target tech.

Do not force patterns that conflict with the repository's current direction.

## Technology coverage policy

Support at least these targets with adapted rule sets:

- Java
- Go
- Python
- JavaScript/TypeScript
- .NET (C#)

If user asks for another tech, derive a comparable ruleset using the same structure and quality gates.

## Output contract to user

Report:

1. Detected/used technology.
2. Files created under `.agents/rules/[tech]/`.
3. Why these rule topics were chosen.
4. Any assumptions due to missing context.

If `.agents/ai-scaffolding-context.md` exists, update it with:

- tech selected,
- rules created,
- topic rationale,
- open gaps or conflicts.

## Anti-patterns to avoid

- Dumping broad language documentation into rules files.
- Writing long theoretical essays with no operational value.
- Creating too many tiny files with overlapping content.
- Copying generic rules that do not reflect repo context.
- Ignoring existing `AGENTS.md` constraints.
- Re-asking questions already answered in `.agents/ai-scaffolding-context.md`.

## Reference inputs

This skill aligns with:

- `RAG/MEMORY_RULES/CLAUDE_RULES.md`
- `RAG/MEMORY_RULES/ANATOMY_OF_THE_CLAUDE_FOLDER.md`
- `RAG/WORKFLOWS/THE_CODING_AGENT_HARNESS.md`

Use these references to preserve modularity, progressive disclosure, and context efficiency.
