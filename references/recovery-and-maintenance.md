# Recovery and Maintenance

Repairing mistakes and keeping the object database healthy. Covers `git reflog` for recovery,
`git fsck`, `git gc`, and `git prune`.

**When this applies:** recovering a lost commit/branch after a bad reset/rebase, finding dangling
objects, compacting the repository, or pruning unreachable objects.

## Recovering lost work with reflog

Most "lost" commits are still in the **reflog** — the local history of where `HEAD` and branches have
pointed. Find the prior position and restore it.

```
git reflog                 # e.g. abc123 HEAD@{2}: commit: WIP
git branch recovered 'HEAD@{2}'  # verify the reflog entry before using it
```

`HEAD@{<n>}` and `<branch>@{<n>}` address reflog entries; `HEAD@{<date>}` addresses by time. A bad
reset also leaves the previous tip in `ORIG_HEAD`. Reflog entries live in `.git/logs/` and expire per
`gc.reflogExpire` (default 90 days) and `gc.reflogExpireUnreachable` (default 30 days), so recover
promptly. This can recover retained commits, not guarantee recovery of discarded uncommitted
content. See [rewriting-history](rewriting-history.md).

## Inspecting integrity with fsck

`git fsck` verifies object connectivity and validity and surfaces orphaned objects:

- `--unreachable` lists objects no ref can reach; `--dangling` (default) lists objects nothing points
  to; `--lost-found` writes dangling objects under `.git/lost-found/` so you can inspect them.
- `--connectivity-only` is a faster check; `--name-objects` shows how each object is reachable.

```
git fsck --lost-found
```

This is the deeper recovery tool when the reflog has already expired — a dangling commit can be
turned back into a branch with `git branch <name> <sha>`.

## Optimizing with gc

`git gc` packs loose objects, prunes unreachable ones, expires reflogs, and updates the commit-graph.
It runs automatically when `gc.auto` thresholds are crossed (default ~6700 loose objects), so manual
runs are rarely needed.

- `--aggressive` optimizes harder (much slower); `--prune=<date>` controls loose-object pruning
  (default `2.weeks.ago`); `--no-prune` keeps them.

```
git gc            # routine cleanup
git gc --prune=now
```

## Pruning unreachable objects

`git prune` removes unpacked unreachable objects from the database — but it is normally invoked **by
`git gc`**, not directly. Keep it distinct from the unrelated `git remote prune` /
`git fetch --prune`, which prune destination refs according to refspecs; tag refspecs can also
prune local tags (see [remotes-and-sync](remotes-and-sync.md)).

```
git prune -n   # dry run; prefer running git gc instead
```

## Operational cautions

- **Don't** panic after a bad `reset --hard` or rebase — check `git reflog` and `ORIG_HEAD` first.
- **Don't** run `git gc --prune=now` while another process may be writing objects — it can drop
  objects still being referenced.
- **Don't** confuse `git prune` (object database) with `git remote prune` / `git fetch --prune`
  (reference pruning governed by refspecs).

- **Do** recover lost commits with `git branch <name> <sha-from-reflog>` before they expire.
- **Do** let `gc.auto` handle routine maintenance.
- **Don't** call `git prune` directly unless you understand the consequences — prefer `git gc`.

## Related

[rewriting-history](rewriting-history.md) · [inspection-and-comparison](inspection-and-comparison.md) · [plumbing](plumbing.md) · [remotes-and-sync](remotes-and-sync.md)
