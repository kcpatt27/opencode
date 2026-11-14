# Stop a refetch from resurrecting stale todo rows

Status: open
Source: mining S3-D0073, shard-3.md.
Repo: C:\Users\kcpat\projects\opencode.

## Evidence

The old blank-fetch masked stale server rows. A real refetch, once added, brings
them back at the next turn refetch, which defeats the designed wipe: stale turn
todos reopen the dock.

This depends on the v2 fetch fix in `01-v2-todo-accessor-no-fetch.md`. Land that
first, then close this gap.

## Action

Make an explicit turn-end wipe win until a fresh `todo.updated` event arrives.
Track the wipe generation and ignore a refetch result that predates it.

## Acceptance

- After a turn-end wipe, a refetch does not reopen the dock.
- A fresh `todo.updated` event after the wipe does reopen it.
- A test covers wipe, refetch, then event, and asserts the order.
- The v1 path is unchanged.
