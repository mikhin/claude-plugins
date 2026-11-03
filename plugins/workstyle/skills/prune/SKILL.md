---
name: prune
description: Tidy git in the project repos — prune, drop merged branches and stray worktrees, fast-forward to the active release branch. Never touches unmerged work. Triggers — "/prune", "clean up git", "tidy the repos".
allowed-tools: Bash(git:*), Bash(gh:*), Bash(test:*), Bash(cat:*), mcp__linear__get_issue
---

Repos: the current one, plus any the user names. Short names for the repos and their clones are in `~/.claude/workstyle-context.md`.

Tidy means merged things only. Deleting anything unmerged needs the user's yes.

## Never

- `git stash`, in any form. The user may edit from an IDE during the session. List stashes, never touch them.
- Touch uncommitted changes. A dirty tree blocks `switch` and `pull` in that repo, not the rest.
- Delete a `release/*` branch, `main`, a branch with an open PR, or a teammate's remote branch.
- Push, except the remote delete in "Ask".

## Steps, per repo

1. `git status --porcelain`, `git worktree list`, `git stash list`.
2. `git fetch --prune`.
3. Bases: `origin/main` and every `origin/release/*`. The active base is the newest release branch by commit date, or `main` when none is newer.
4. Each local branch except the bases is merged when either holds:
   - `git merge-base --is-ancestor <b> <base>` for some base;
   - `gh pr list --head <b> --state merged --json headRefOid` returns its local SHA (a squash merge).

   Merged and no open PR (`gh pr list --head <b> --state open`) → delete. Use `git branch -d`, or `-D` for the squash case.
5. Clean tree → `git switch <active base>` and `git pull --ff-only`. Other local bases behind origin → `git fetch origin <b>:<b>`.
6. A worktree on a deleted branch with a clean tree → `git worktree remove`. Then `git worktree prune`.
7. `git gc --prune=now`.
8. A deleted branch whose name holds an issue key (`abc-707-…` → `ABC-707`): `get_issue`. Status still In Progress or In Review → it goes in the report. Never change a status.

## Ask

The user's own branch whose PR was closed unmerged: ask once whether to delete it locally and on origin.

An unmerged branch with no PR, such as `exp/*`, stays without a question. It goes in the report as kept.

## Report

One block per repo: what was deleted, which branch is checked out now, what stayed and why, which issues to move. Use plain words: "origin has 3 new commits", not "behind 3".
