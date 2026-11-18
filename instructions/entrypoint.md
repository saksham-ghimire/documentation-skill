# Trace Entrypoint

**Executed By:** Subagent (read [Subagent orchestration](./_orchestration.md#spawning-a-subagent))

**Execution:** The parent spawns one subagent per selected entrypoint. Each subagent traces only its assigned ID, writes its corresponding document, and reports its result. The parent exclusively updates the shared index.

**Objective:** Trace an entrypoint's execution path and document its behavior from the code.

**Inputs:** `entrypoint-index.json`, `schema.dbml`, `template/entrypoint.md`.

**Output:** `entrypoints/<id>.md`.

**Usage:** `trace [<id>...] [--limit N] [--retrace] [--yes]`

| Form | Selects |
|---|---|
| `trace` | Every entry whose `doc_status` is `pending` |
| `trace <id> <id>` | Only those entries, whatever their status |
| `trace --limit 5` | The first 5 of the selection, in index order |
| `trace --retrace` | Every entry, regardless of status |
| `trace --yes` | Skip confirmation before replacing existing documents |

## Steps

1. **Map the two call trees.**
   - **Before handler:** Trace execution on the request's path before the handler.
   - **From handler:** Trace execution from the handler through response completion.
   - Include implicitly executed application code, even when invoked by a framework rather than an explicit function call.
   - Open each application function and follow its calls into their bodies, including conditional branches.
   - Stop at a library, storage, another service, or a function already in the tree.
   - Record unresolved call targets under Unknowns.

2. **Explain what happens** from the caller's side:
   - Who can do what, and how the result differs between them.
   - What they send, what is checked, and what they get back.
   - What changes the outcome: input, settings, stored state.
   - Side effects and their order, when the order matters.

3. **Record the details:** Defaults, limits, edge cases, validation rules, and failure responses. Only include details that affect observable behavior.

4. **Capture cross-cutting behavior.**
   - Document behavior outside the handler that affects access, input, output, persisted state, or execution context.
   - Document application-defined execution context, including how request identity, logging correlation, and other context are established, propagated, and cleared.
   - Describe its effect on this entrypoint. Include implementation details only when needed to explain a distinct outcome.
   - Ignore routine log statements, metrics emission, and framework defaults without application-specific behavior.

5. **Capture effects and outbound communication** caused by either call tree, with why each happens.
   - **Effects:** Record explicit application reads and attempted or committed table mutations, including database-triggered mutations. Do not treat implicit constraint checks as reads.
   - **Outbound:** Record actual communication with external systems. Don't duplicate database operations or local SDK calls.
   - Document transaction settings and database constraints through the behavior they enforce, not as table effects.

6. **Trace storage-level behavior.**
   - For each accessed table, inspect `schema.dbml` for database-defined behavior applicable to its operations.
   - Follow relevant triggers, constraints, cascades, and policies to determine their effects.
   - Include resulting state changes, failures, and transaction rollback behavior in Behavior and Effects.
   - Distinguish attempted operations from committed changes; account for transaction and savepoint boundaries.

7. **Capture plans and transitions:**
   - Include only explicitly stated future changes, replacements, or deprecations found in traced code (TODO, FIXME, HACK, deprecation markers, feature flags).
   - Do not infer future direction or propose improvements from current limitations.

8. **Write `doc`** following the [template](../template/entrypoint.md).

9. **Report result:** Return the assigned entrypoint ID and whether its document was written successfully. Do not update `entrypoint-index.json`. The parent marks successful entries `done` and leaves failed entries `pending`.

## Rules

- Keep the two call trees separate, preserving execution order.
- Keep call trees structural: functions and implicit execution paths only. Put conditions and outcomes under Behavior.
- Include application functions that determine behavior or effects; omit helper calls that add no distinct behavior.
- Write both call trees before documenting behavior.
- Never describe a function whose implementation hasn't been inspected.
- Describe outcomes, not implementation steps. Keep simple behavior simple.
- Keep endpoint-specific behavior under What happens; put applicable behavior shared across entrypoints under Cross-cutting behavior. Don't repeat it in both.
- Don't infer behavior or intent that the code doesn't establish.
- Cite sources as `path:line`, except in Summary.
- The parent sets selected entries to `pending` before replacing their documents and updates statuses after collecting subagent results.
- Subagents must not modify `entrypoint-index.json`.
- Omit empty sections rather than documenting that nothing was found.
- Use Unknowns only for behavior that cannot be determined from source code. Missing configuration values, deployment state, external service state, and runtime data are not unknowns; document the behavior conditional on them instead.
- Do not modify application code.
- Include only communication initiated by the application under Outbound. Browser requests caused by returned HTML belong under Behavior.
- Do not cite installed dependencies under `.venv/`; cite the application code invoking them.

**Final reply:** `traced <n>, failed <n>, pending <n>`

## Formatting conventions

- **Call trees:** Use a fenced `text` block with `├─`, `└─`, and `│` to show hierarchy. One function or implicit execution path per line, with its own citation.
- **Citations:** Use repository-relative `path:line`. Do not abbreviate paths or use bare line numbers (`:42`).
- **Behavior:** Use numbered steps for ordered flows and bullets for independent rules. Keep each item focused on one behavior or outcome.
- **Cross-cutting behavior:** Group related behavior under concise labels.
- **Effects:** Use one row per table and operation. Distinguish conditional, attempted, and committed mutations.