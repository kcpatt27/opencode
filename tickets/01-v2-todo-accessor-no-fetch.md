# Fetch todos on protocol v2 instead of blanking the store

Status: open
Source: mining S3-D0070, shard-3.md.
Repo: C:\Users\kcpat\projects\opencode.

## Evidence

`packages/app/src/context/server-session.ts`, `todo(sessionID, request)`:

```ts
if ((await options?.protocol) === "v2") {
  setData("todo", sessionID, [])
  return
}
```

On protocol v2 the accessor never fetches. It blanks the store with `[]` and
returns. The v2 SSE stream connects live-only with no replay, and `pagehide`
stops the stream, so a missed `todo.updated` event is permanent and the dock
stays stale.

## Action

Give the v2 path a real fetch, or a replay buffer that closes the gap between
connect and the first event. Do not blank the store as a substitute for a fetch.

## Acceptance

- On protocol v2, `todo()` returns the server's current todos.
- A missed `todo.updated` event is recovered on the next accessor call or on
  reconnect.
- A test simulates a dropped event and asserts the dock recovers.
- The v1 path is unchanged.
