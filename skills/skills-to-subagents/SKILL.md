---
name: skills-to-subagents
description: Analyze the current project context and existing `.agents/skills` to design and create high-value subagents. Use this when the user asks to create subagents, specialize agent behavior, or convert repeated skill workflows into isolated reusable agents. Always read `AGENTS.md`, `.agents/rules`, `.agents/skills`, `.agents/agents`, and `.agents/ai-scaffolding-context.md` first when available before proposing or creating subagents.
disable-model-invocation: true
argument-hint: [target area or stack]
---

# Skills to Subagents

Design and create project-specific subagents grounded in the repository's real skills and rules.

## Goal

When invoked, convert skill-level knowledge into practical subagents that are:

- clearly scoped,
- justified by isolation/complexity needs,
- reusable across future tasks,
- aligned with project conventions.

Create the final subagent definitions directly in markdown.

## Subagent suitability rule

Use subagents for workflows that benefit from isolated context, specialization, verification independence, or parallel execution.

Prefer plain skills instead when the task is quick, one-off, or mostly knowledge retrieval without isolation needs.

## Operating flow

### Step 0 - Mandatory shared-context check

Before proposing any subagent:

1. Read `.agents/ai-scaffolding-context.md` if present.
2. Reuse stack, maturity, constraints, priorities, and prior stage outputs from that file.
3. Only explore or ask questions for gaps or contradictions.

### Step 1 - Mandatory discovery

Before proposing any subagent:

1. Read root `AGENTS.md`.
2. Read `.agents/rules/` (if present).
3. Inventory existing `.agents/agents/` to avoid duplicate or conflicting subagents.
4. Inventory `.agents/skills/`:
   - skill name,
   - what it does,
   - whether it is task-heavy, knowledge-heavy, or mixed,
   - side effects (writes files, runs commands, installs tools),
   - expected repetition level.

If repository evidence is sparse, state assumptions explicitly and continue conservatively.

### Step 2 - Build a skill capability map

Create a compact map of current skills grouped by function:

1. Discovery/research
2. Generation/scaffolding
3. Validation/review
4. Installation/integration
5. Governance/rules

Use this map to detect where delegation gives the highest value.

### Step 3 - Decide where a subagent is warranted

Apply the decision criteria from `references/SUBAGENT_CREATION_CRITERIA.md`.

Prefer subagents when at least one is true:

- workflow has many steps and context can distract the parent agent,
- task benefits from isolated reasoning,
- independent verification/audit perspective is needed,
- same workflow is reused often,
- parallelization can reduce turnaround time.

Avoid creating subagents for trivial one-step tasks.

### Step 4 - Propose candidates with rationale

For each candidate subagent, provide:

1. Name (kebab-case)
2. Trigger context (when to delegate)
3. Inputs and expected outputs
4. Why subagent (instead of direct skill usage)
5. Risks if not delegated

Keep the initial set focused (typically 2-4 subagents).

Approval boundary:

- in direct use, get explicit user approval before writing persistent files under `.agents/agents/`,
- if the user explicitly asked to create subagents, that request can count as approval for the agreed candidate set,
- in `ai-scaffolding` stage 5, treat the orchestrated flow plus the approved candidate list as sufficient approval.

### Step 5 - Ask only unresolved questions

Ask only if candidate responsibilities or target stack remain unclear after reading:

1. `.agents/ai-scaffolding-context.md`,
2. `AGENTS.md`,
3. `.agents/rules/`,
4. `.agents/skills/`.

If you ask anything and the shared context exists, append the answer there.

### Step 6 - Create subagents directly

For approved candidates, write the subagent files directly.

Each generated subagent must include:

- frontmatter with `name` and `description`,
- precise description with delegation cues,
- concise and actionable process,
- explicit output contract,
- model/readonly settings when justified.

Store subagents in `.agents/agents/<name>.md`.

Minimum metadata rules:

- `name`: kebab-case and specific.
- `description`: clearly state when to delegate. Prefer phrases like `Use proactively when...` or `Automatically delegate when...`.
- `model`: use `inherit` by default, `fast` for lightweight verifier-style tasks.
- `readonly`: set `true` only when the subagent should not modify files.

### Step 7 - Validate coverage and overlap

Run a quick overlap check:

1. No duplicate responsibilities.
2. Each subagent has one primary purpose.
3. Delegation boundaries are clear.
4. Coverage aligns with project goals in `AGENTS.md`.
5. No subagent duplicates an existing skill, command, rule, or persistent subagent without a clear isolation benefit.

If overlaps exist, merge or narrow scopes before finalizing.

## Required output contract

Always report:

1. Discovery summary (`AGENTS.md`, rules, skill inventory, and shared context findings).
2. Capability map by skill domain.
3. Candidate subagents with rationale.
4. Subagents created (file paths).
5. Deferred candidates (with reason).
6. Approval basis used for any persistent subagent files.
7. Assumptions and open risks.

If `.agents/ai-scaffolding-context.md` exists, update it with:

- capability map summary,
- chosen candidates,
- files created,
- deferred items and risks.

## Anti-patterns

- Creating generic subagents without reading local skills.
- Creating many near-duplicate agents.
- Writing vague descriptions that do not trigger delegation.
- Skipping explicit output formats.
- Overfitting subagents to one prompt only.
- Re-asking questions already answered in `.agents/ai-scaffolding-context.md`.

## Internal references

- `references/SUBAGENT_CREATION_CRITERIA.md`
- `references/SUBAGENT_TEMPLATES.md`
