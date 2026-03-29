# Contributing

Thanks for contributing to `ai-scaffolding-skills`.

This repository is meant to evolve over time. Contributions can:

- add new skills to the suite,
- improve or refine existing skills,
- expand references, examples, or evals,
- improve documentation and installation guidance.

## What you can contribute

Typical contributions include:

- a new skill under `skills/<skill-name>/`
- improvements to `ai-scaffolding`
- improvements to companion skills like `agents-md-generator`, `tech-rules-generator`, `tech-skill-installer`, or `skills-to-subagents`
- better references, templates, or fallback guides
- README or docs improvements

## Recommended workflow

1. Fork the repository.
2. Create a new branch for your work.
3. Make your changes.
4. Test locally.
5. Open a pull request.

## Contribution guidelines

### Adding a new skill

Create a new folder:

```text
skills/<skill-name>/
  SKILL.md
  references/
  evals/
  scripts/
  assets/
```

Minimum requirements:

- folder name in kebab-case
- `SKILL.md` with valid frontmatter
- clear `name`
- clear `description` explaining when to use the skill
- concise workflow and output contract

Try to keep the skill independently usable, even if it is also part of the larger suite.

### Modifying an existing skill

When updating a current skill:

- preserve its main purpose unless the change is intentional and documented
- avoid adding hidden external dependencies
- if a skill needs another external skill, make that dependency explicit
- if installation is required, the skill should not silently install it unless that behavior is explicitly part of the design
- keep the shared context behavior consistent with `.agents/ai-scaffolding-context.md` when relevant

### Documentation changes

If you add or materially change a skill:

- update `README.md`
- mention new behavior, dependencies, or recommendations clearly
- add or update references if the workflow depends on them

## Local testing

Before opening a pull request, test your changes locally.

Useful checks:

```bash
npx skills add . --list
npx skills add . --skill '*'
```

If you changed only one skill, also test that skill directly if possible.

Recommended manual checks:

- confirm the skill is detected correctly
- confirm `name` matches the folder name
- confirm descriptions are clear and triggerable
- confirm references and examples still make sense
- confirm dependency handling is explicit and safe

## Pull requests

When opening a pull request:

- explain what changed
- explain why the change is useful
- mention whether you added a new skill or modified an existing one
- mention any dependency or behavior changes

Small, focused pull requests are preferred over very large mixed changes.

## Design principles for this repo

Please try to preserve these principles:

- each skill should be useful on its own
- `ai-scaffolding` should remain the main orchestrator
- shared context should flow through `.agents/ai-scaffolding-context.md`
- external skill dependencies should be explicit
- the suite should fail safely and ask the user before risky or surprising installs

## Questions

If something is unclear, open an issue or describe the uncertainty in your pull request.
