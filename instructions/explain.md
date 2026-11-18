# Explain Technical Behavior

**Objective:** Answer engineering questions about the application and preserve reusable technical understanding for future developers.

**Inputs:** `schema.dbml`, `entrypoint-index.json`, `entrypoints/<id>.md`, `mechanism-index.json`, `mechanisms/<id>.md`, `workflow-index.json`, `workflows/<id>.md`, `template/technical-topic.md`, and relevant application source code when needed.

**Output:** `mechanisms/topics/<id>.md`.

**Usage:** `explain <question>`

## Steps

1. **Understand the question.**
   - Identify the technical concern and its scope.
   - Search existing technical topics for an equivalent explanation.
   - Read relevant schema, mechanism, workflow, and entrypoint documentation.

2. **Investigate the behavior.**
   - Trace how the concern is handled across relevant components and execution paths.
   - Inspect application code when the existing documentation is insufficient or potentially outdated.
   - Identify relevant guarantees, configuration, state, failure boundaries, and limitations.
   - Distinguish actual enforcement from assumptions or intended behavior.

3. **Answer the question.**
   - Explain the technical approach directly and coherently.
   - Include significant exceptions, limitations, and failure or concurrency implications.
   - Distinguish application-wide behavior from behavior limited to particular operations.

4. **Preserve the knowledge.**
   - Create or update a topic document following `template/technical-topic.md`.
   - Group related findings under one cohesive technical concern.
   - Consolidate equivalent questions into an existing topic rather than creating duplicates.
   - Record the established explanation and relevant internal references for future investigation.

5. **Check consistency.**
   - If investigation exposes stale or incorrect existing documentation, update the affected documentation through `sync`.
   - Ensure the saved topic reflects the current established implementation.

## Rules

- Answer the question before expanding into related technical concerns.
- Prefer established documentation, but verify relevant code when evidence is incomplete or potentially stale.
- Do not infer guarantees from framework conventions, code names, or intended behavior.
- Distinguish what the application enforces from what it merely attempts or assumes.
- Preserve meaningful transaction, concurrency, security, failure, and recovery boundaries.
- Explain how multiple mechanisms or workflows interact when necessary.
- Do not duplicate full mechanism or workflow documents.
- Store technical topics separately from `mechanism-index.json`; a question does not automatically establish a new mechanism.
- Use stable, descriptive topic IDs normalized to lowercase hyphenated names.
- Update an existing topic when new findings belong to the same concern.
- Do not create a topic document for unsupported speculation or questions unrelated to the application.
- Technical-topic documents may include internal source and documentation references for maintainability.
- Do not modify application code.
- Do not insert internal source references into the standalone product or engineering overviews.

**Final reply:** `explained <topic-id>`