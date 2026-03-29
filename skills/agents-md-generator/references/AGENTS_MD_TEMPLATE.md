# AGENTS.md Template (Lean + Practical)

Use this template as a baseline, then adapt it to repository evidence.

```markdown
# AGENTS

This repository is [one-sentence project purpose].

## Stack and tooling

- Primary stack: [example: Next.js 15 + TypeScript]
- Package/build tool: [pnpm | npm | yarn | maven | gradle | go modules | poetry]
- Key frameworks/libraries: [short list]

## Commands

- Install deps: `[command]`
- Dev: `[command]`
- Build: `[command]`
- Test: `[command]`
- Lint/format: `[command]`
- Typecheck/static analysis: `[command]`

## Working conventions

- Prefer changes that match existing patterns in the touched module.
- Keep changes scoped; avoid broad refactors unless requested.
- Add or update tests when behavior changes.
- Keep docs aligned with behavior when public interfaces change.

## Boundaries

- Always:
  - Run the most relevant checks for modified areas.
  - Preserve backward compatibility unless task explicitly allows breaking changes.
- Ask first:
  - Dependency upgrades, schema migrations, CI pipeline changes, infra changes.
- Never:
  - Commit secrets/credentials.
  - Use destructive repository operations without explicit instruction.

## Progressive disclosure

- Detailed coding conventions: `[docs/path-or-note]`
- Testing strategy: `[docs/path-or-note]`
- Architecture decisions: `[docs/path-or-note]`
```

## Template notes

- Keep root file compact and durable.
- Avoid exhaustive path maps.
- Move long language/framework specifics into dedicated docs.
