# Document Feature

**Executed By:** Subagent (read [Subagent orchestration](./_orchestration.md#spawning-a-subagent))

**Execution:** The parent spawns one subagent per selected feature. Each subagent handles only its assigned feature, writes one document, and reports its result. The parent exclusively updates the shared index.

**Objective:** Explain what a product capability enables, how users experience it, and the important rules governing it.

**Inputs:** `feature-index.json`, `entrypoints/<id>.md`, `template/feature.md`.

**Output:** `features/<id>.md`.

**Usage:** `feature [<id>...] [--limit N] [--retrace] [--yes]`

| Form | Selects |
|---|---|
| `feature` | Every feature whose `doc_status` is `pending` |
| `feature <id> <id>` | Only the specified features, regardless of status |
| `feature --limit 5` | First 5 pending features, sorted by ID |
| `feature --retrace` | Every feature, regardless of status |
| `feature --yes` | Skip confirmation before replacing existing documents |

## Steps

1. **Understand the scope.**
   - Read the feature's `outcome` and all assigned entrypoint traces.
   - Identify the intended actors, user goals, main interactions, and resulting product outcomes.
   - Distinguish important product rules from incidental endpoint and implementation details.

2. **Explain the product capability.**
   - Organize the explanation around how users accomplish their goals.
   - Describe the main journeys, available actions, and what users experience as a result.
   - Include access restrictions, limits, exceptional outcomes, and system-initiated behavior only when they materially affect the capability or user experience.
   - Summarize related operations into cohesive explanations rather than documenting each operation separately.

3. **Write the document** following [the template](../template/feature.md).

4. **Report result:** Return the assigned feature ID and whether its document was written successfully. Do not update `feature-index.json`. The parent verifies the document and immediately marks successful features `done`; failed features remain `pending`.

## Rules

- Use only indexed entrypoint traces. Do not inspect application source code.
- Stay within the feature's defined `outcome`; do not include unrelated behavior from assigned traces.
- Write for a product owner unfamiliar with the application, not an engineer implementing or debugging it.
- Explain what users can accomplish, how they interact with the capability, and what results they can expect.
- Organize by user goals and journeys, not entrypoints, services, or database tables.
- Prioritize behavior that helps the intended actor understand, use, or make decisions about the capability. Omit incidental implementation consequences and rare failure details unless they materially change the expected product experience.
- Do not turn the document into an exhaustive list of validations, endpoint responses, or edge cases.
- Include exact limits, defaults, and restrictions only when they meaningfully affect product usage or decisions.
- Describe important failures through their user-visible consequences, not their underlying technical causes.
- Exclude transaction boundaries, database constraints, concurrency behavior, serialization details, and internal error handling unless their consequences are essential to understanding the product.
- Include background and system-initiated behavior when it materially changes the product outcome.
- Connect operations only when the traces establish their relationship. Do not assume an initiated operation necessarily completes.
- Describe actual application behavior, not intended requirements or proposed features.
- Retain technical terms only when they identify important external integrations or observable events.
- Prefer concise, cohesive explanations over exhaustive operation lists. Keep simple features short.
- Use tables only when they make meaningful product rules or comparisons easier to understand.
- Do not include source citations, trace links, repository paths, or implementation details.
- Do not infer missing functionality or claim a feature is unsupported solely because it has not been traced.
- When trace coverage is incomplete, describe only established behavior without implying completeness.
- Omit empty sections and avoid repeating behavior.
- The parent sets selected features to `pending` before replacing their documents and updates each status as its subagent finishes.
- Subagents must not modify `feature-index.json`.
- Do not modify application code or entrypoint traces.

**Final reply:** `features <n>, failed <n>, pending <n>`