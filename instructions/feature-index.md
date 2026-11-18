# Feature Index

**Objective:** Discover product capabilities from completed entrypoint traces and establish their boundaries for workflow discovery and product documentation.

**Inputs:** `entrypoint-index.json`, `entrypoints/<id>.md`.

**Output:** `feature-index.json`, following `template/feature-index.jsonc`.

## What counts as a feature

A capability through which a user or external actor accomplishes a recognizable product outcome.

A feature may span multiple entrypoints, including background operations that directly contribute to its outcome.

Each feature must have an `outcome` explaining what its entrypoints collectively enable for the intended actor.

## Steps

1. **Discover features.**
   - Read completed traces and identify the product outcomes they enable.
   - Identify the actors who achieve each outcome.
   - Group entrypoints by outcome, not by their names, code locations, or underlying entities.

2. **Validate boundaries.**
   - **Cohesion:** Every assigned entrypoint must directly contribute to the feature's `outcome`.
   - **Actor alignment:** Every assigned entrypoint must serve the stated outcome for its intended actor, not merely operate on related entities or data.
   - **Separation:** Merge groups describing the same outcome; split groups containing independently understandable outcomes, including distinct actor goals.
   - **Coverage:** Account for every completed entrypoint through feature membership or `excluded`, without forcing unrelated operations into features.

3. **Write the index** following the template.

## Rules

- Do not create separate features for individual operations that form one capability.
- Do not combine independent actor goals into one feature merely because they concern the same product entity.
- Different actors may participate in one feature when their actions directly contribute to the same outcome.
- Include system-initiated behavior when it directly contributes to a product outcome.
- An entrypoint may belong to multiple features when it contributes to distinct outcomes.
- Connect entrypoints only through relationships established by completed traces.
- Exclude general maintenance and supporting operations that do not directly contribute to a product outcome.
- Use `unknowns` only when a product capability is established but its boundaries cannot be determined.
- Do not retrace code or describe feature behavior.
- Preserve `done` for unchanged features with an existing document; otherwise set `pending`.
- Generate IDs by normalizing feature names: lowercase, replace runs of non-alphanumeric characters with `-`, and trim `-`. Resolve collisions with numeric suffixes.
- Sort features by ID and their entrypoint lists alphabetically.

**Final reply:** `features <n>, excluded <n>, unknowns <n>`