# Workflow Index

**Objective:** Discover technical workflows underlying indexed product features and associate them with applicable documented mechanisms.

**Inputs:** `feature-index.json`, `schema.dbml`, `entrypoint-index.json`, `entrypoints/<id>.md`, `mechanism-index.json`, `mechanisms/<id>.md`.

**Output:** `workflow-index.json`, following `template/workflow-index.jsonc`.

**Prerequisite:** Complete `feature-index`, `mechanism-index`, and mechanism documentation before indexing workflows.

## What counts as a workflow

A coordinated technical process whose state transitions, execution dependencies, or consistency boundaries require understanding multiple operations or execution stages together.

A workflow may span multiple entrypoints, database operations, background execution, and external systems. It may exist within a single entrypoint when its coordination, transaction boundaries, concurrency, or failure behavior establishes a substantial technical concern warranting independent documentation.

A workflow can support multiple features. Each workflow has an `invariant` identifying the defining state relationship or consistency concern connecting its operations.

## Steps

1. **Discover workflows.**
   - For each indexed feature, read its `outcome` and assigned completed entrypoint traces.
   - Identify technical processes through state transitions, execution dependencies, and consistency concerns.
   - Consolidate equivalent processes discovered across different features into one workflow.
   - Check excluded entrypoints for technical workflows not associated with a product-facing feature.

2. **Validate boundaries.**
   - **Significance:** Confirm that the process warrants an independent technical explanation beyond routine operations.
   - **Cohesion:** Every assigned entrypoint must establish, change, enforce, or recover the workflow's `invariant`.
   - **Separation:** Merge groups describing the same process; split groups with independently meaningful state or consistency concerns.
   - **Feature coverage:** Identify the features each workflow supports. A workflow may support multiple features or none.
   - **Execution coverage:** Check completed traces for relevant technical processes without forcing independent operations into workflows.

3. **Link applicable mechanisms.** For each workflow, read its `invariant`, assigned entrypoint traces, and completed mechanism documents.
   - Link a mechanism when it executes within the workflow, explicitly or implicitly.
   - Include shared middleware, framework hooks, context propagation, database policies, and other cross-cutting execution paths.
   - Record applicable mechanism IDs without describing their behavior. Use an empty array when none apply.

4. **Write the index** following the template.

## Rules

- Use indexed features as the starting point for workflow discovery, not as fixed workflow boundaries.
- Group workflows by shared state transitions, execution dependencies, or consistency concerns, not merely by tables, services, modules, or feature names.
- Do not create one workflow per feature by default. Split a feature into independent technical processes when appropriate.
- Merge equivalent workflows shared by multiple features rather than documenting the same process separately.
- Create a standalone workflow only when its technical process warrants an independent explanation. Keep routine ordering, relationship replacement, and ordinary CRUD behavior within broader workflows unless they establish distinct consistency or failure boundaries.
- Include database-enforced behavior, asynchronous operations, and external communication when they participate in a workflow.
- Connect operations only through established state transitions, shared identifiers, or explicitly documented communication. Do not assume subsequent execution without evidence.
- Include an entrypoint only when it establishes, changes, enforces, or recovers the workflow's defining state relationship. Do not include it merely because it reads related state or invokes a shared helper.
- Include only tables whose state or constraints directly participate in the `invariant`.
- An entrypoint may participate in multiple workflows when it contributes distinct behavior.
- Do not create workflows for independent operations without meaningful lifecycle, coordination, or consistency concerns.
- When workflows share operations, each must own a distinct technical concern. Merge workflows that would primarily document the same behavior.
- Keep `invariant` to one concise sentence defining the state relationship or consistency concern. Do not include implementation details, edge cases, or limitations.
- Use only completed traces, indexes, `schema.dbml`, and mechanism documents. Do not retrace application code or regenerate mechanism documents.
- A mechanism does not need to change the workflow's defining outcome to be applicable.
- Do not link mechanisms merely because they exist in the same application or access the same tables.
- Link only documented mechanisms whose applicability is established by the workflow's entrypoint traces.
- A mechanism may apply to only some entrypoints or execution branches within a workflow.
- Use `unknowns` only when a process is established but required participating behavior cannot be identified. Do not treat incomplete trace coverage alone as an unknown.
- Preserve `done` for unchanged workflows with an existing document; otherwise set `pending`. Changes to feature or mechanism associations also set the workflow to `pending`.
- Generate IDs by normalizing names: lowercase, replace runs of non-alphanumeric characters with `-`, and trim `-`. Resolve collisions with numeric suffixes.
- Sort workflows by ID, and their feature, entrypoint, table, and mechanism lists alphabetically. Remove duplicate IDs.

**Final reply:** `workflows <n>, unknowns <n>`