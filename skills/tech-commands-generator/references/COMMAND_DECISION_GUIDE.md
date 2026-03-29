# Command Decision Guide

Use this guide when it is unclear whether the user needs a command, a rule, a skill, or no artifact at all.

## Fast test

Create a command when the answer to most of these is yes:

1. Will the user likely run this more than once?
2. Is this something the user should invoke manually?
3. Does the workflow benefit from repo-aware context gathering?
4. Can it be given a short name like `/commit` or `/typecheck`?
5. Would a stable prompt reduce mistakes or save time?

If the answer is mostly no, do not create a command.

## Decision matrix

### Create a command

Choose a command when the request is a repeated operational workflow.

Examples:

- create a repo-aware commit
- open a PR using project conventions
- run lint plus summarize issues
- update the current branch from the base branch

### Create or update rules

Choose rules when the behavior should apply all the time rather than through manual invocation.

Examples:

- always prefer Conventional Commits
- never use force push on protected branches
- every React component must follow a team structure

### Create or use a skill

Choose a skill when the job needs a broader reusable workflow, substantial discovery, or multiple outputs beyond a single named command.

Examples:

- scaffold a full `.agents/` structure
- install multiple stack-specific skills
- analyze a project and generate rules, skills, and subagents together

### Create nothing

Choose no artifact when the request is one-off, trivial, or unclear.

Examples:

- explain what a pull request is
- help with one specific failing test right now
- answer a conceptual Git question

## Heuristics for Git commands

Git commands are usually good candidates because they are frequent, manual, and benefit from project context.

Good Git command patterns:

- inspect status/diff/log before acting,
- respect branch conventions,
- inspect PR templates before drafting,
- explain actions and outcomes clearly.

Bad Git command patterns:

- skip repo inspection,
- hardcode destructive operations,
- assume `develop` exists everywhere,
- generate commands that differ only by tiny wording changes.

## Heuristics for stack-specific commands

Add stack-specific commands when at least one of these is true:

1. The user explicitly asked for that stack.
2. The repo already has scripts or config proving the workflow exists.
3. The workflow is common enough to justify a reusable prompt.

Do not add stack-specific commands just because a framework is present.

Examples:

- JavaScript/TypeScript + lint/test scripts present: `/lint`, `/test`, `/typecheck`
- React + ESLint + TypeScript present: `/lint`, `/typecheck`, maybe `/test`
- Next.js + test/lint scripts present: `/lint`, `/test`, `/typecheck`, maybe `/analyze-bundle`
- Angular workspace present: `/lint`, `/test`, `/build`, `/typecheck`
- Python + Ruff + pytest present: `/ruff`, `/test`
- Go with `go test` and lint tooling present: `/test`, `/lint`, maybe `/vet`
- Java with Maven only: maybe `/test` and `/mvn-verify`, not a speculative `/spotbugs` if SpotBugs is absent
- .NET with SDK-style projects: `/test`, `/build`, `/format`, maybe `/analyze`

## Naming rules

Prefer short names without redundant tech prefixes.

Good:

- `commit`
- `pr`
- `lint`
- `typecheck`

Use prefixes only when they avoid ambiguity.

Examples:

- `api-test`
- `react-lint`
- `backend-build`

## Pack size rule

Default target:

- Git-only request: 4-6 commands
- Git + one stack: 6-10 commands total

If you feel pressure to create many more, stop and justify each addition.
