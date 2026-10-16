# Blame, search and bisect

Open when attributing lines, searching tracked content, bisecting a regression or describing a commit.

## Who and where

- `git blame [-L <start>,<end>] <file>` annotates each line with the last commit, author, and date;
  `-L :<funcname>` scopes to a function; `-w` ignores whitespace; `-C` / `-M` detect moved/copied
  lines.
- `git grep <pattern>` searches tracked files (`--cached` searches the index, a `<tree-ish>` searches
  that tree); `-n`, `-i`, `-w`, `-l`, `-F`, `-E`, `-P`, and `-A/-B/-C` context behave like `grep`.

```
git blame -L 40,60 src/app.c
git grep -n "TODO" -- '*.py'
```

## Finding a bad commit

`git bisect` binary-searches history. Mark endpoints and let Git check out midpoints:

```
git bisect start HEAD v1.2     # HEAD bad, v1.2 good
git bisect run make test       # automate; test-command exit status determines the result
git bisect reset
```

`bisect run` treats test exit 0 as good, 1–127 except 125 as bad, and 125 as untestable
(skip). Other statuses abort; 126/127 can indicate a broken test command, so check the harness.

`git describe` names a commit from the nearest reachable tag (e.g. `v1.0.4-14-g2414721`); `--tags`
includes lightweight tags, `--dirty` flags a modified tree.

## Operational cautions

- **Don't** confuse `git diff` (unstaged) with `git diff --cached` (staged) — they compare different
  pairs.
- **Don't** forget `--` before a path that could be read as a revision.
- **Don't** leave a `git bisect` session open — always `git bisect reset`.

- **Do** use `-S`/`-G` pickaxe to find when code appeared or vanished.
- **Do** automate bug-hunting with `git bisect run`.
- **Don't** rely on `git describe` without tags — it errors unless `--always`.

## Related

[snapshotting](snapshotting.md) · [rewriting-history](rewriting-history.md) · [refs-and-revisions](refs-and-revisions.md) · [remotes-and-sync](remotes-and-sync.md)

See the [topic index](inspection-and-comparison.md) and [related operations](history-diffs-and-objects.md) for surrounding context.
