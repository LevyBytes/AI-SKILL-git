# Amend, reset and rebase

Open when amending local commits, choosing a reset mode or rebasing unshared history.

Changing commits that already exist: amending, resetting, rebasing, and replaying. Covers
`git commit --amend`, `git reset` (commit forms), `git rebase` (including interactive),
`git cherry-pick`, `git revert`, and `git reflog` for recovery.

**When this applies:** fixing the last commit, moving a branch pointer, reordering/squashing commits,
copying commits between branches, undoing a commit safely, or recovering after a mistake.

## The golden rule

Rewriting **published** history forces everyone else to repair theirs. Amend, reset, and rebase
freely on local/unshared commits; for commits already pushed to a shared branch, prefer `git revert`,
which records a new undo commit instead of altering the past.

## Amend the last commit

`git commit --amend` replaces the tip commit with the staged content and (optionally) a new message;
`--no-edit` keeps the existing message.

```
git add forgotten.c
git commit --amend --no-edit
```

## Moving HEAD with reset

`git reset [<mode>] <commit>` moves the current branch and conditionally touches the index and
working tree:

- `--soft`: move `HEAD` only; index and working tree **unchanged** (changes stay staged).
- `--mixed` (default): move `HEAD` and reset the index; working tree **unchanged** (changes become
  unstaged).
- `--hard` — move `HEAD`, index, **and** working tree (discards changes — destructive).

`ORIG_HEAD` records the pre-reset commit; it does not recover discarded uncommitted edits.
Later operations can overwrite it. Preserve a recovery branch before further history changes.

```
git reset --soft HEAD~1   # undo last commit, keep changes staged
```

## Rebasing

`git rebase <upstream>` reapplies your commits on top of another base; `--onto <newbase>` transplants
a range; `--keep-base` rebases onto the merge base. Resolve conflicts then `git rebase --continue`
(or `--skip` / `--abort`). `--autostash` shelves dirty changes around the operation.

Interactive rebase (`-i`) opens a todo list whose verbs reshape history: `pick`, `reword`, `edit`,
`squash`, `fixup`, `drop`, `exec`, and `break`. Pair `--autosquash` with `git commit --fixup` /
`--squash` to mark commits for automatic folding.

```
git rebase -i HEAD~5
```

See the [topic index](rewriting-history.md) and [related operations](cherry-pick-revert-and-recovery.md) for surrounding context.
