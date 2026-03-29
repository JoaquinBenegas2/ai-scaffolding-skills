---
name: ai-scaffolding
description: Build or upgrade an AI project scaffolding by creating a shared AI scaffolding context, root AGENTS guidance, modular rules, and supporting skills. Use this skill proactively whenever the user asks for AI scaffolding, agent bootstrap, AGENTS.md setup, .agents folder setup, rules/skills initialization, or phased agent architecture by stack (especially Java, but also other stacks). Always perform adaptive discovery first, persist findings to `.agents/ai-scaffolding-context.md`, ensure the required companion skills exist, then run them in strict sequence.
---

# AI Scaffolding

Create a practical, reusable AI scaffolding in the target project with shared context and low ambiguity.

## Goal

When the user asks for AI scaffolding, this skill must:

1. Build enough context to reduce ambiguity.
2. Persist that context to `.agents/ai-scaffolding-context.md`.
3. Reuse the same context across the full scaffolding run.
4. Guide the companion skills in strict sequence.
5. Produce a final report with what was created, assumptions, and next steps.

## Target scaffold

The expected baseline structure is:

```text
.agents/
  skills/
  agents/
  rules/
  ai-scaffolding-context.md
AGENTS.md
```

If parts already exist, refine and complete instead of replacing blindly.

## Required companion skills

This workflow expects these skills to be available:

1. `agents-md-generator`
2. `tech-rules-generator`
3. `tech-skill-installer`
4. `skills-to-subagents`

Special dependency note:

- `tech-skill-installer` depends on `find-skills`.
- If `find-skills` is missing, always ask the user before installing it.
- Never auto-install `find-skills` without explicit user approval.
- If `tech-skill-installer` recommends creating missing custom skills, handle that recommendation after the installer stage before moving on.

Before orchestrating the workflow:

1. Check whether these skills already exist in the current environment.
2. If one or more are missing, install them before continuing.
3. If the missing dependency is `find-skills`, ask the user first and wait for approval before installing it.

Preferred installation policy:

- If the current bundle source is known, install the missing skills from that same source.
- If the current repository contains the companion skill folders locally, use that local path as the install source.
- If the source cannot be determined safely, ask one focused question for the source repo/path and then continue.

Do not silently skip a missing companion skill.

## Mandatory execution order

Always guide the workflow in this exact sequence:

1. `agents-md-generator`
2. `tech-rules-generator`
3. `tech-skill-installer`
4. `skills-to-subagents`

Never reorder the sequence unless the user explicitly requests a different order.

If the host supports isolated execution per skill, prefer isolated/forked execution to reduce context bleed. If not, run sequentially in the same conversation while preserving the shared context file.

## Operating flow

### Step 1 - Repository discovery and maturity detection

Before asking questions:

1. Read root `AGENTS.md` if present.
2. Inspect `.agents/` current state (`agents`, `skills`, `rules`).
3. Inspect stack evidence (tooling files, framework files, source layout, tests, CI).
4. Read `.agents/ai-scaffolding-context.md` if it already exists and reuse valid prior context.

Classify maturity using evidence:

- `greenfield`: no meaningful codebase yet, mostly empty/new project.
- `in-progress`: active codebase with existing structure and conventions.

Use `references/DISCOVERY_QUESTION_BANK.md` to decide what to ask.

### Step 2 - Adaptive questioning policy

Ask only what cannot be inferred from repository evidence or the existing context file.

Question budget:

- `greenfield`: ask up to 8 focused questions across 1-2 turns.
- `in-progress`: ask up to 3 focused questions in one turn.

If uncertainty remains after the budget is used, continue with explicit assumptions.

Prioritize questions that materially change outputs:

1. stack and runtime,
2. architecture style,
3. project goals and constraints,
4. team conventions,
5. desired initial scaffolding scope.

Every relevant user answer must be written back into `.agents/ai-scaffolding-context.md`.

### Step 3 - Build and persist the shared context

Create or update `.agents/ai-scaffolding-context.md` using `references/AI_SCAFFOLDING_CONTEXT_TEMPLATE.md`.

The context file must capture:

- project maturity,
- target stack,
- architecture style,
- quality priorities,
- constraints,
- repository evidence,
- explicit assumptions,
- answers provided by the user,
- outputs produced by each stage.

Treat this file as the shared source of truth for the entire scaffolding flow.

### Step 4 - Skill-by-skill orchestration

For each required skill in order:

1. Confirm the skill is installed.
2. Ask the skill to read `.agents/ai-scaffolding-context.md` before exploring or questioning further.
3. Run the skill for its stage objective.
4. Read the outputs it produced.
5. Update `.agents/ai-scaffolding-context.md` with:
   - files created or updated,
   - key decisions,
   - remaining gaps,
   - assumptions or risks.

Handoff rules:

- From stage 1 to 2: pass AGENTS constraints and project intent.
- From stage 2 to 3: pass generated rules and uncovered capability gaps.
- From stage 3 to 4: pass installed/missing skills and bundle rationale.

### Step 4.5 - Handle missing custom skills after installer stage

After `tech-skill-installer` runs:

1. Inspect its output for:
   - `I could not find skills for: ...`
   - `I recommend creating skills for: ...`
2. If no custom skill creation is recommended, continue to the next stage.
3. If custom skill creation is recommended:
   - ask the user whether to install `skill-creator`,
   - wait for the user's answer before deciding the creation path.

Decision policy:

- If the user approves installing `skill-creator`, install it and use it to create the missing skills.
- If the user declines, create the missing skills anyway using the lightweight process in `references/BASIC_SKILL_CREATION_GUIDE.md`.

Do not drop the missing-skill recommendations just because `skill-creator` is unavailable.

If `.agents/ai-scaffolding-context.md` exists, record:

- which skills were recommended for creation,
- whether `skill-creator` was approved,
- whether skills were created with `skill-creator` or the basic manual path.

### Step 5 - Consolidation and consistency checks

At the end of the 4-stage sequence, validate:

1. Scaffold structure exists and is coherent.
2. `AGENTS.md` and `.agents/rules/` do not conflict.
3. Installed/created skills align with rules and stack.
4. `.agents/ai-scaffolding-context.md` reflects the final state accurately.
5. Outputs from the final stage remain non-overlapping and purpose-specific.

If conflicts exist, report them and propose the smallest safe fix.

## Output contract to user

Always report:

1. Maturity classification and why.
2. Questions asked and why they mattered.
3. Skill execution trace in order.
4. Files created/updated by stage.
5. Companion skills validated or installed.
6. Decisions propagated through the context file.
7. Whether missing custom skills were created with `skill-creator` or the manual fallback path.
8. Assumptions and unresolved risks.
9. Recommended next validation step.

## Anti-patterns

- Asking long generic questionnaires without reading the repo first.
- Running stages out of order.
- Keeping key context only in transient conversation instead of the context file.
- Ignoring existing project context in in-progress repositories.
- Replacing user artifacts when refinement is enough.
- Silently continuing when required companion skills are missing.
- Installing `find-skills` without explicit user approval.

## Internal references

- `references/DISCOVERY_QUESTION_BANK.md`
- `references/SUBAGENT_PROMPT_CONTRACT.md`
- `references/AI_SCAFFOLDING_CONTEXT_TEMPLATE.md`
- `references/BASIC_SKILL_CREATION_GUIDE.md`
- `AGENTS.md`

Use these references to keep the workflow concrete, incremental, and reusable.
