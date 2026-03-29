# AI Scaffolding Context Template

Use this template for `.agents/ai-scaffolding-context.md`.

```markdown
# AI Scaffolding Context

## Status
- Last updated:
- Project maturity:
- Current stage:

## Target stack
- Primary stack:
- Framework/runtime:
- Package/build tool:
- Architecture style:

## Quality priorities
- Delivery speed:
- Reliability:
- Security:
- Maintainability:
- Observability:

## Constraints
- Hard constraints:
- Soft preferences:
- Things to avoid:

## Repository evidence
- Key files:
- Source layout:
- CI/testing/tooling signals:

## User-provided answers
- Question:
  - Answer:

## Accepted assumptions
- Assumption 1

## External skill dependencies
- find-skills status:
- find-skills approval:
- skill-creator status:
- skill-creator approval:
- dependency notes:

## Stage outputs
### agents-md-generator
- Files:
- Decisions:
- Open issues:

### tech-rules-generator
- Files:
- Decisions:
- Open issues:

### tech-commands-generator
- Files:
- Decisions:
- Open issues:

### tech-skill-installer
- find-skills search commands executed:
- find-skills install commands executed:
- Installation target used:
- Installed skills summary file:
- Installed external project skills:
- Created custom project skills:
- Optional opportunities deferred by cap:
- Missing project-skill coverage:
- Decisions:
- Open issues:
- Recommended new skills:
- Missing external dependencies:

### skills-to-subagents
- Files:
- Decisions:
- Open issues:
```

## Writing rules

- Keep the file compact and easy to update.
- Prefer bullets over long prose.
- Update the file after each stage and after each user answer that materially affects decisions.
- Preserve valid prior context unless the user or repository evidence contradicts it.
- Never collapse companion-skill validation and project-skill installation into the same bullet.
