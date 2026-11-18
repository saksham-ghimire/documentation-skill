# Engineering Overview

**Objective:** Generate a comprehensive, standalone engineering document explaining the application's architecture, data model, technical workflows, and shared execution mechanisms to developers unfamiliar with the system.

**Inputs:** `schema.dbml`, `feature-index.json`, `features/<id>.md`, `workflow-index.json`, `workflows/<id>.md`, `mechanism-index.json`, `mechanisms/<id>.md`, `entrypoint-index.json`, `entrypoints/<id>.md`, `template/engineering-overview.md`.

**Output:** `engineering.md`.

**Prerequisite:** Complete feature, mechanism, and workflow documentation.

## Steps

1. **Understand the system.**
   - Read `schema.dbml`, all completed workflow and mechanism documents, and feature documents for product context.
   - Identify major application components, execution boundaries, data responsibilities, and external integrations.
   - Consult completed entrypoint traces when needed to establish data usage or behavior not resolved by the synthesized documents.

2. **Explain the architecture.**
   - Describe major application components and how requests, background operations, and external interactions move through the system.
   - Explain boundaries between application execution, database transactions, and external systems.
   - Explain the shared mechanisms establishing important technical contracts.

3. **Explain the data model.**
   - Group application-owned tables into meaningful data domains.
   - Explain what each entity represents and why it exists.
   - Describe important fields, ownership, relationships, and record lifecycles.
   - Explain how entities are created, updated, referenced, and removed.
   - Connect entities to the technical processes that use them.
   - Describe database-enforced behavior when it materially affects application behavior or integrity.

4. **Explain the technical processes.**
   - Organize related workflows into coherent engineering areas.
   - Explain their state transitions, coordination, and consistency boundaries.
   - Explain how shared mechanisms operate and interact with these processes.
   - Cover significant transaction, concurrency, external-system, and failure characteristics.

5. **Write `engineering.md`** following [the template](../template/engineering-overview.md).
   - Generate data-domain and workflow-area subsections from the documented application.
   - Integrate the necessary technical explanations directly into the document.
   - Verify that every documented workflow and mechanism is covered and every application-owned table is accounted for without requiring external references.

6. **Validate the final document.**
   - Verify every application-owned table, documented workflow, and mechanism is adequately covered.
   - Confirm the document is self-contained, with no links, citations, repository paths, references to supporting documents, or references to the documentation process.
   - Ensure each section contributes distinct engineering understanding. Consolidate repeated explanations without losing important architectural, data-model, or consistency details.
   - Remove incidental endpoint-level details that do not materially affect architecture, security, data integrity, or system behavior.
   - Review sensitive security and operational information for the intended audience. Preserve material risks and limitations for authorized readers.
   - Do not report success until these checks pass.

## Rules

- Use only the supplied documentation and indexes. Do not inspect or retrace application source code.
- The output must be entirely self-contained and suitable for sharing independently outside the project.
- Do not include links, citations, source references, document references, repository paths, or references to the documentation process.
- Never direct the reader to a workflow, mechanism, trace, schema, or other document for additional details. Include the necessary technical explanation directly.
- Technical names such as tables, fields, protocols, and application components may be used when they are necessary to explain the system.
- Explain how the system works as a whole, not as a collection of independent workflow summaries.
- Organize by architectural responsibilities, data domains, and technical processes rather than file names or entrypoint IDs.
- Derive entity meanings from established usage and relationships. Do not infer business meaning from table or column names alone.
- Distinguish persisted state, derived values, temporary execution context, and externally stored data.
- Explain ownership, lifecycle, and relationships where supported; do not invent missing semantics.
- Distinguish intended invariants from guarantees actually enforced by the application.
- Preserve significant transaction boundaries, concurrency risks, partial failures, and consistency limitations.
- Do not turn the document into a table-by-table schema dump or exhaustive endpoint catalog.
- Integrate workflow and mechanism explanations into the architectural narrative without unnecessary repetition.
- Explain how workflows and mechanisms interact when that relationship matters to system behavior.
- Include implementation details necessary for architectural and data-model understanding.
- Cover every application-owned table, giving more attention to entities central to the application's processes.
- Cover all completed workflows and mechanisms, grouping related ones where appropriate.
- Identify genuine gaps in established behavior rather than guessing.
- Use feature documents to establish product context, not to replace technical evidence.
- Follow the template's section structure, omitting sections with no established content.
- Generate subsections from the application's actual architecture and data domains.
- Keep the guide comprehensive and cohesive without repeating the same explanations across sections.
- Replace an existing engineering overview only after the new document has been written successfully.
- Do not modify indexes, source documents, or application code.

**Final reply:** `engineering overview written`