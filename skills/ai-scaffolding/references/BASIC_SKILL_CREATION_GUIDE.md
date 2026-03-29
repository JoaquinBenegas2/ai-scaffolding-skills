# Basic Skill Creation Guide

Use this guide when the workflow needs to create a new skill but `skill-creator` is not installed or the user does not want to install it.

## Goal

Create a minimal but solid skill that is immediately usable and easy to refine later.

## Minimum folder structure

```text
.agents/
  skills/
    <skill-name>/
      SKILL.md
      references/   # optional
      scripts/      # optional
      assets/       # optional
      evals/        # optional
```

## Minimum SKILL.md requirements

Every skill must include frontmatter:

```yaml
---
name: skill-name
description: Explain what the skill does and when to use it. Include trigger phrases.
---
```

## Writing rules

1. Keep one clear purpose per skill.
2. Make the description triggerable:
   - what it does,
   - when to use it,
   - nearby user phrasings.
3. Keep `SKILL.md` concise and operational.
4. Move large supporting material into `references/`.
5. Include a simple workflow and output contract.

## Recommended body shape

```markdown
# Skill Name

One-sentence purpose.

## Goal

What successful use of the skill should produce.

## Workflow

### Step 1 - Discovery
What to inspect first.

### Step 2 - Questions
Ask only what cannot be inferred.

### Step 3 - Execution
How to do the core work.

## Output contract

What to return or create.

## Anti-patterns

What to avoid.
```

## Naming rules

- Use kebab-case.
- Match folder name and `name` field exactly.
- Prefer specific names over broad generic names.

## Good enough quality bar

- The skill can run independently.
- The description makes triggering likely.
- The workflow is explicit.
- The output is concrete.
- The skill can be refined later without rethinking the whole structure.
