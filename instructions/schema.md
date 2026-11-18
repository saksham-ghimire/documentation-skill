# Generate Schema

**Executed By:** Subagent (read [Subagent orchestration](./_orchestration.md#spawning-a-subagent))

**Objective:** Document application-owned database structure and database-enforced behavior from source code.

**Output:** `schema.dbml`

## What to capture

- Application-owned tables, whether defined through ORM models, migrations, raw SQL, or runtime DDL.
- Columns, data types, nullability, defaults, primary keys, foreign keys, unique constraints, indexes, and enums.
- JSON/JSONB shapes defined or accessed in application code, including nested keys and their types.
- Database triggers, constraints, cascades, and row-level security policies that affect reads or writes on included tables.

## Conventions

- Use native DBML syntax for tables, columns, relationships, indexes, and constraints.
- For JSON/JSONB columns, use a multiline column `note` containing the known structure. Include nested keys, types, and optionality. Do not describe what the keys mean.
- For database behavior without native DBML syntax, use a table `Note` with the following format:

  ```
  Trigger: <name>
  Timing: BEFORE | AFTER | INSTEAD OF
  Event: INSERT | UPDATE | DELETE
  When: <condition, if any>
  Action: <function or operation>
  Source: <path:line>
  ```

  Use separate entries for each trigger. Record constraints, cascades, and policies using their declared names and conditions.

## Rules

- Include tables created or managed by application code, regardless of whether an ORM model exists.
- Exclude framework-managed and third-party tables that the application does not own, even when their migrations run within the repository.
- Include framework-origin tables when application code modifies their schema or defines database behavior on them.
- Inspect migrations, schema definitions, ORM models, and SQL executed by application code. Do not rely on ORM models alone.
- Use migrations and database definitions as the source of truth for database structure.
- Inspect application schemas, types, validators, serializers, and JSON read/write paths to recover JSON shapes.
- Record only keys and types established by code; omit unknown portions of a JSON structure.
- Do not infer column meaning, business rules, or descriptions from names.
- Do not add explanatory notes to ordinary columns or tables.
- Do not treat ORM hooks or application callbacks as database triggers.
- Preserve database-defined behavior even when its implementation is outside application code.
- Cite non-native DBML definitions with `path:line`.