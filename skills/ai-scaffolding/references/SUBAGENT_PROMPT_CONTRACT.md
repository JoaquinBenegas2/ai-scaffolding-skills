# Subagent Prompt Contract

Use this structure for every delegated prompt in the ai-scaffolding sequence.

## Required prompt sections

1. `Objective`
   - One sentence stating the exact expected outcome.

2. `Repository evidence`
   - Current files/folders and stack signals discovered.

3. `Project maturity`
   - `greenfield` or `in-progress` and short rationale.

4. `Constraints`
   - Hard constraints from `AGENTS.md`, rules, and user direction.

5. `Decisions already made`
   - Accepted decisions from previous stages that must be preserved.

6. `Target outputs`
   - Explicit file paths and artifacts expected from this stage.

7. `Output contract`
   - Ask the subagent to report:
     - files created/updated,
     - key decisions and rationale,
     - assumptions,
     - open risks/questions.

## Reusable template

```text
Objective:
<one-line objective>

Repository evidence:
<stack/tooling/layout summary>

Project maturity:
<greenfield|in-progress> - <why>

Constraints:
<must preserve / must avoid>

Decisions already made:
<bullet list>

Target outputs:
<explicit file paths and expected artifacts>

Output contract:
Return:
1) files created/updated,
2) key decisions + rationale,
3) assumptions,
4) open risks/questions.
```

## Stage-specific reminder

- Stage 1 (`agents-md-generator-subagent`): include high-level project intent and scope boundaries.
- Stage 2 (`tech-rules-generator-subagent`): include AGENTS constraints from stage 1.
- Stage 3 (`tech-skill-installer-subagent`): include rules coverage and any unresolved capability gaps.
- Stage 4 (`skills-to-subagents-subagent`): include final skill inventory and missing responsibility map.
