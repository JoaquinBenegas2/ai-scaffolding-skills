# Rule File Template

Use this template for each file in `.agents/rules/[tech]/`.

Filename format: `[tech]-<rule>.md`

```markdown
# <Tech> - <Rule Title>

## Intent
Explain the outcome this rule protects and why it matters.

## Apply this
- Actionable instruction 1
- Actionable instruction 2
- Actionable instruction 3

## Avoid this
- Anti-pattern 1
- Anti-pattern 2

## Examples
Good:
- <short good example>

Bad:
- <short bad example>

## Verification checklist
- [ ] Can an engineer verify this rule in code review?
- [ ] Is it aligned with root AGENTS.md?
- [ ] Is it specific (not generic advice)?
```

## Writing rules that scale

- Prefer stable guidance over temporary implementation details.
- Keep each file focused on one concern.
- Keep examples short and practical.
- Remove overlap with other rules.
- If a topic grows too much, split into a new `[tech]-*.md` file.
