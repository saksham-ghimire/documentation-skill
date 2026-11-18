# Mechanism Index

**Objective:** Discover deliberate, application-specific technical behavior executed implicitly outside handlers' explicit call chains.

**Inputs:** `schema.dbml`, `entrypoint-index.json`, `entrypoints/<id>.md`.

**Output:** `mechanism-index.json`, following `template/mechanism-index.jsonc`.

## What counts as a mechanism

An application-specific technical behavior deliberately implemented through middleware, framework hooks, model adaptation, database triggers, policies, or other implicit execution paths.

A mechanism must satisfy all three conditions:

- **Deliberate:** The application explicitly implements or configures the behavior to achieve a technical purpose beyond ordinary framework, ORM, or database operation.
- **Implicit:** The behavior executes automatically rather than through a handler's explicit function calls.
- **Meaningful:** Understanding the behavior reveals a non-obvious technical contract, guarantee, or execution consequence.

Exclude ordinary infrastructure behavior such as generic request validation, exception handling, routing, session cleanup, ORM defaults, and database constraints unless the application introduces a distinct technical strategy beyond their standard operation.

Each mechanism has a `contract` describing what the application-specific behavior establishes.

## Steps

1. **Discover mechanisms.**
   - Read every completed entrypoint trace, including both call trees, Cross-cutting behavior, and Effects.
   - Inspect `schema.dbml` for deliberately implemented database behavior.
   - Identify application-specific implicit execution strategies and the technical purposes they serve.
   - Examine all implicit execution paths, including those named in call trees but not expanded under Cross-cutting behavior.
   - Look for application-defined context propagation, observability, and lifecycle behavior even when it does not affect responses or persisted state.

2. **Validate candidates.**
   - **Deliberate:** Confirm the behavior is explicitly programmed or configured by the application rather than provided by ordinary infrastructure.
   - **Implicit:** Confirm execution is automatically triggered outside the handler's explicit call chain.
   - **Meaningful:** Reject candidates that merely restate expected framework, ORM, database, or validation behavior.
   - **Cohesion:** Group all phases and enforcement components serving the same technical purpose into one mechanism.
   - **Separation:** Split mechanisms only when they establish independently meaningful technical contracts.

3. **Check applicability.**
   - For each mechanism, examine every completed entrypoint trace.
   - Include every entrypoint whose execution can reach the mechanism, including conditional execution paths.
   - Verify coverage against all completed entrypoints before writing the index.

4. **Write the index** following the template. Complete mechanism documentation before indexing workflows.

## Rules

- Prefer a small set of meaningful mechanisms over an exhaustive catalog of automatically executed behavior.
- Do not index behavior merely because it uses middleware, dependencies, hooks, decorators, validators, or constraints.
- Exclude ordinary database `CHECK`, `NOT NULL`, `UNIQUE`, primary-key, foreign-key, and cascade enforcement. Include deliberately programmed database strategies such as custom triggers and row-level-security policies.
- Do not create mechanisms for routine request validation, generic exception translation, framework routing, resource cleanup, timestamps, or generated identifiers.
- Include deliberately implemented logging, tracing, and correlation context propagation; exclude routine logging calls and default framework instrumentation.
- Distinguish application-specific technical protocols from ordinary infrastructure usage.
- Group related phases into one mechanism when they collectively implement the same technical contract, even when implemented across different execution boundaries.
- Do not split a mechanism by request versus response processing, database tables, handlers, or implementation functions.
- Do not create mechanisms for explicitly invoked services, business operations, or technical workflows initiated by handlers.
- Do not infer guarantees from names or intended behavior. Describe only what the completed traces establish, including imperfect enforcement.
- Populate `entrypoints` only after validating mechanisms, not from representative discovery examples.
- List every applicable completed entrypoint, but use `*` suffix wildcards to compress complete groups sharing an ID prefix.
- A wildcard must match only applicable entrypoints. If any completed entrypoint matching the prefix is not applicable, use narrower prefixes or explicit IDs.
- Treat wildcard patterns as prefixes over entrypoint IDs in `entrypoint-index.json`, not as route patterns. Preserve exact coverage when expanding them.
- Include only tables directly involved in implementing the mechanism, not every table affected by it.
- Keep `contract` to one concise sentence describing the technical behavior or guarantee. Leave implementation details and limitations to mechanism documentation.
- Use only completed traces and `schema.dbml`; do not retrace application code.
- Use `unknowns` only when an application-specific implicit mechanism is established but its contract cannot be resolved from the available traces.
- Preserve `done` for unchanged mechanisms with an existing document; otherwise set `pending`.
- Generate IDs by normalizing names: lowercase, replace runs of non-alphanumeric characters with `-`, and trim `-`. Resolve collisions with numeric suffixes.
- Sort mechanisms by ID, and their entrypoint patterns and table lists alphabetically. Remove redundant IDs covered by a wildcard.

**Final reply:** `mechanisms <n>, unknowns <n>`