# Command safety and shell quoting

Open when choosing a Git operation that changes files or history, recovering work, or adapting a
Git example to the shell running it.

## Before changing repository state

Read the reference for the command's affected state: working tree, index, local refs or remote refs.
Inspect current changes and preserve work before an operation that can discard it. Follow the
current task's authorization, including constraints such as no fetch or no push. Reading an example
does not authorize executing it. Preserve exact Git identifiers; report gaps instead of inventing options.

For shared history, read [history rewriting](rewriting-history.md) and [remote updates](remotes-and-sync.md).
For local recovery, use [recovery and maintenance](recovery-and-maintenance.md). Reflogs can help
recover commits; they do not guarantee recovery of uncommitted content discarded by reset or clean.

## Interpret example syntax

Angle-bracket terms such as `<path>` and `<commit>` are placeholders. Substitute real arguments;
do not paste the brackets into a shell. Sample commit IDs, branch names and filenames need matching
repository objects or files. Inspect a command's prerequisites before running it.

Quote revision arguments when the shell treats braces, at-signs, carets or other characters specially.
For example, these quoted arguments work in POSIX shells and PowerShell:

```text
git show 'HEAD@{1}'
git diff '@{u}...HEAD'
```

Windows cmd.exe uses different quoting rules; single quotes do not quote its arguments. Adapt the
shell syntax without changing the revision expression passed to Git. Use `--` where supported to
separate revision/options from path arguments, and quote paths containing spaces.

`&&` examples assume a shell supporting conditional chaining. Windows PowerShell 5.1 does not;
run commands separately and check `$LASTEXITCODE` before proceeding. Capture native exit status
immediately rather than assuming a printed message establishes success.

## Recovery limits

Prefer creating a recovery branch at the verified lost commit before moving an existing branch.
Check the reflog entry before using it: its position changes as later operations add entries.
An overwrite requires both the right target and authorization to discard the affected content.

For merge conflicts, inspect both sides and preserve useful local changes before aborting. For force
pushes, verify the expected remote commit; an implicit lease can be weakened by background fetches.

Return to the [Context Router](../SKILL.md#context-router).
