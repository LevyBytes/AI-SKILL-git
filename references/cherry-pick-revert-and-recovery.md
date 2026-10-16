# Cherry-pick, revert and recovery

Open when replaying commits, reverting shared history or recovering a commit through the reflog.

## Copying and undoing commits

- `git cherry-pick <commit>...` replays the changes of specific commits as new commits on the current
  branch; accepts ranges `A..B`; `-x` appends a "(cherry picked from commit …)" line; `-n`
  applies without committing; `-m <parent-number>` cherry-picks a merge.
- `git revert <commit>...` makes a **new** commit that undoes an earlier one — the safe choice for
  shared history. Reverting a merge needs `-m <parent-number>` to pick the mainline parent.

```
git revert HEAD          # safe undo of the last commit
git cherry-pick abc123   # bring one commit to this branch
```

Both use the resolve → `git add` → `--continue` flow on conflict and share `--abort` / `--quit` /
`--skip`.

## Recovering with reflog

`git reflog` lists where `HEAD` and branches have pointed (`HEAD@{<n>}`, `HEAD@{<date>}`). After a bad
reset or rebase, find the prior position and return to it.

```
git reflog
git branch recovered 'HEAD@{1}'  # only after verifying this is the wanted commit
```

Entries live in `.git/logs/` and expire per `gc.reflogExpire` (default 90 days) /
`gc.reflogExpireUnreachable` (default 30 days). See [recovery-and-maintenance](recovery-and-maintenance.md).

## Operational cautions

- **Don't** rebase or `--amend` commits already pushed to a shared branch — use `git revert`.
- **Don't** confuse the reset modes: `--soft` keeps everything staged, `--hard` discards the working
  tree.
- **Do** inspect `reflog` and `ORIG_HEAD` promptly; expired/pruned commits or discarded
  uncommitted changes may be unrecoverable.

- **Do** clean up local history with interactive rebase before sharing.
- **Do** choose the mainline parent deliberately for `git revert -m <parent-number>`;
  parent 1 is not universal, and reverting a merge affects later merge results.
- **Don't** use `git reset --hard` with uncommitted work you might still want.

## Related

[snapshotting](snapshotting.md) · [branching-and-merging](branching-and-merging.md) · [recovery-and-maintenance](recovery-and-maintenance.md) ·
[inspection-and-comparison](inspection-and-comparison.md)

See the [topic index](rewriting-history.md) and [related operations](amend-reset-and-rebase.md) for surrounding context.
