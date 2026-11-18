# Execute Documentation Pipeline

**Objective:** Generate complete product and engineering documentation autonomously by orchestrating existing documentation commands in dependency order.

**Inputs:** `codebase.yaml` and all documentation command instructions.

**Outputs:** All configured documentation indexes, detailed documents, `overview.md`, and `engineering.md`.

**Usage:** `exec`

## Execution

The parent agent orchestrates every stage. Read `_orchestration.md` and follow each command's instruction file. Use native subagents when available and permitted; otherwise execute the same tasks sequentially. References below to concurrency or subagents use this fallback when necessary.

A stage is complete only when all its work has finished, its outputs have been verified, and the parent has updated the relevant index statuses.

## Steps

1. **Initialize.**
    - Run `init`, always asking the user for the documentation folder name and waiting for their answer. Do not skip this interaction because `exec` is otherwise autonomous or configuration already exists.
   - Wait for initialization to finish before proceeding.

2. **Build the foundation.**
   - Start `schema` and `entrypoint-index` concurrently.
   - Wait for both commands to finish successfully before proceeding.

3. **Trace entrypoints.**
   - Run `entrypoint` for all pending entrypoints.
   - Wait for every entrypoint subagent to finish and all results to be recorded.

4. **Index features.**
   - Run `feature-index`.
   - Wait for completion.

5. **Document features.**
   - Run `feature` for all pending features.
   - Wait for every feature subagent to finish and all results to be recorded.

6. **Index mechanisms.**
   - Run `mechanism-index`.
   - Wait for completion.

7. **Document mechanisms.**
   - Run `mechanism` for all pending mechanisms.
   - Wait for every mechanism subagent to finish and all results to be recorded.

8. **Index workflows.**
   - Run `workflow-index`.
   - Wait for completion.

9. **Document workflows.**
   - Run `workflow` for all pending workflows.
   - Wait for every workflow subagent to finish and all results to be recorded.

10. **Generate final documents.**
    - Start `product-overview` and `engineering-overview` concurrently.
    - Wait for both to finish and pass their final validation.

11. **Verify completion.**
    - Confirm all required indexes and documents exist.
    - Confirm no required records remain pending or failed.
    - Confirm both final documents are complete and standalone.
    - Report incomplete stages and their reasons.

## Rules

- Execute commands using their existing instruction files. Do not duplicate or redefine their behavior.
- Start independent commands as separate subagents when available and permitted; otherwise execute them sequentially. Wait for all work before advancing.
- Run sequential stages strictly in the specified order.
- Never advance past a stage while any of its subagents are still running.
- Within document-generation stages, assign one record per subagent according to that command's instructions, or process one record at a time using the sequential fallback.
- The parent exclusively updates shared indexes and marks each successful record `done` as soon as its subagent finishes and its document is verified.
- Do not wait for the entire batch before updating individual completed records, but wait for the entire batch before advancing stages.
- After interactive initialization, use documented defaults for routine pipeline decisions. Never suppress the folder-name question, confirmation of a changed documentation root, required environment permissions, or safety approvals. If a required decision cannot be resolved, pause and ask the user.
- Preserve valid completed documents rather than regenerating them unnecessarily.
- Do not skip stage validation because outputs already exist.
- If a stage fails, preserve completed work and report the failure. Do not run dependent stages with incomplete prerequisites.
- Do not fabricate missing documentation or mark incomplete records `done`.
- Do not modify application code.
- Keep generated documentation inside the configured documentation folder, except initialization configuration and coding-agent instruction files.
- Ensure the final product and engineering documents are standalone and contain no references to intermediate documentation.

**Final reply:** `exec completed <n> stages, failed <n>, pending <n>`
