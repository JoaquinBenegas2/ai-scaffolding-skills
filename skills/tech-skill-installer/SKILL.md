---
name: tech-skill-installer
description: Discover and install a high-coverage skill bundle tailored to the project technology and architecture (Java, .NET, Python, Go, JavaScript/TypeScript, React, Angular, and more). Use this skill proactively when the user asks to install skills for a stack, frontend/backend, monolith/microservices, testing, architecture, or best practices, even if they do not mention "find-skills". Always read AGENTS.md, `.agents/rules`, and `.agents/ai-scaffolding-context.md` first when available, infer needs, ask only missing questions, verify that `find-skills` is available, and consult the user before installing `find-skills` if it is missing.
---

# Tech Skill Installer

Install a high-coverage skill bundle based on the requested stack and real repository context.

This stage installs or recommends project skills for the target repository. It does not exist to validate the ai-scaffolding companion skills.

Primary success condition:

- run real discovery with `find-skills`,
- install external project skills that fit the target stack,
- then identify any remaining gaps that should be closed with custom skills.

Do not present dependency availability as the main outcome.

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
7. Install a coherent set of project skills in the default `.agents/` location.
8. Keep the installed bundle intentionally bounded.

Companion-skill validation boundary:

- Checking that `agents-md-generator`, `tech-rules-generator`, `tech-commands-generator`, `tech-skill-installer`, or `skills-to-subagents` are present does not satisfy this skill.
- Success requires actual `find-skills` search plus one of these outcomes:
  - install external project skills, or
  - if external coverage is insufficient, explicitly trigger custom-skill follow-up for the missing coverage.

## Operating flow

### Execution mode

Before installing, choose mode based on user request and context:

- `plan-only`: search/evaluate/recommend without installing.
- `install`: search and execute installation.

If the user does not specify mode, default to `install`.

### Installation target policy

When running install commands or answering installer prompts:

- keep the default installation target that writes into `.agents/`,
- do not select Windsurf, Claude, Kiro, Codex, Cursor, or any other environment-specific target,
- do not create multiple agent-tool folders for the same skill bundle,
- if the installer offers an environment selection menu, leave it unselected and continue with the default target.

Success for this skill assumes the installed artifacts land under the repository's `.agents/` structure unless the user explicitly asked for a different target.

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

Coverage strategy:

- prefer real, broadly useful external skills over perfect-but-nonexistent niche matches,
- for mainstream stacks like Java/Spring Boot, React, Next.js, Python, Go, and .NET, assume that at least some generic or adjacent skills should exist,
- only escalate to custom-skill recommendations after broad and adjacent external searches have been exhausted.

### Step 5 - Validate `find-skills` dependency

This skill depends on `find-skills`.

Before running any discovery/install search:

1. Check whether `find-skills` is already available in the current environment or repository skill set.
2. If it is available, continue normally.
3. If it is missing, always consult the user before installing it.

If `find-skills` is available, dependency validation is only a prerequisite. You must still execute real search and install work.

Required behavior when `find-skills` is missing:

- explain briefly that `tech-skill-installer` depends on `find-skills`,
- ask the user whether you should install `find-skills`,
- do not install it silently,
- do not continue with the search phase until the user answers.

If `.agents/ai-scaffolding-context.md` exists, record:

- whether `find-skills` was already available,
- whether the user approved installation,
- how it was installed.
- whether companion skills were already present, but keep that separate from project-skill results.

### Step 6 - Search with find-skills

Load and use `find-skills` to research concrete options.

This step requires actual `find-skills` usage. Merely stating that `find-skills` is available is incomplete.

Search order is mandatory:

1. broad stack queries,
2. adjacent capability queries,
3. repo-specific or domain-specific queries.

Do not start from invented skill names.

For each domain in the plan:

1. Run specific queries (not generic).
2. Validate skill quality (installs, source, reputation).
3. Avoid duplicates and low-value overlap.
4. Record the concrete query strings used in the final report and shared context.

Search execution rule:

- record the exact `find-skills` search commands or invocations used,
- do not substitute high-level prose like "skills validated" for actual search evidence,
- if `find-skills` returns candidates, carry those results into selection and installation instead of stopping at the search phase.

Minimum search breadth before declaring zero-install outcome:

- cover at least 3 domains for mainstream stacks,
- include at least one broad stack query and one adjacent capability query for each covered domain,
- include at least one query that is not framework- or vendor-specific when the stack is mainstream.

Selection policy during search:

- prefer the best available approximation for a capability even if it is broader than the repository,
- do not reject a good Java backend, testing, security, or observability skill only because it is not Spring- or Auth0-specific,
- treat exact stack specialization as a bonus, not a requirement.
- prefer installing good external skills first, then use custom skills only for uncovered areas.

If GitHub CLI/API reputation checks are unavailable, use fallback validation:

1. installs,
2. owner reputation,
3. maintenance/recency signals,

and mark star validation as pending in the final report.

If there is no clear skill for a need, document the capability gap and continue with nearest alternatives.

When a gap is important enough that the project would benefit from a custom skill, recommend creation explicitly.

Gap wording rule:

- express gaps as missing capabilities or domains, not as imagined skill package names,
- good: `Spring Boot architecture guidance`, `Auth0 resource server hardening`, `scheduler and email operations`,
- bad: `spring-boot-architecture-specialist`, `auth0-resource-server-hardening`.

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

