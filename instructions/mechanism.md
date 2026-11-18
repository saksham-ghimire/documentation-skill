# Document Mechanism

**Executed By:** Subagent (read [Subagent orchestration](./_orchestration.md#spawning-a-subagent))

**Execution:** The parent spawns one subagent per selected mechanism. Each subagent handles only its assigned ID, writes its corresponding document, and reports its result. The parent exclusively updates the shared index.

**Objective:** Explain a shared technical mechanism and its execution contract to developers unfamiliar with the system.

**Inputs:** `mechanism-index.json`, `entrypoints/<id>.md`, `template/mechanism.md`.

**Output:** `mechanisms/<id>.md`.

**Usage:** `mechanism [<id>...] [--limit N] [--retrace] [--yes]`

| Form | Selects |
|---|---|
| `mechanism` | Every mechanism whose `doc_status` is `pending` |
| `mechanism <id> <id>` | Only specified mechanisms, regardless of status |
| `mechanism --limit 5` | First 5 pending mechanisms, sorted by ID |
| `mechanism --retrace` | Every mechanism, regardless of status |
| `mechanism --yes` | Skip confirmation before replacing existing documents |

## Steps

1. **Understand the contract.**
   - Read the mechanism's `contract` and assigned entrypoint traces, expanding wildcard patterns against `entrypoint-index.json`.
   - Identify how the mechanism executes, where it applies, and where its behavior differs.

2. **Explain the mechanism.**
   - Describe how it works across application components and execution boundaries.
   - Explain what it enforces, how context or state propagates, and what happens on failure.
   - Capture applicability, exceptions, limitations, and guarantees established by the traces.

3. **Write the document** following [the template](../template/mechanism.md).

4. **Report result:** Return the assigned mechanism ID and whether its document was written successfully. Do not update `mechanism-index.json`. The parent marks successful mechanisms `done` and leaves failed mechanisms `pending`.

## Rules

- Read only completed entrypoint traces and the mechanism index. Use `entrypoint-index.json` only to resolve entrypoint patterns. Do not inspect application code.
- Explain the shared mechanism as a whole, not independently for each entrypoint.
- Distinguish enforced guarantees from assumptions and limitations.
- Preserve execution order and failure boundaries when they affect the contract.
- Describe where the mechanism applies and where behavior differs.
- Do not infer guarantees or implementation details not established by traces.
- Write for developers unfamiliar with the system.
- Do not include source citations, trace links, or repository paths.
- Omit empty sections and avoid repetition.
- The parent sets selected mechanisms to `pending` before replacing their documents and updates statuses after collecting subagent results.
- Subagents must not modify `mechanism-index.json`.
- Do not modify application code or entrypoint traces.

**Final reply:** `mechanisms <n>, failed <n>, pending <n>`