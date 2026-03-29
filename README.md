# ai-scaffolding-skills

Portable skill suite for bootstrapping and evolving an AI scaffolding inside a software project.

The suite is designed so each skill can run independently, but the ideal workflow is to install them all and let `ai-scaffolding` orchestrate the full process from one entry point.

## Goal

These skills help you:

- Discover the real state of a repository,
- Create or refine `AGENTS.md`,
- Generate modular tech rules in `.agents/rules/`,
- Generate reusable project commands in `.agents/commands/`,
- Discover and install supporting skills for the project stack,
- Design and create subagents from the installed/local skill set,
- Keep all decisions in a shared context file: `.agents/ai-scaffolding-context.md`.

## Included skills

This repo contains these project-owned skills:

- `ai-scaffolding`
- `agents-md-generator`
- `tech-rules-generator`
- `tech-commands-generator`
- `tech-skill-installer`
- `skills-to-subagents`

## Recommended installation strategy

The recommended setup is to install the full suite:

```bash
npx skills add JoaquinBenegas2/ai-scaffolding-skills --skill *
```

That gives `ai-scaffolding` everything it needs for the normal orchestration flow.

If you want to choose skills interactively from the terminal toggle UI:

```bash
npx skills add JoaquinBenegas2/ai-scaffolding-skills
```

You can still install a single skill and use it independently, for example:

```bash
npx skills add JoaquinBenegas2/ai-scaffolding-skills --skill ai-scaffolding
```

## Recommended companion skills

These are not included in this repository, but they are recommended:

- `find-skills`
  - used by `tech-skill-installer`
  - if it is missing, the workflow must ask the user before installing it
- `skill-creator`
  - optional but recommended when the installer detects missing custom skills
  - if it is missing, `ai-scaffolding` asks the user whether to install it
  - if the user declines, `ai-scaffolding` falls back to a basic manual skill creation path

## How the suite works

High level behavior:

- Every skill can run on its own.
- `ai-scaffolding` is the orchestrator skill.
- It explores the repo first and persists shared state in `.agents/ai-scaffolding-context.md`.
- Companion skills read that file first when it exists, reuse its answers, and only ask for missing information.
- `ai-scaffolding` validates required companion skills, runs `agents-md-generator` -> `tech-rules-generator` -> `tech-commands-generator` -> `tech-skill-installer` -> `skills-to-subagents`, handles missing dependencies, and updates the context after every stage.
- `tech-skill-installer` is for project skills, not workflow companion validation: it must install useful project skills or emit explicit gap lines that trigger the custom-skill path.
- `tech-skill-installer` must keep the default `.agents/` install target, avoid environment-specific targets like Windsurf/Claude/Codex/Kiro, and keep the installed bundle between 3 and 15 skills unless it explicitly documents why it could not reach the minimum.
- `tech-skill-installer` must also create `.agents/installed-skills-summary.md` with only a title and a table listing each installed skill, its objective, why it was installed, and the repository link for manual review.

## Flow diagram

```mermaid
flowchart TD
    U[User request] --> A[ai-scaffolding]
    A --> B[Explore repo and existing .agents state]
    B --> C[Create or update .agents/ai-scaffolding-context.md]
    C --> D[agents-md-generator]
    D --> D1[Create or refine AGENTS.md]
    D1 --> E[tech-rules-generator]
    E --> E1[Create .agents/rules/<tech>/...]
    E1 --> F[tech-commands-generator]
    F --> F1[Create .agents/commands/*.md]
    F1 --> G[tech-skill-installer]
    G --> G1{find-skills available?}
    G1 -->|No| G2[Ask user before installing find-skills]
    G1 -->|Yes| G3[Discover and install useful skills]
    G2 --> G3
    G3 --> H{Custom skills needed?}
    H -->|No| I[skills-to-subagents]
    H -->|Yes| J{Install skill-creator?}
    J -->|Yes| J1[Install and use skill-creator]
    J -->|No| J2[Create skills with basic fallback guide]
    J1 --> I
    J2 --> I
    I --> I1[Create .agents/agents/*.md]
    I1 --> Z[Final report and updated shared context]
```

## Dependency policy

The suite is intentionally conservative with external skill dependencies:

- No external skill should be installed silently.
- `find-skills` requires explicit user approval before installation.
- `skill-creator` requires explicit user approval before installation.
- If `skill-creator` is not installed, the workflow still completes using the built-in fallback guidance.
- A completed installer stage must show installed project skills or explicit uncovered gaps; companion-skill validation alone is not enough.

## Structure

```text
ai-scaffolding-skills/
  skills/
    ai-scaffolding/
    agents-md-generator/
    tech-rules-generator/
    tech-commands-generator/
    tech-skill-installer/
    skills-to-subagents/
```

Each skill may include:

```text
<skill-name>/
  SKILL.md
  references/
  scripts/
  assets/
```

## Install with npx skills

List available skills:

```bash
npx skills add JoaquinBenegas2/ai-scaffolding-skills --list
```

Install all skills from this repo:

```bash
npx skills add JoaquinBenegas2/ai-scaffolding-skills --skill *
```

Install by selecting the skills you want in the terminal UI:

```bash
npx skills add JoaquinBenegas2/ai-scaffolding-skills
```

Install one skill:

```bash
npx skills add JoaquinBenegas2/ai-scaffolding-skills --skill ai-scaffolding
```

## Suggested usage

Best entry point:

```text
/ai-scaffolding bootstrap this project for Java
```

Independent usage examples:

```text
/agents-md-generator
/tech-rules-generator Java
/tech-commands-generator React Git
/tech-skill-installer React frontend
/skills-to-subagents verification workflow
```

## Contributing

Contributions are welcome.

- See `CONTRIBUTING.md` for the recommended workflow to add new skills or improve existing ones.
- The project is released under the MIT license in `LICENSE`.
