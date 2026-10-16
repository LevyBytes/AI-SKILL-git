# History, diffs and objects

Open when reading commit history, selecting ranges, comparing states or inspecting objects.

Reading history and comparing states. Covers `git log`, `git diff`, `git show`, `git blame`,
`git grep`, `git bisect`, and `git describe`.

**When this applies:** browsing commit history, diffing working tree/index/commits, inspecting an
object, finding who/when a line changed, searching content, or hunting the commit that introduced a
change.

## Browsing history with log

`git log [<revision-range>] [-- <path>]` lists commits. Shaping options:

- `--oneline`, `--graph`, `--decorate` for a compact visual history.
- `-p` / `--patch` (show diffs), `--stat`, `--name-status`.
- Filters: `-<n>`, `--since=<date>` / `--until=<date>`, `--author=<pattern>`, `--grep=<pattern>`,
  `--merges` / `--no-merges`, `--follow` (across renames of one file).
- "Pickaxe": `-S<string>` finds commits that add/remove a string; `-G<regex>` matches diff hunks.
- `--pretty=<format>` / `--format=<format>` (e.g. `format:%h %s`).

```
git log --oneline --graph --decorate --all
```

## Revision ranges

Many commands accept ranges: `A..B` = reachable from `B` but not `A`; `A...B` = symmetric difference;
`^<rev>` excludes. Full revision syntax lives in [refs-and-revisions](refs-and-revisions.md).

```
git log origin/main..HEAD     # local commits not yet pushed
```

## Diffing

`git diff` compares two states; the form decides which two:

| Command | Compares |
| --- | --- |
| `git diff` | working tree vs index (unstaged) |
| `git diff --cached` / `--staged` | index vs `HEAD` (staged) |
| `git diff <commit>` | working tree vs that commit |
| `git diff <c1> <c2>` | two commits |
| `git diff <c1>...<c2>` | changes on `c2` since the merge base |

Useful flags: `--stat`, `--name-only`, `-w` / `--ignore-all-space`, `--color-words`,
`-M` / `--find-renames`, `--no-index` (compare paths outside a repo).

```
git diff --staged
```

## Showing objects

`git show [<object>]` prints a commit's message and diff, a tag's message, a tree's names, or a blob's
contents. Object syntax includes `<commit>:<path>` and `<rev>^{tree}`.

```
git show HEAD
git show v1.0:README.md
```

See the [topic index](inspection-and-comparison.md) and [related operations](blame-search-and-bisect.md) for surrounding context.
