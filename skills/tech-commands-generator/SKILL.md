---
name: tech-commands-generator
description: Generate or expand a reusable slash-command pack under `.agents/commands/` for repeated manual AI workflows. Use this proactively when the user asks for basic commands, Git commands, or stack-specific commands for Java, Go, Python, JavaScript/TypeScript, React, Next.js, Angular, or .NET. Always inspect repo conventions first, reuse `.agents/ai-scaffolding-context.md` when present, include Git commands by default unless the user opts out, and only create commands that represent repeated high-value workflows rather than one-off tasks or permanent rules.
---

# Tech Commands Generator

Create a lean, reusable command pack for manual invocation by the user.

## Goal

When the user asks for commands, this skill must:

1. Determine whether the request really needs commands.
2. Create or update `.agents/commands/` as the canonical project command pack.
3. Include Git commands by default unless the user explicitly opts out.
4. Add stack-specific commands only when they are justified by repo evidence or the user's request.
5. Keep the command set small, reusable, and grounded in real workflows.

## What a command is

A command is a manually invoked prompt shortcut for a repeated workflow.

A good command:

- represents a recurring task the user is likely to run many times,
- has a clear intent and expected outcome,
- benefits from stable project-specific guidance,
- can be named cleanly as a slash command,
- saves the user from rewriting the same prompt over and over.

A command is not:

- a permanent rule that should live in `AGENTS.md` or `.agents/rules/`,
- a full autonomous skill with large discovery/orchestration scope,
- a one-off task the user probably will not repeat,
- a thin clone of a built-in command with no project-specific value.

Use `references/COMMAND_DECISION_GUIDE.md` when the boundary is unclear.

## Output layout

Create or update this structure:

```text
.agents/
  commands/
    README.md
    <command-name>.md
    <command-name>.md
    ...
```

Use flat filenames by default so the filename maps cleanly to the slash command name.

Examples:

- `commit.md` -> `/commit`
- `commit-push.md` -> `/commit-push`
- `pr.md` -> `/pr`
- `lint.md` -> `/lint`
- `typecheck.md` -> `/typecheck`

Only introduce prefixes like `react-` or `api-` when they prevent ambiguity or collisions.

Always create or refresh `.agents/commands/README.md` with:

1. command list,
2. one-line purpose for each command,
3. arguments if any,
4. why each command exists,
5. assumptions and repo evidence used.

## Discovery workflow

Before writing commands:

1. Read root `AGENTS.md` if present.
2. Read `.agents/ai-scaffolding-context.md` if it exists.
3. Inspect existing `.agents/commands/` to avoid duplicates or conflicting names.
4. Inspect `.agents/rules/` and relevant skills if they already constrain workflow.
5. Inspect repo evidence for actual repeated tasks:
   - git history and branching conventions,
   - `.github/` PR templates and contributing docs,
   - package scripts,
   - build files,
   - CI workflows,
   - test/lint/typecheck entrypoints,
   - framework/toolchain files.
6. Detect the requested technology from the user prompt and repository evidence.

If `.agents/ai-scaffolding-context.md` exists:

1. Reuse its stack, constraints, accepted assumptions, and workflow conventions first.
2. Treat it as the primary source for prior user answers.
3. Only ask questions for true gaps or contradictions.

Ask only focused questions that materially change the command pack.

Question budget:

- If repo evidence is strong: ask at most 2 focused questions.
- If repo evidence is weak or the project is greenfield: ask at most 4 focused questions.

Good questions are about:

- target stack when ambiguous,
- whether Git commands should be omitted,
- base branch convention when it cannot be inferred,
- preferred package manager or task runner when scripts are unclear.

If key details are missing after that, continue with explicit assumptions and document them in the README.

## Decision policy: when to create commands

Create a command only when most of the following are true:

1. The workflow is repeated or likely to be repeated.
2. The workflow is primarily user-invoked rather than always-on policy.
3. The workflow needs project context to do well.
4. The workflow can be expressed as a clear prompt with a stable name.
5. The workflow saves meaningful time or avoids common mistakes.

Do not create a command when any of these dominate:

1. The request is really asking for conventions, not a reusable prompt.
2. The behavior belongs in `AGENTS.md`, rules, or another skill.
3. The command would encode unsupported or imaginary repo workflows.
4. The command would duplicate an existing command without adding value.
5. The task is trivial enough that a named command would be noise.

Prefer 4-10 commands total for a focused request. More than that usually means the pack is bloated.

## Mandatory Git baseline

Unless the user explicitly says not to, include a Git baseline.

Default Git baseline:

1. `/commit`
2. `/commit-push`
3. `/pr`
4. `/commit-push-pr`
5. `/update-branch`
6. `/merge-from-<base-branch>`

Naming rule for the merge command:

- If the repository clearly uses `develop`, name it `/merge-from-develop`.
- Otherwise prefer `/merge-from-<detected-base-branch>`.
- If the base branch cannot be inferred safely, use `/merge-from-base` and document the assumption.

Git command requirements:

