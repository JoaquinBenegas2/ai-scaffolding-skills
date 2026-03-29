---
name: tech-skill-installer
description: Discover and install a high-coverage skill bundle tailored to the project technology and architecture (Java, .NET, Python, Go, JavaScript/TypeScript, React, Angular, and more). Use this skill proactively when the user asks to install skills for a stack, frontend/backend, monolith/microservices, testing, architecture, or best practices, even if they do not mention "find-skills". Always read AGENTS.md, `.agents/rules`, and `.agents/ai-scaffolding-context.md` first when available, infer needs, ask only missing questions, verify that `find-skills` is available, and consult the user before installing `find-skills` if it is missing.
---

# Tech Skill Installer

Install a high-coverage skill bundle based on the requested stack and real repository context.

## Goal

When the user says something like:

- "Install the skills I need for React"
- "I need skills for Java microservices"
- "Set me up for a .NET backend"

you should:

1. Understand technology and scope (frontend/backend/fullstack, monolith/microservices).
2. Read local context before asking questions.
3. Ask only what cannot be inferred.
4. Build a domain-based search plan.
5. Verify `find-skills` is available.
6. Use `find-skills` to discover concrete skills.
7. Install a coherent set (10-20+ is valid when it adds non-redundant coverage).

## Operating flow

### Execution mode

Before installing, choose mode based on user request and context:

- `plan-only`: search/evaluate/recommend without installing.
- `install`: search and execute installation.

If the user does not specify mode, default to `install`.

### Step 1 - Mandatory local discovery

Before asking questions:

1. Read root `AGENTS.md`.
2. Read `.agents/ai-scaffolding-context.md` if it exists.
3. Inspect `.agents/rules/` for stack, architecture, standards, and constraints.
4. Inspect repository signals (frameworks, tooling, CI, testing).

If `AGENTS.md` is missing, state the limitation and continue with conservative defaults.

If the repo has little or no code (for example, meta-context only), infer from user prompt + `AGENTS.md` + `.agents/rules/` + shared context and avoid over-questioning.

### Step 2 - Build a working hypothesis

Create an initial hypothesis for:

- primary technology,
- application type (frontend/backend/fullstack),
- architecture style (monolith, microservices, modular monolith),
- quality priorities (testing, observability, performance, security, DX),
- repo maturity (greenfield or maintenance).

### Step 3 - Targeted questions (gaps only)

Use `references/TECH_DISCOVERY_QUESTION_BANK.md`.

Do not run long questionnaires by default. Ask only minimal questions that resolve material uncertainty, for example:

- React: state strategy, data fetching, testing level, design system.
- Java: framework (Spring/Quarkus), architecture, integration patterns.
- .NET: ASP.NET style, layering, observability, testing.

Limit: maximum 3 questions per iteration. If uncertainty remains, document assumptions and continue.

Global limit: do not exceed 5 questions across the entire run.

If local context already answers something, do not ask it again.

If `.agents/ai-scaffolding-context.md` exists, record any new user answer there.

### Step 4 - Skill coverage plan

Define a domain coverage matrix:

1. Core stack (main language/framework).
2. Architecture (monolith/microservices/modular).
3. Testing and quality.
4. Data/API/integrations.
5. UI/UX (if applicable).
6. Performance and observability.
7. Security and operations.
8. Productivity/documentation.

Use `references/TECH_SKILL_MAP.md` as the baseline.

### Step 5 - Validate `find-skills` dependency

This skill depends on `find-skills`.

Before running any discovery/install search:

1. Check whether `find-skills` is already available in the current environment or repository skill set.
2. If it is available, continue normally.
3. If it is missing, always consult the user before installing it.

Required behavior when `find-skills` is missing:

- explain briefly that `tech-skill-installer` depends on `find-skills`,
- ask the user whether you should install `find-skills`,
- do not install it silently,
- do not continue with the search phase until the user answers.

If `.agents/ai-scaffolding-context.md` exists, record:

- whether `find-skills` was already available,
- whether the user approved installation,
- how it was installed.

### Step 6 - Search with find-skills

Load and use `find-skills` to research concrete options.

For each domain in the plan:

1. Run specific queries (not generic).
2. Validate skill quality (installs, source, reputation).
3. Avoid duplicates and low-value overlap.

If GitHub CLI/API reputation checks are unavailable, use fallback validation:

1. installs,
2. owner reputation,
3. maintenance/recency signals,

and mark star validation as pending in the final report.

If there is no clear skill for a need, document the gap and continue with nearest alternatives.

When a gap is important enough that the project would benefit from a custom skill, recommend creation explicitly.

Use this exact sentence when applicable:

`I recommend creating skills for: <item 1>, <item 2>, ...`

When gaps remain after search, include this exact line in the final report:

`I could not find skills for: <item 1>, <item 2>, ...`

List every uncovered domain or requirement in that line.

If both conditions are true, include both lines:

- one line for what could not be found,
- one line for what should be created as custom project skills.

### Step 7 - Selection and installation

Select a final bundle by priority:

- **Tier 1 (essential):** stack base + architecture + testing.
- **Tier 2 (recommended):** performance, security, observability, code quality.
- **Tier 3 (contextual):** product/domain-specific needs.

Install skills approved by the plan using `find-skills` commands.

If the user sets no limit, prefer broad but useful coverage (10-20+ is valid if not noisy).

Size and quality rules:

- avoid more than 2 skills per subdomain unless justified,
- avoid bloated bundles with many variants of the same topic,
- apply tier thresholds (see quality policy).

If mode is `plan-only`, do not run install commands; provide ready-to-run commands only.

## Skill quality policy

Recommended adoption thresholds by tier:

- Tier 1: prefer skills with >=300 installs.
- Tier 2: prefer skills with >=100 installs.
- Tier 3: allow >=30 installs only with explicit justification.

If a critical skill is below threshold but fills an important gap, include it only with risk note and in-threshold alternative.

Application rule:

- do not include below-threshold skills by default,
- for exceptions, include in report: reason, risk, and in-threshold alternative.

## Decision policy

Prioritize in this order:

1. Repository `AGENTS.md`.
2. Shared AI scaffolding context.
3. Rules in `.agents/rules/`.
4. Technical evidence in code/tooling.
5. User responses.
6. Stack defaults.

If there is conflict, explain it and choose the option most compatible with the current repository.

## Output contract

Always report:

1. Detected technology and context.
2. Questions asked (if any) and decisions made.
3. Domain-based search plan.
4. Selected skills by tier with short rationale.
5. Installation commands executed.
6. Installed skills and pending failures (if any).
7. Identified gaps and proposed next step.
8. `find-skills` status (already present, user-approved install, or blocked waiting for approval).

If custom skills are advisable, add the exact sentence:

`I recommend creating skills for: ...`

If any gap remains, add the exact sentence:

`I could not find skills for: ...`

If `.agents/ai-scaffolding-context.md` exists, update it with:

- detected stack and architecture,
- skill bundle rationale,
- commands executed,
- installed skills and gaps.

## Anti-patterns to avoid

- Asking broad questions before reading the repo.
- Installing `find-skills` without asking the user first.
- Installing by keyword only without quality checks.
- Picking only 2-3 generic skills and leaving critical gaps.
- Ignoring architecture (microservices vs monolith).
- Installing redundant skills that duplicate value.
- Forcing recommendations that conflict with local rules.
- Re-asking questions already answered in `.agents/ai-scaffolding-context.md`.

## Internal references

- `references/TECH_DISCOVERY_QUESTION_BANK.md`
- `references/TECH_SKILL_MAP.md`

Use these references to keep decisions consistent, high-coverage, and low-noise.
