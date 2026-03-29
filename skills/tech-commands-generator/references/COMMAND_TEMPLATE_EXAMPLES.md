# Command Template Examples

These examples show the expected style for files created under `.agents/commands/`.

The exact wording can change by repo, but the structure should stay concrete and repo-aware.

## Example: `commit.md`

```md
---
description: Create a commit using repo-aware style
subtask: true
---

Review the current Git changes and create a commit.

Before committing:
1. Inspect `git status`.
2. Inspect staged and unstaged diffs.
3. Inspect recent commit messages to detect local style.
4. Prefer Conventional Commits unless the repo shows a stronger established convention.

Then:
1. Stage relevant files if needed.
2. Draft a concise commit message that explains the purpose of the change.
3. Create the commit safely without destructive Git operations.

Output:
- final commit message,
- short summary of staged changes,
- any assumptions or blockers.
```

## Example: `pr.md`

```md
---
description: Create a pull request using repo conventions
subtask: true
---

Prepare and create a pull request for the current branch.

Before drafting the PR:
1. Inspect git status and branch tracking state.
2. Inspect commits and diff against the base branch.
3. Inspect `.github` PR templates, contributing docs, and any detectable PR conventions.

Then:
1. Push the branch if needed.
2. Draft the PR title and body using project conventions.
3. If no convention exists, use a conventional structure with title, summary, context, and validation notes.

Output:
- PR title,
- PR body,
- PR URL if created,
- assumptions or missing conventions.
```

## Example: `merge-from-base.md`

```md
---
description: Merge the base branch into the current branch
subtask: true
---

Update the detected base branch, merge it into the current branch, and resolve conflicts carefully.

Workflow:
1. Detect the current branch and the base branch convention.
2. Fetch remotes and update the base branch.
3. Merge the updated base branch into the current branch using the repo-aligned strategy.
4. Resolve conflicts conservatively and keep behavior intact.

Finish with a very detailed report:
- files with conflicts,
- how each conflict was resolved,
- relevant before/after diffs,
- any residual risk.
```

## Example: `lint.md`

```md
---
description: Run the project's lint workflow and summarize issues
subtask: true
---

Run the repository's real lint workflow.

Before acting:
1. Detect the package manager and lint entrypoint from repo files.
2. Prefer existing scripts over invented commands.

Then:
1. Run lint.
2. Summarize failures by file and rule.
3. If safe and requested, apply straightforward fixes.

Output:
- command executed,
- result,
- categorized issues,
- suggested next step.
```

## Example: `typecheck.md`

```md
---
description: Run the project's typechecking workflow
subtask: true
---

Run the repository's real typechecking workflow and summarize the result.

Before acting:
1. Confirm TypeScript or another static typing tool is actually present.
2. Detect the real typecheck script or fallback command used by the repo.

Then:
1. Run the typecheck command.
2. Group errors by file and root cause.
3. Suggest the smallest safe fix path.

Output:
- command executed,
- pass/fail result,
- grouped errors,
- recommended follow-up.
```
