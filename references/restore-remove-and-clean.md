# Restore, remove and clean

Open when restoring or unstaging paths, moving or removing tracked files, or cleaning untracked files.

## Undoing in the working tree and index

`git restore` is the modern, file-scoped tool:

- `git restore <path>` restores the working tree **from the index** (discards unstaged edits).
- `git restore --staged <path>` restores the index **from HEAD** (unstages).
- `git restore --source=<tree> <path>` pulls content from any commit; `-p` selects hunks.

`git reset` (path form) `git reset [<tree-ish>] -- <path>` updates only the staged version (default
from `HEAD`) — the opposite of `git add`, equivalent to `git restore --staged`. The commit-moving
forms (`--soft`/`--mixed`/`--hard`) live in [rewriting-history](rewriting-history.md).

```
git restore --staged file.txt   # unstage
git restore file.txt            # discard unstaged edits
```

## Removing and moving

- `git rm <path>` deletes from **both** the working tree and index; `--cached` removes from the index
  only (keeps the file on disk — use to stop tracking); `-r` recurses.
- `git mv <source> <dest>` renames/moves and updates the index in one step.

```
git rm --cached secret.env
git mv old.txt new.txt
```

## Cleaning untracked files

`git clean` deletes untracked files; `-f` / `--force` is required by default (`clean.requireForce`).
`-d` includes untracked directories; `-n` / `--dry-run` previews; `-x` also removes **ignored** files
while `-X` removes **only** ignored files; `-e <pattern>` adds an exclude.

```
git clean -nd      # preview, then:
git clean -fd
```

## Operational cautions

- **Don't** confuse `-x` (untracked **and** ignored) with `-X` (ignored **only**) — they differ.
- **Don't** `--amend` or hard-reset commits you have already pushed.
- **Don't** run `git clean -f` without a `-n` preview; deletions are not recoverable via Git.

- **Do** stage selectively with `git add -p` to make focused commits.
- **Do** use `git restore`/`git restore --staged` instead of overloaded `git checkout` for files.
- **Don't** rely on `-a` to include brand-new files — it stages tracked changes only.

## Related

[setup-and-config](setup-and-config.md) · [rewriting-history](rewriting-history.md) · [branching-and-merging](branching-and-merging.md) ·
[inspection-and-comparison](inspection-and-comparison.md)

See the [topic index](snapshotting.md) and [related operations](staging-and-committing.md) for surrounding context.
