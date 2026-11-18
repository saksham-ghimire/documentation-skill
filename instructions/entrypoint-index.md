# Entrypoint Index

**Executed By:** Subagent (read [Subagent orchestration](./_orchestration.md#spawning-a-subagent))

**Objective:** Explore the codebase and index every point where a request, message, schedule, or person directly invokes application behavior.

**Output:** `entrypoint-index.json`, following `template/entrypoint-index.jsonc`.

## What counts as an entrypoint

A handler, command, or script that can be invoked independently to execute application behavior.

| Kind | Trigger format | Example |
|---|---|---|
| `http` | `METHOD /full/path` (`ANY` if every method is accepted) | `GET /api/users/{id}` |
| `graphql` | `Type.field` | `Mutation.createUser` |
| `rpc` | `Service/Method` | `UserService/GetUser` |
| `message` | `<transport> <topic, queue or event>` | `kafka orders.created` |
| `schedule` | Schedule as declared | `cron 0 3 * * *` |
| `command` | Command a person runs | `php artisan users:prune` |

Normalize path parameters to `{name}` and wildcards to `*`.

Scripts that operate on application data count as entrypoints (e.g., data cleanup). Scripts used only for building, setup, or schema migrations do not.

## Rules

- Exclude entrypoints that only report system status or serve static content without accessing application data (e.g., health checks, readiness probes, static assets).
- Don't trace or describe what an entrypoint does.
- Cite sources as `path:line`. The `handler` is where the handler function is defined; for a script with no function, its first executed line.
- Resolve the full trigger, including route prefixes and mounts.
- Record one entry per unique kind, trigger, and handler.
- Build each `id` as the template says. If two would clash, append the handler's `path:line` using the same normalization.
- Use `unknowns` only for starting points whose trigger or handler the code can't resolve.
- Set `doc_status` to `pending` for new entries. Preserve `done` for unchanged entries with an existing document; otherwise set `pending`.
- Rewrite the index to reflect the current codebase. Remove entries no longer present.

**Final reply:** `entrypoints <n>, excluded <n>, unknowns <n>`
