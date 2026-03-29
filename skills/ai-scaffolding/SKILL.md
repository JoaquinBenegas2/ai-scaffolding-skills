---
name: ai-scaffolding
description: Build or upgrade an AI project scaffolding by creating a shared AI scaffolding context, root AGENTS guidance, modular rules, reusable commands, and supporting skills. Use this skill proactively whenever the user asks for AI scaffolding, agent bootstrap, AGENTS.md setup, .agents folder setup, rules/commands/skills initialization, or phased agent architecture by stack (especially Java, but also Go, Python, JavaScript/TypeScript, React, Next.js, Angular, and .NET). Always perform adaptive discovery first, persist findings to `.agents/ai-scaffolding-context.md`, ensure the required companion skills exist, then run them in strict sequence.
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
  commands/
  ai-scaffolding-context.md
AGENTS.md
```

If parts already exist, refine and complete instead of replacing blindly.

## Required companion skills

This workflow expects these skills to be available:

1. `agents-md-generator`
2. `tech-rules-generator`
3. `tech-commands-generator`
4. `tech-skill-installer`
5. `skills-to-subagents`

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

Skill categories in this workflow:

- `companion skills`: the orchestrator dependencies listed above that make the 5-stage workflow run.
- `project skills`: the stack- and repo-specific skills that `tech-skill-installer` must discover for the target repository.

Do not treat companion-skill validation as a substitute for project-skill discovery or installation.

## Mandatory execution order

Always guide the workflow in this exact sequence:

1. `agents-md-generator`
2. `tech-rules-generator`
3. `tech-commands-generator`
4. `tech-skill-installer`
5. `skills-to-subagents`

Never reorder the sequence unless the user explicitly requests a different order.

Execution preference:

- By default, prefer ephemeral subagents (isolated, non-persistent delegated runs) for each stage when the host supports them.
- Treat these ephemeral subagents as execution-time helpers only: create them for the current run, pass the needed context, collect the result, and do not persist them under `.agents/agents/`.
- Use `references/SUBAGENT_PROMPT_CONTRACT.md` to structure every delegated stage prompt so the handoff stays consistent.
- If the host does not support ephemeral or isolated subagents, run the stages sequentially in the same conversation while preserving the shared context file.
- Create persistent subagents under `.agents/agents/` only when the user explicitly asks for reusable subagents or when a later stage is specifically generating them as project artifacts.

## Operating flow

### Step 1 - Repository discovery and maturity detection

Before asking questions:

1. Read root `AGENTS.md` if present.
2. Inspect `.agents/` current state (`agents`, `skills`, `rules`, `commands`).
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
2. Build the stage handoff from `.agents/ai-scaffolding-context.md` plus current repo evidence.
3. If ephemeral subagents are supported, delegate the stage with the prompt structure from `references/SUBAGENT_PROMPT_CONTRACT.md`.
4. If ephemeral subagents are not supported, run the stage directly in the current conversation while preserving the same handoff structure.
5. Ask the skill to read `.agents/ai-scaffolding-context.md` before exploring or questioning further.
6. Run the skill for its stage objective.
7. Read the outputs it produced.
8. Update `.agents/ai-scaffolding-context.md` with:
   - files created or updated,
   - key decisions,
   - remaining gaps,
   - assumptions or risks.

Stage-specific context expectations:

- `agents-md-generator`: persist the root guidance decisions and repo-level operating conventions.
- `tech-rules-generator`: persist the selected technologies, rule topics, and rule-pack rationale.
- `tech-commands-generator`: persist the command files created, the Git baseline kept or omitted, stack-specific commands added, and the repo evidence behind them.
- `tech-skill-installer`: persist the companion-skill validation separately from the project-skill search queries, default `.agents/` install target, installed project skills, optional deferred opportunities, unmet domains, dependency approvals, and bundle rationale.
- `skills-to-subagents`: persist which reusable subagents were created or intentionally skipped and why.

Delegation rule:

- Default to ephemeral subagents for stage isolation when possible.
- Fall back gracefully to in-thread execution when the host does not support them.
- Do not create persistent subagent files just to perform temporary orchestration.

Handoff rules:

- From stage 1 to 2: pass AGENTS constraints and project intent.
- From stage 2 to 3: pass generated rules plus workflow conventions that should shape the command pack.
- From stage 3 to 4: pass the command baseline, detected Git conventions, and uncovered capability gaps.
- From stage 4 to 5: pass installed/missing skills and bundle rationale.

### Step 4.5 - Handle missing custom skills after installer stage

After `tech-skill-installer` runs:

1. Inspect its output for:
   - installed project skills or executed install commands,
   - confirmation that the default `.agents/` target was used without selecting an environment-specific host,
   - `I could not find skills for: ...`
   - `I recommend creating skills for: ...`
2. If the installer did not confirm the default `.agents/` target, or did not install any project skills and did not emit at least one of the exact gap lines above, treat stage 4 as incomplete and rerun it with an explicit correction.
3. If project skills were installed and no custom skill creation is recommended, continue to the next stage.
4. If custom skill creation is recommended:
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
- whether stage 4 installed project skills, emitted explicit gaps, or had to be rerun for being incomplete.

### Step 5 - Consolidation and consistency checks

At the end of the 5-stage sequence, validate:

1. Scaffold structure exists and is coherent.
2. `AGENTS.md` and `.agents/rules/` do not conflict.
3. `.agents/commands/` aligns with `AGENTS.md`, `.agents/rules/`, and actual repo workflows.
4. `tech-skill-installer` kept installation on the default `.agents/` target and did not select an environment-specific host unless the user explicitly requested it.
5. `tech-skill-installer` produced one of these outcomes: installed project skills, or exact explicit gap lines for uncovered domains.
6. Installed/created project skills align with rules, commands, and stack.
7. The installed project-skill count stays within the expected 3-15 range, or the shortfall/excess candidates are explicitly justified.
8. `.agents/ai-scaffolding-context.md` reflects the final state accurately.
9. Outputs from the final stage remain non-overlapping and purpose-specific.

If conflicts exist, report them and propose the smallest safe fix.

## Output contract to user

Always report:

1. Maturity classification and why.
2. Questions asked and why they mattered.
3. Skill execution trace in order.
4. Files created/updated by stage.
5. Companion skills validated or installed.
6. Project skills discovered, installed, or explicitly marked as gaps.
7. Installation target used for project skills and whether any opportunities were intentionally deferred by the 15-skill cap.
8. Decisions propagated through the context file.
9. Whether missing custom skills were created with `skill-creator` or the manual fallback path.
10. Assumptions and unresolved risks.
11. Recommended next validation step.

## Anti-patterns

- Asking long generic questionnaires without reading the repo first.
- Running stages out of order.
- Keeping key context only in transient conversation instead of the context file.
- Ignoring existing project context in in-progress repositories.
- Replacing user artifacts when refinement is enough.
- Silently continuing when required companion skills are missing.
- Installing `find-skills` without explicit user approval.
- Treating `tech-skill-installer` as a companion-skill check instead of a project-skill installation stage.
- Allowing `tech-skill-installer` to select an environment-specific install target when the default `.agents/` path is sufficient.
- Allowing `tech-skill-installer` to install an unbounded number of project skills.

## Internal references

- `references/DISCOVERY_QUESTION_BANK.md`
- `references/SUBAGENT_PROMPT_CONTRACT.md`
- `references/AI_SCAFFOLDING_CONTEXT_TEMPLATE.md`
- `references/BASIC_SKILL_CREATION_GUIDE.md`

Use these bundled references together with any root `AGENTS.md` already present in the target repository to keep the workflow concrete, incremental, and reusable.
