# Staging and committing

Open when inspecting the index, staging changes selectively or creating a commit.

Recording work into commits and managing the staging area (the index). Covers `git add`,
`git status`, `git commit`, `git restore`, `git reset` (path form), `git rm`, `git mv`, and
`git clean`.

**When this applies:** staging or unstaging changes, committing, checking working-tree state, or
discarding/cleaning local modifications.

## The index (staging area)

Git records the *index* into the next commit, not your working tree directly. The cycle is: edit →
stage → commit. `git status` shows the split — its short format uses two columns `XY` where `X` is
the staged (index) state and `Y` is the unstaged (working tree) state (`M` modified, `A` added,
`D` deleted, `R` renamed, `??` untracked, `!!` ignored).

```
git status -sb
```

## Staging with git add

- `git add <pathspec>` stages current content; re-run after further edits.
- `-A` / `--all` stages additions, modifications, **and** deletions; `-u` / `--update` limits to
  already-tracked paths.
- `-p` / `--patch` stages selected hunks interactively; `-n` / `--dry-run` previews; `-f` / `--force`
  adds ignored files; `-N` / `--intent-to-add` records a path for later.

```
git add -p
```

## Committing

`git commit` records the staged index. Key flags:

- `-m <msg>` / `--message=<msg>` (repeat for paragraphs); `-a` / `--all` auto-stages modified and
  deleted **tracked** files (not new ones).
- `--amend` rewrites the tip commit (don't amend already-pushed history); `--no-edit` keeps the
  message.
- `-s` / `--signoff`, `-S` / `--gpg-sign`, `--allow-empty`, `--no-verify` (skip `pre-commit` /
  `commit-msg` hooks).

Convention: a short summary line, a blank line, then the body. Identity comes from `user.name` /
`user.email` unless `GIT_AUTHOR_*` / `GIT_COMMITTER_*` are set.

```
git commit -m "Fix off-by-one in parser"
```

See the [topic index](snapshotting.md) and [related operations](restore-remove-and-clean.md) for surrounding context.
