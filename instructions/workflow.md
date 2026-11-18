# Document Engineering Workflow

**Executed By:** Subagent (read [Subagent orchestration](./_orchestration.md#spawning-a-subagent))

**Execution:** The parent spawns one subagent per selected workflow. Each subagent handles only its assigned ID, writes its corresponding document, and reports its result. The parent exclusively updates the shared index.

**Objective:** Explain a complete application workflow, its state transitions, coordination, and failure behavior to developers unfamiliar with the system.

**Inputs:** `workflow-index.json`, `entrypoints/<id>.md`, `mechanisms/<id>.md`, `template/workflow.md`.

**Output:** `workflows/<id>.md`.

**Usage:** `workflow [<id>...] [--limit N] [--retrace] [--yes]`

| Form | Selects |
|---|---|
| `workflow` | Every workflow whose `doc_status` is `pending` |
| `workflow <id> <id>` | Only specified workflows, regardless of status |
| `workflow --limit 5` | First 5 pending workflows, sorted by ID |
| `workflow --retrace` | Every workflow, regardless of status |
| `workflow --yes` | Skip confirmation before replacing existing documents |

## Steps

1. **Understand the workflow.**
   - Read its `invariant` and assigned entrypoint traces.
   - Read the completed mechanism documents listed under `mechanisms`.
   - Identify the coordinated operations, state transitions, and execution boundaries.

2. **Explain how it works.**
   - Combine participating operations into a complete technical process.
   - Explain state changes, ordering, transaction boundaries, concurrency, and failure consequences.
   - Describe guarantees, unresolved races, and recovery behavior established by the traces.

3. **Apply mechanisms.**
   - Reference every linked mechanism in Cross-cutting mechanisms.
   - Explain where each mechanism applies and its workflow-specific consequences.
   - Link to the mechanism document rather than repeating its implementation, general guarantees, or limitations.

4. **Write the document** following [the template](../template/workflow.md).

5. **Report result:** Return the assigned workflow ID and whether its document was written successfully. Do not update `workflow-index.json`. The parent verifies the document and immediately marks successful workflows `done`; failed workflows remain `pending`.

## Rules

- Use only assigned entrypoint traces and linked mechanism documents. Do not inspect application code.
- Do not regenerate mechanism explanations.
- Preserve workflow-specific consequences even when caused by a shared mechanism.
- Explain how operations connect; do not describe endpoints independently.
- Preserve execution order, state transitions, transaction boundaries, and failure consequences.
- Distinguish enforced guarantees from unresolved races and consistency risks.
- Do not assume separate operations execute sequentially unless established by the traces.
- Do not invent behavior, guarantees, or recovery mechanisms.
- In Cross-cutting mechanisms, explain only how each mechanism affects this workflow. Do not repeat its implementation, general guarantees, or limitations.
- Use relative Markdown links to mechanism documents.
- Do not include source citations, trace links, or repository paths, except relative links to mechanism documents.
- Write for developers unfamiliar with the system.
- Omit empty sections and avoid repetition.
- Do not mark a workflow `done` if any linked mechanism document is missing or incomplete.
- The parent sets selected workflows to `pending` before replacing their documents and updates each status as its subagent finishes.
- Subagents must not modify `workflow-index.json`.
- Do not modify application code or entrypoint traces.

**Final reply:** `workflows <n>, failed <n>, pending <n>`