- `/commit` must inspect recent commit style, but prefer Conventional Commits when there is no strong contrary convention.
- `/commit-push` must do the same, then sync safely with the tracked remote before pushing.
- `/pr` must inspect `.github` templates and project conventions first; if nothing exists, use a clean conventional PR structure.
- `/commit-push-pr` must chain the above responsibly without skipping repo checks.
- `/update-branch` must bring the current branch up to date from the detected base branch using the safest repo-aligned strategy.
- `/merge-from-<base-branch>` must update the base branch first, merge it into the current branch, resolve conflicts carefully, and finish with a very detailed conflict summary that includes relevant diffs.

Never encode force-push, hard reset, or destructive Git behavior unless the user explicitly asked for it.

## Stack-specific expansion policy

After the Git baseline, add stack-specific commands only when they are justified.

Examples:

- JavaScript/TypeScript: `/lint`, `/test`, `/typecheck`
- React: `/lint`, `/test`, `/typecheck`, `/component-review`
- Next.js: `/lint`, `/test`, `/typecheck`, `/analyze-bundle`
- Angular: `/lint`, `/test`, `/build`, `/typecheck`
- Java: `/test`, `/spotbugs`, `/checkstyle`, `/mvn-verify`
- Go: `/test`, `/lint`, `/vet`, `/mod-tidy`
- Python: `/test`, `/ruff`, `/mypy`, `/format`
- .NET: `/test`, `/build`, `/format`, `/analyze`

Selection rules:

1. Prefer commands backed by real scripts or real tooling already present in the repo.
2. Prefer commands for high-friction workflows such as linting, testing, typechecking, release prep, or refactor review.
3. Do not invent commands only because a technology is present.
4. If the user explicitly asks for a stack, add 2-5 useful commands for that stack when justified.
5. If the repo lacks evidence for a likely command, either skip it or create it with a clearly stated assumption.

Examples of good stack-specific choices for React:

- `/lint` when lint scripts or ESLint config exist.
- `/typecheck` when TypeScript exists.
- `/test` when a test runner exists.
- `/lint-ts` only when there is a real combined lint-and-types workflow worth naming.

## Technology coverage policy

Support at least these targets with adapted command packs:

- Git workflows
- Java
- Go
- Python
- JavaScript/TypeScript
- React
- Next.js
- Angular
- .NET (C#)

If the user asks for another technology, derive a comparable command pack using the same decision policy and quality gates.

## Command authoring standard

Commands in `.agents/commands/` should be Markdown files with lightweight frontmatter and a prompt body.

Use this shape by default:

```md
---
description: Short command summary
agent: optional-existing-agent
subtask: true
---

Goal: what this command should accomplish.

Before acting, inspect the relevant repo context.

Follow this workflow:
1. ...
2. ...
3. ...

Output:
- ...
- ...
```

Why this shape:

- It stays readable in `.agents/commands/`.
- It remains close to the documented OpenCode Markdown command format.
- It is easy to adapt later to native command directories in other hosts.

Authoring rules:

1. Make the command body concrete and execution-oriented.
2. Explain why key checks matter instead of relying on rigid phrasing.
3. Use `$ARGUMENTS`, `$1`, `$2`, etc. only when arguments materially help reuse.
4. Use `@file` references only for stable, known paths.
5. Use shell injection syntax only when it clearly improves repeatability and stays low-risk.
6. End with an explicit output/report contract so the command produces consistent results.

Use `references/COMMAND_TEMPLATE_EXAMPLES.md` for concrete patterns.

## Quality gates

Before finalizing, check:

1. Every command has a distinct purpose.
2. Command names are short, memorable, and non-overlapping.
3. The pack includes Git by default unless the user opted out.
4. Stack-specific commands are grounded in repo evidence or explicit user request.
5. `.agents/commands/README.md` matches the actual files created.
6. No command contradicts `AGENTS.md`, rules, or existing project workflow.
7. No command silently assumes dangerous Git behavior.

## Output contract to user

Report:

1. Why commands were appropriate for this request.
2. Which commands were created or updated.
3. Which Git baseline commands were included or intentionally omitted.
4. Which stack-specific commands were added and why.
5. Which conventions were detected from the repo.
6. Which assumptions were made.
7. Which commands were intentionally not created and why.

If `.agents/ai-scaffolding-context.md` exists, update it with:

- command files created or updated,
- Git baseline decisions,
- stack-specific command rationale,
- open gaps or assumptions.

## Anti-patterns

- Creating a huge command dump with weak justification.
- Converting permanent project policy into manual commands.
- Inventing stack commands with no evidence they are useful.
- Duplicating built-in commands with no project-specific behavior.
- Hardcoding `develop` if the repo does not actually use it.
- Writing vague prompts that do not inspect repo context first.

## Internal references

- `references/COMMAND_DECISION_GUIDE.md`
- `references/COMMAND_TEMPLATE_EXAMPLES.md`

Use these bundled references to keep the command pack lean, reusable, and aligned with the rest of the scaffolding workflow.
