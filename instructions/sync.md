# Synchronize Documentation

**Objective:** Keep the documented application consistent with changes to its implementation, propagating affected behavior through all dependent documentation.

**Inputs:** `codebase.yaml`, changed application files, existing schema, indexes, entrypoint traces, feature documents, mechanism documents, workflow documents, technical topics, and final overviews.

**Output:** Updated affected documentation within the configured documentation folder.

**Usage:** `sync`

## Steps

1. **Identify changes.**
   - Inspect the code changes made during the current task, including related configuration, migrations, and database behavior.
   - Determine which existing behaviors changed and whether entrypoints, tables, features, mechanisms, or workflows were added or removed.
   - When change history is unavailable, inspect the relevant changed files and their known dependencies.

2. **Update the foundation.**
   - Regenerate `schema.dbml` when application-owned database structure or database-defined behavior changes.
   - Update `entrypoint-index.json` when entrypoint registrations change.
   - Retrace affected entrypoints, including those affected indirectly through shared middleware, hooks, services, or database behavior.
   - Preserve unaffected traces.

3. **Update indexes.**
   - Reevaluate feature and mechanism membership and contracts using the updated traces.
   - Reevaluate workflow boundaries, entrypoints, tables, feature associations, and mechanism associations.
   - Preserve unchanged entries and their completed statuses.
   - Mark changed or newly discovered entries `pending`.

4. **Update detailed documents.**
   - Regenerate affected feature, mechanism, and workflow documents using their respective instructions.
   - Update or retire technical-topic documents whose explanations depend on changed behavior.
   - Preserve documents whose established behavior remains unchanged.

5. **Update final documents.**
   - Regenerate `overview.md` when product capabilities, journeys, or material product rules change.
    - Regenerate `engineering.md` when architecture, data models, technical mechanisms, workflows, or operational behavior change materially.
    - Follow each overview's existing instructions and templates; keep both final documents standalone.

6. **Verify synchronization.**
    - Verify affected indexes and documents against the current implementation and their command-specific validation rules.
    - Mark records `done` only after their documents have been verified. Leave incomplete records `pending` and report stale documents and blockers.
    - Check dependent documentation for stale references after additions, removals, and renamed behaviors.

## Rules

- Use the existing command instructions for each affected artifact; do not redefine their behavior here.
- Follow `_orchestration.md` for delegated or sequential work and serialize shared index updates through the coordinator.
- Update only affected documentation and preserve valid, unaffected documents and unrelated user changes.
- Do not delete a document based only on missing evidence; verify that its documented behavior was removed or superseded before retiring it, preserving unrelated content.
- Do not fabricate behavior or mark incomplete documentation `done`.
- Do not modify application code or bundled skill resources.
- Keep generated documentation inside the configured documentation folder.
- If synchronization cannot be completed, report which documentation remains stale and why.

**Final reply:** `sync updated <n> documents, stale <n>`
