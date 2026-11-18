# Initialize Documentation

**Objective:** Initialize documentation storage and configure coding agents to synchronize documentation after application changes.

**Inputs:** `template/codebase.yaml`, existing `codebase.yaml`, and project instruction files supported by the current coding agent.

**Outputs:** `codebase.yaml`, configured documentation folder, and updated coding-agent instruction files.

**Usage:** `init`

## Steps

1. **Initialize documentation.**
   - Look for `codebase.yaml` in the invocation directory.
    - Always ask the user for a documentation folder name and wait for their answer, including when called by `exec` and when configuration already exists. Suggest the existing documentation root if configured, otherwise `docs`.
    - If interaction is unavailable, pause initialization and report that a folder selection is required; do not silently choose a folder or continue the pipeline.
    - Require a project-relative folder inside the project root that is not the project root itself or the installed skill directory.
    - If the selected folder is unchanged, preserve existing configured paths and unrelated settings. Add missing path keys following `template/codebase.yaml` and the legacy configuration rules in `SKILL.md`.
    - If the user selects a different folder, explain that existing generated documents will remain in the old location and ask for confirmation before changing the configuration. Do not move, delete, or overwrite old documents automatically. Preserve unrelated settings and remap configured documentation paths beneath the new root while retaining their relative suffixes.
    - Create the selected documentation folder and create or update `codebase.yaml` following `template/codebase.yaml`, substituting the selected folder for every default `docs` prefix. Do not overwrite existing documents.
   - Keep generated documentation inside the selected folder; `codebase.yaml` remains in the invocation directory.

2. **Configure coding agents.**
    - Inspect project-level instructions supported by the current agent, such as `AGENTS.md`, `CLAUDE.md`, `.github/copilot-instructions.md`, `.cursor/rules/`, or `.windsurf/rules/`. These are examples, not universal equivalents; use the current agent's documented format and scope.
    - Update existing relevant project-wide instruction files whose format is known, preserving frontmatter, scope, and unrelated rules. Do not modify home-directory instructions or unrelated scoped rules.
    - If none exists, create only the project-level instruction file appropriate to the current coding agent. If its convention is unknown, ask the user rather than inventing a format or claiming support.
   - Add or update the following section:

   **Documentation synchronization**

   > After changing application code, configuration, or database behavior, check for `codebase.yaml` in the project root.
   >
   > If present, invoke the `documentation` skill's `sync` command before completing the coding task.
   >
   > Synchronize all affected documentation, including schema, entrypoint traces, indexes, feature documents, mechanism documents, workflow documents, technical topics, and final product and engineering overviews.
   >
   > Update only affected documentation. If synchronization cannot be completed, report which documentation remains stale.

    - Preserve unrelated instructions and avoid duplicate sections.
    - Explain that this is an instruction-based synchronization reminder, not a background hook. It works only when an agent loads and follows the file and can access the installed skill. Report unsupported or unconfigured agents rather than claiming universal synchronization.

3. **Verify initialization.**
   - Confirm the documentation configuration and folder exist.
   - Confirm the configured agent instruction files contain the synchronization rule.

## Rules

- Never overwrite unrelated coding-agent instructions.
- If the synchronization rule already exists, update it in place.
- Do not create instruction files for agents the user has not configured unnecessarily.
- Never skip the folder-name question, even during `exec` or a repeated `init`.
- Do not modify application code.
- Keep synchronization instructions independent of the chosen documentation folder name.
- Re-running `init` must be safe and must not duplicate configuration.

**Final reply:** `documentation initialized`
