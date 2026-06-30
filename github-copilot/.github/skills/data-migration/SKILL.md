---
name: data-migration
description: 'Use when the structure of stored data needs to change (adding/removing fields, renaming columns, changing data formats) without losing existing data. Guides the user through a safe migration in plain language. Keywords: vibe coding, data migration, database, schema change, rename column, add field, existing data, safe, non-technical.'
argument-hint: 'What data change is needed, e.g.: add a "last contacted" date to customers, rename the "notes" field to "description", add a required category to all invoices'
user-invocable: true
---

# Data Migration Skill

Safely change the structure of stored data while keeping all existing data intact. Every step is explained in plain language with a backup step before touching anything.

## When to Use

- A behavior contract requires a new data field that does not exist yet
- An existing field needs to be renamed, removed, or changed to a different type
- Data from an old format needs to be converted to a new format
- A new required field is being added to data that was saved without it

## When Not to Use

- No data has been saved yet (the project is new) — just change the data model directly
- The change is only to the UI, not to stored data
- A bug is corrupting data — use `Diagnose and Fix` prompt first to stop the corruption

## Core Rule

**Always back up data before migrating.** Even for small changes, a failed migration can permanently lose data.

## Procedure

### Phase 1 — Understand the change

Ask the user to describe what needs to change in plain language:
- What is the current data? (e.g., "each customer has a name, phone, and notes")
- What should it look like after? (e.g., "each customer should also have a 'last contacted date'")
- What should happen to existing records? (e.g., "existing customers should have today's date as the last contacted date")

Read the relevant behavior contract and data model to understand the technical scope.

### Phase 2 — Assess risk

Categorize the migration:

| Change type | Risk | Approach |
|-------------|------|---------|
| Adding optional field | Low | Add field with null default, no existing data affected |
| Adding required field | Medium | Add field, then fill existing records with a default value |
| Renaming a field | Medium | Add new field, copy data, remove old field in sequence |
| Removing a field | Medium | Confirm data is not needed, then remove |
| Changing data type | High | Convert data, verify, then update schema |
| Splitting/merging fields | High | Step-by-step with verification at each stage |

Tell the user the risk level and approach in plain language before proceeding.

### Phase 3 — Back up first

Before making any changes:

1. Identify where data is stored (database file path, table name, etc.)
2. Create a backup:
   - For SQLite/local files: copy the file to `[filename].backup-YYYYMMDD`
   - For other databases: use the appropriate export/dump command
3. Confirm with the user:
   > "I've saved a backup of your data to `[backup path]`. If anything goes wrong during this migration, you can restore it. Ready to proceed?"

Wait for confirmation.

### Phase 4 — Migrate step by step

For each step of the migration:

1. **Announce the step** in plain language: what change is being made and why
2. **Make the change** (schema + code update, no more than 3 files)
3. **Verify data integrity:**
   - Do existing records still load correctly?
   - Do new records save correctly with the new structure?
   - Does the behavior contract still pass?
4. **Report the result** in plain language
5. **Wait for confirmation** before the next step

### Phase 5 — Verify and clean up

After all steps:
1. Open the affected feature and verify it works end-to-end
2. Confirm existing data is intact
3. Confirm new data saves correctly
4. If everything is verified: offer to delete the backup file
   > "Everything looks good. Should I delete the backup file `[backup path]`? Or would you prefer to keep it a while longer?"

### Phase 6 — Save progress

> "Migration complete. Ready to save? Commit: `Migrate [data] to add/change/rename [field]`"
