# 03 - Pre-push typecheck fails on the symlink checkout

Status: open
Source: 2026-11-14 clear-chat audit.
Repo: `projects/opencode`.

## Evidence

`git push` runs a pre-push typecheck that fails because
`packages/app/src/custom-elements.d.ts` is mode `120000` in git and is
materialized as a text file on Windows. The mining ticket push used
`--no-verify` to get past it.

## Action

1. Decide the Windows checkout policy: enable `core.symlinks`, replace the
   symlink with a real file, or add a Windows guard to the typecheck.
2. Apply the fix on `dev`.
3. Push without `--no-verify`.

## Acceptance

A normal `git push` from this Windows checkout passes the pre-push typecheck.
