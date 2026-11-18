# Product Overview

**Objective:** Generate a comprehensive, standalone product document that helps product owners and newcomers understand what the application does, who uses it, and how its capabilities fit together.

**Inputs:** `feature-index.json`, `features/<id>.md`, `template/product-overview.md`.

**Output:** `overview.md`.

**Prerequisite:** Complete feature documentation.

## Steps

1. **Understand the product.**
   - Read the feature index and all completed feature documents.
   - Identify the product's purpose, intended actors, major capabilities, and relationships between them.
   - Identify important user journeys spanning multiple features.
   - Establish the core product concepts and how they relate.

2. **Organize the overview.**
   - Introduce the product, its purpose, and the value it provides.
   - Explain who uses it and what each actor can accomplish.
   - Describe core concepts in product language, including their relationships.
   - Group related features into coherent product areas.
   - Explain journeys and access rules that connect multiple capabilities.
   - Identify material limitations established by the documentation.

3. **Write `overview.md`** following [the template](../template/product-overview.md).
   - Generate product-area subsections from documented capabilities.
   - Integrate all relevant information directly into the document.
   - Verify every indexed feature is represented without requiring the reader to open another document.

4. **Validate the final document.**
   - Verify that every indexed feature is represented.
   - Confirm the document is fully understandable without access to intermediate documentation.
   - Remove all links, citations, source references, document references, repository paths, and references to the documentation process.
   - Remove supporting-document lists and sections that exist only to direct readers elsewhere.
   - Replace phrases such as "the documentation shows" or "the documented behavior" with direct statements about the product.
   - Check that sections provide distinct product understanding without unnecessary repetition.
   - Do not report success until these checks pass.

## Rules

- Use only the feature index and completed feature documents. Do not inspect application code or entrypoint traces.
- The output must be entirely self-contained and suitable for sharing independently outside the project.
- Do not include links, citations, source references, document references, repository paths, or references to the documentation process.
- Never direct the reader to another document for additional details. Include the necessary explanation directly.
- Explain the product as a whole, not as a collection of independent feature summaries.
- Organize by product areas, actors, and user journeys rather than feature IDs.
- Describe how capabilities relate without inventing journeys or dependencies.
- Explain core concepts by their product meaning and relationships, not their database implementation.
- Cover every indexed feature, giving more attention to capabilities central to understanding the product.
- Describe actual application behavior, not intended requirements, acceptance criteria, or proposed functionality.
- Do not invent product vision, target audience, business models, or capabilities beyond what the documentation establishes.
- Include important restrictions, access conditions, and exceptional outcomes when they materially affect product understanding.
- Distinguish separate types of access or entitlement where their relationship matters.
- Avoid repeating detailed rules unnecessarily, but do not omit information essential to understanding the product.
- When documentation is incomplete, describe only established behavior without implying completeness.
- Follow the template's section structure, omitting sections with no established content.
- Generate subsections from actual capabilities rather than forcing predefined categories.
- Keep the document comprehensive, cohesive, and proportional to the application's complexity.
- Replace an existing overview only after the new document has been written successfully.
- Do not modify indexes, source documents, or application code.

**Final reply:** `product overview written`