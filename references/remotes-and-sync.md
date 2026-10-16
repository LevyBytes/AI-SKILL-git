# Remotes and Syncing

Exchanging work with other repositories. Covers `git remote`, `git fetch`, `git pull`, `git push`,
and refspecs.

**When this applies:** adding/inspecting remotes, downloading updates, integrating upstream changes,
publishing commits, or working with refspecs and upstream tracking.

## Managing remotes

A *remote* is a named URL whose branches you track under `refs/remotes/<name>/`. `git remote`
subcommands:

- `git remote add <name> <URL>` (e.g. `origin`); `-f` fetches immediately; `-t <branch>` limits
  tracked branches.
- `git remote -v` lists with URLs; `git remote show <name>` details one.
- `git remote rename <old> <new>`, `git remote remove <name>`.
- `git remote set-url [--push] <name> <newurl>`, `git remote get-url <name>`.
- `git remote prune <name>` drops stale remote-tracking refs.

```
git remote add origin https://example.com/repo.git
```

Key config: `remote.<name>.url`, `remote.<name>.fetch`.

## Fetching

`git fetch [<repository> [<refspec>]]` downloads objects and updates remote-tracking branches (e.g.
`refs/remotes/origin/main`) without merging into your working tree. Refs updated depend on
refspecs; tags and other local refs can also be destinations. Useful flags: `--all`,
`-p` / `--prune` (drop deleted remote refs), `-t` / `--tags`, `--depth=<depth>` / `--unshallow`,
`--dry-run`. Results are also recorded in `FETCH_HEAD`.

```
git fetch --all --prune
```

### Refspecs

A refspec maps source to destination refs: `[+]<src>:<dst>`. The leading `+` allows non-fast-forward
updates. The default fetch refspec is `+refs/heads/*:refs/remotes/origin/*`. A `^<pattern>` is a
negative (exclude) refspec.

## Pulling

`git pull` fetches and integrates the selected upstream. Choose `--ff-only` to reject divergence,
`--rebase` to replay local commits, or `--no-rebase` to merge. Defaults depend on Git version and
configuration; do not assume an implicit merge. `--autostash` temporarily shelves local changes.

```
git pull --rebase origin main
# equivalent to: git fetch origin && git rebase origin/main
```

## Pushing

`git push [<repository> [<refspec>]]` uploads local commits and refs. Common flags:

- `-u` / `--set-upstream` records the upstream so later `git push`/`git pull` need no arguments.
- `--tags` / `--follow-tags` publish tags (not sent by default); `--delete` removes a remote ref.
- `--force-with-lease` checks an expected remote ref value; an implicit lease uses local tracking
  refs and can be weakened by background fetches. Use `--force-with-lease=<refname>:<expect>`
  with a verified expected commit when protecting a specific update. `--atomic` makes a multi-ref
  push all-or-nothing when supported by the server.

```
git push -u origin main
git push origin --delete stale-branch
```

`push.default` (default `simple`) controls behavior with no refspec; deletion syntax is
`git push origin :branch` or `--delete`.

## Operational cautions

- **Don't** assume `git fetch` integrates changes — inspect its refspecs and tag/pruning effects; use
  `git pull` or a manual merge/rebase to integrate.
- **Do** verify the expected remote commit before an authorized force-with-lease update.
- **Don't** expect tags to travel with a normal push — pass `--tags` / `--follow-tags`.

- **Do** `git fetch` + review before integrating on shared branches.
- **Do** set upstream with `-u` on the first push of a branch.
- **Don't** `git pull` with uncommitted conflicting changes — stash or commit first (or `--autostash`).

## Related

[branching-and-merging](branching-and-merging.md) · [setup-and-config](setup-and-config.md) · [refs-and-revisions](refs-and-revisions.md) · [rewriting-history](rewriting-history.md)
