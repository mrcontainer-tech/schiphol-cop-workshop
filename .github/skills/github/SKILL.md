---
name: github
description: Branch, commit, and open pull requests, and work with issues, using the gh CLI following this repo's conventions. Use when work on a ticket is implemented and tested, or when the user asks to create a branch, open/inspect a PR, check PR status, or view/create/comment on GitHub issues.
---

# GitHub via gh

Ship reviewed work from ticket to pull request without leaving the terminal.

## Preconditions - check before opening a PR

1. `pytest` is green.
2. The manual repro/demo command from the ticket shows the intended behavior.
3. `git diff` has been shown to and confirmed by the user.
4. You are NOT on `main` (create the branch first if needed).

## Branch → commit → PR

```bash
# 1. Branch from main, named after the ticket
git checkout -b <ticket-key>-<slug>        # e.g. ops-1-destination-filter

# 2. Stage and commit with a conventional title + ticket key
git add -A
git commit -m "fix: match destination filter case-insensitively (OPS-1)"

# 3. Push and open the PR
git push -u origin <branch>
gh pr create --title "OPS-1: Destination filter misses every flight" --body-file /tmp/pr-body.md
```

Commit types: `feat:` new behavior · `fix:` bug fix · `test:` tests only · `docs:` docs/ADRs · `chore:` plumbing.

## PR body

Follow [`.github/pull_request_template.md`](../../pull_request_template.md): a `Closes <KEY>` line, a 1-2 sentence summary, the ticket's acceptance criteria as checked boxes, and a test plan (pytest + the manual command you ran). Write the body to a temp file and pass `--body-file` - don't fight shell quoting.

## Inspecting

| Action | Command |
|--------|---------|
| PR status of current branch | `gh pr status` |
| View a PR | `gh pr view <number>` (add `--web` to open the browser) |
| PR diff | `gh pr diff <number>` |
| List open PRs | `gh pr list` |
| Check CI | `gh pr checks <number>` |

## Issues

Jira is the source of truth for tickets (see the **jira** skill). Use GitHub issues only when the user explicitly asks to work with them - for example tracking repo-specific bugs or tasks that live on GitHub.

| Action | Command |
|--------|---------|
| List open issues | `gh issue list` |
| Filter issues | `gh issue list --label bug --assignee @me` |
| View an issue | `gh issue view <number>` (add `--web` to open the browser) |
| View with comments | `gh issue view <number> --comments` |
| Create an issue | `gh issue create --title "..." --body-file /tmp/issue-body.md` |
| Comment on an issue | `gh issue comment <number> --body-file /tmp/comment.md` |
| Close an issue | `gh issue close <number>` |
| Reopen an issue | `gh issue reopen <number>` |

Write issue and comment bodies to a temp file and pass `--body-file` - don't fight shell quoting. Link a PR to an issue with a `Closes #<number>` line in the PR body so merging auto-closes it.

## Rules

- One ticket = one branch = one PR. Don't bundle tickets.
- Never `git push --force`. Never commit directly to `main`.
- Never merge or close a PR unless the user explicitly asks.
- Never close or reopen an issue unless the user explicitly asks.
- `gh` not authenticated → tell the user to run `gh auth login` themselves.
