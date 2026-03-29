# Subagent Creation Criteria

This reference defines when to create subagents from existing project skills.

## Core principle

Use a subagent when isolation and specialization improve reliability, speed, or quality versus running the workflow inline.

## Decision matrix

Score each candidate workflow from 0 to 2 on each axis:

1. Complexity
- 0: one-step action
- 1: moderate multi-step workflow
- 2: long workflow with branching decisions

2. Isolation value
- 0: parent context is enough
- 1: some risk of context contamination
- 2: high need for isolated context

3. Reuse frequency
- 0: rare/one-off
- 1: occasional recurring task
- 2: frequent recurring workflow

4. Verification independence
- 0: no independent check needed
- 1: some validation useful
- 2: independent audit/verification is important

5. Parallelization benefit
- 0: no gain from parallelism
- 1: partial gain
- 2: substantial gain

Interpretation:

- 0-3: keep as inline skill usage
- 4-6: consider subagent if team expects reuse
- 7-10: create subagent

## Trigger quality standard

Descriptions should include:

1. What the subagent is specialized in.
2. When to delegate (clear contexts and keywords).
3. What outputs it must return.

Good trigger phrasing examples:

- "Use proactively when implementing or restructuring project-wide agent rules."
- "Delegate automatically when a skill installation plan includes unresolved coverage gaps."
- "Use after implementation to independently verify outputs and identify missed edge cases."

## Scope boundaries

Every subagent should have:

- One primary responsibility.
- Clear in-scope and out-of-scope boundaries.
- A concise output schema.

If two candidates overlap heavily, either:

1. Merge into one broader subagent, or
2. Split by lifecycle stage (for example: planner vs verifier).

## Mapping from skill types to subagent archetypes

1. Discovery skills -> Research/Explorer subagent
2. Generation skills -> Builder/Scaffolder subagent
3. Validation skills -> Verifier/Auditor subagent
4. Installation skills -> Integrator/Operator subagent
5. Governance skills -> Rules architect subagent

## Minimum quality checklist

- Name is kebab-case and specific.
- Description is concrete and delegation-ready.
- Process has explicit ordered steps.
- Output format is explicit.
- Read/write constraints are intentional.
- Scope avoids overlap with existing subagents.