Bundle size policy for actual installation:

- hard minimum target: 3 installed project skills,
- hard maximum: 15 installed project skills,
- default target range: 3-8 unless the repository clearly benefits from broader coverage,
- if more than 15 good candidates exist, install only the best-fitting 15 and report the rest as optional opportunities,
- if fewer than 3 defensible skills are available, install what passes the quality bar and explicitly report the shortfall and missing coverage.

Install skills approved by the plan using `find-skills` commands.

Do actual installation work in `install` mode. Do not stop after planning unless the user explicitly requested `plan-only`.

Installation rule:

- if `find-skills` produced viable candidates, execute real install commands,
- do not finish with zero external installs unless broad and adjacent candidates were genuinely exhausted or rejected for documented reasons,
- for mainstream stacks, treat at least one external install as the default expectation before escalating to custom-skill creation.

When an install command prompts for target environment or host tool, keep the default `.agents/` target and do not select any tool-specific option.

After installation work in `install` mode, create or update `.agents/installed-skills-summary.md`.

Format requirements for `.agents/installed-skills-summary.md`:

- include only a title and a markdown table,
- do not add prose before or after the table,
- use one row per installed project skill,
- table columns must be: `Skill`, `Objective`, `Why Installed`, `Repository`,
- the `Repository` column must contain the repository link for the installed skill so the user can review it manually later.

If no project skills were installed, still create the file with the title and table header only.

If the user sets no limit, stay within the bundle size policy above.

Size and quality rules:

- avoid more than 2 skills per subdomain unless justified,
- avoid bloated bundles with many variants of the same topic,
- apply tier thresholds (see quality policy).
- prefer coverage breadth over many near-duplicate skills in one domain.

If mode is `plan-only`, do not run install commands; provide ready-to-run commands only.

If mode is `install` and no project skill meets the quality bar or no relevant skill is found:

- do not claim success based only on companion-skill validation,
- treat zero installs on a mainstream stack as exceptional and assume the search was too narrow unless the documented queries show broad and adjacent coverage,
- include the exact line `I could not find skills for: ...`,
- include `I recommend creating skills for: ...` when the missing coverage is important enough to justify a custom skill,
- make the uncovered domains explicit enough for `ai-scaffolding` to trigger the custom-skill path.

If external installs do not reach the minimum useful bundle:

- install the best external skills you did find,
- then recommend custom-skill creation for the remaining high-value coverage,
- do not treat "zero external installs + gap lines" as the preferred outcome for mainstream stacks.

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
4. Concrete `find-skills` queries executed.
5. Exact `find-skills` install commands executed.
6. Installed external project skills by tier with short rationale.
7. Installation target used and confirmation that no environment-specific target was selected.
8. Summary file created or updated at `.agents/installed-skills-summary.md`.
9. Remaining coverage to create as custom skills, if external installs were insufficient.
10. Optional opportunities intentionally not installed because of bundle-size limits.
11. Identified gaps and proposed next step.
12. `find-skills` status (already present, user-approved install, or blocked waiting for approval).

Do not lead the report with companion-skill validation. Lead with what was searched, installed, and still needs creation.

If no project skills were installed, the report must still include at least one of the exact gap lines below. A report with zero installed project skills and no explicit gap lines is incomplete.

If fewer than 3 project skills were installed, explain why the minimum target could not be reached.

If zero project skills were installed for a mainstream stack, explain why each broad or adjacent candidate was rejected and why custom-skill creation is or is not recommended.

If 15 project skills were installed and more viable candidates remained, do not install more; list them as optional opportunities instead.

If custom skills are advisable, add the exact sentence:

`I recommend creating skills for: ...`

If any gap remains, add the exact sentence:

`I could not find skills for: ...`

If `.agents/ai-scaffolding-context.md` exists, update it with:

- detected stack and architecture,
- companion-skill validation result,
- search queries executed,
- installation target used,
- summary file path,
- skill bundle rationale,
- commands executed,
- installed project skills,
- optional opportunities not installed,
- gaps.

## Anti-patterns to avoid

- Asking broad questions before reading the repo.
- Installing `find-skills` without asking the user first.
- Installing by keyword only without quality checks.
- Reporting only companion-skill validation without showing real `find-skills` search and install work.
- Picking only 2-3 generic skills and leaving critical gaps.
- Ignoring architecture (microservices vs monolith).
- Installing redundant skills that duplicate value.
- Selecting Windsurf, Claude, Kiro, Codex, Cursor, or any other environment-specific install target when the default `.agents/` target is available.
- Installing more than 15 project skills by default.
- Omitting the installed skill summary file or filling it with prose instead of the required title-plus-table format.
- Jumping straight to niche repo-specific queries and never trying broad or adjacent capability queries.
- Describing missing coverage as invented skill names instead of capabilities.
- Forcing recommendations that conflict with local rules.
- Re-asking questions already answered in `.agents/ai-scaffolding-context.md`.
- Treating this skill as complete after only validating the workflow companion skills.

## Internal references

- `references/TECH_DISCOVERY_QUESTION_BANK.md`
- `references/TECH_SKILL_MAP.md`

Use these references to keep decisions consistent, high-coverage, and low-noise.
