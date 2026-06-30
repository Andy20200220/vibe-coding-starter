---
name: data-migration
description: '存储数据的结构需要变更时使用（添加/删除字段、重命名列、改变数据格式），确保不丢失已有数据。用大白话引导用户完成安全的迁移过程。 / Use when the structure of stored data needs to change (adding/removing fields, renaming columns, changing data formats) without losing existing data. Guides the user through a safe migration in plain language. Keywords: vibe coding, data migration, database, schema change, rename column, add field, existing data, safe, non-technical.'
argument-hint: 'What data change is needed, e.g.: add a "last contacted" date to customers, rename the "notes" field to "description", add a required category to all invoices'
user-invocable: true
---

# 数据迁移技能 / Data Migration Skill

安全地变更存储数据的结构，同时保留所有已有数据不丢失。每一步都用大白话解释，操作之前先备份。

Safely change the structure of stored data while keeping all existing data intact. Every step is explained in plain language with a backup step before touching anything.

## 何时使用 / When to Use

- 行为契约要求增加一个新数据字段，但还没建

- A behavior contract requires a new data field that does not exist yet

- 已有字段需要重命名、删除或改变数据类型

- An existing field needs to be renamed, removed, or changed to a different type

- 旧格式的数据需要转换成新格式

- Data from an old format needs to be converted to a new format

- 要给之前没这个字段的数据添加新的必填字段

- A new required field is being added to data that was saved without it

## 何时不使用 / When Not to Use

- 还没有保存过任何数据（项目是全新的）—— 直接改数据模型就行

- No data has been saved yet (the project is new) — just change the data model directly

- 改的只是 UI，不是存储的数据

- The change is only to the UI, not to stored data

- 有 bug 在损坏数据 —— 先用 `Diagnose and Fix` 流程阻止数据损坏

- A bug is corrupting data — use `Diagnose and Fix` prompt first to stop the corruption

## 核心规则 / Core Rule

**迁移前一定要备份数据。** 即使是很小的改动，失败的迁移也可能会永久丢失数据。

**Always back up data before migrating.** Even for small changes, a failed migration can permanently lose data.

## 执行步骤 / Procedure

### 第一阶段 —— 了解要改什么 / Phase 1 — Understand the change

请用户用大白话描述要改什么：

Ask the user to describe what needs to change in plain language:

- 现在的数据是什么样的？（例如："每个客户有姓名、电话和备注"）

- What is the current data? (e.g., "each customer has a name, phone, and notes")

- 改完之后应该是什么样的？（例如："每个客户还应该有一个'最后联系日期'"）

- What should it look like after? (e.g., "each customer should also have a 'last contacted date'")

- 已有的记录应该怎么处理？（例如："已有客户的最后联系日期填今天的日期"）

- What should happen to existing records? (e.g., "existing customers should have today's date as the last contacted date")

阅读相关的行为契约和数据模型，了解技术范围。

Read the relevant behavior contract and data model to understand the technical scope.

### 第二阶段 —— 评估风险 / Phase 2 — Assess risk

将迁移分类：

Categorize the migration:

| 变更类型 / Change type | 风险 / Risk | 方式 / Approach |
|-------------|------|---------|
| 添加可选字段 / Adding optional field | 低 / Low | 添加字段，默认值为空，已有数据不受影响 / Add field with null default, no existing data affected |
| 添加必填字段 / Adding required field | 中 / Medium | 先添加字段，然后给已有记录填上默认值 / Add field, then fill existing records with a default value |
| 重命名字段 / Renaming a field | 中 / Medium | 先添加新字段，复制数据，再依次删除旧字段 / Add new field, copy data, remove old field in sequence |
| 删除字段 / Removing a field | 中 / Medium | 确认数据不再需要，然后删除 / Confirm data is not needed, then remove |
| 改变数据类型 / Changing data type | 高 / High | 先转换数据，验证，再更新结构 / Convert data, verify, then update schema |
| 拆分/合并字段 / Splitting/merging fields | 高 / High | 逐步进行，每步都验证 / Step-by-step with verification at each stage |

在动手之前，用大白话告诉用户风险等级和处理方式。

Tell the user the risk level and approach in plain language before proceeding.

### 第三阶段 —— 先备份 / Phase 3 — Back up first

在做任何修改之前：

Before making any changes:

1. 确定数据存储位置（数据库文件路径、表名等）

1. Identify where data is stored (database file path, table name, etc.)

2. 创建备份：

2. Create a backup:

   - SQLite/本地文件：复制文件为 `[文件名].backup-YYYYMMDD`

   - For SQLite/local files: copy the file to `[filename].backup-YYYYMMDD`

   - 其他数据库：使用对应的导出/转储命令

   - For other databases: use the appropriate export/dump command

3. 跟用户确认：

3. Confirm with the user:

   > "我已经把你的数据备份到了 `[备份路径]`。如果迁移过程中出了任何问题，都可以恢复。可以开始了吗？"

   > "I've saved a backup of your data to `[backup path]`. If anything goes wrong during this migration, you can restore it. Ready to proceed?"

等待确认。

Wait for confirmation.

### 第四阶段 —— 逐步迁移 / Phase 4 — Migrate step by step

迁移的每一步：

For each step of the migration:

1. **用大白话说明这一步**：改什么、为什么这样改

1. **Announce the step** in plain language: what change is being made and why

2. **做修改**（结构 + 代码更新，最多不超过 3 个文件）

2. **Make the change** (schema + code update, no more than 3 files)

3. **验证数据完整性：**

3. **Verify data integrity:**

   - 已有记录还能正确加载吗？

   - Do existing records still load correctly?

   - 新记录能按新结构正确保存吗？

   - Do new records save correctly with the new structure?

   - 行为契约仍然通过吗？

   - Does the behavior contract still pass?

4. **用大白话汇报结果**

4. **Report the result** in plain language

5. **等待确认** 然后再进行下一步

5. **Wait for confirmation** before the next step

### 第五阶段 —— 验证和清理 / Phase 5 — Verify and clean up

所有步骤完成后：

After all steps:

1. 打开受影响的功能，端到端验证正常

1. Open the affected feature and verify it works end-to-end

2. 确认已有数据完好

2. Confirm existing data is intact

3. 确认新数据能正确保存

3. Confirm new data saves correctly

4. 一切验证通过后：询问是否删除备份文件

4. If everything is verified: offer to delete the backup file

   > "一切正常。要不要我把 `[备份路径]` 这个备份文件删掉？还是你想再保留一阵子？"

   > "Everything looks good. Should I delete the backup file `[backup path]`? Or would you prefer to keep it a while longer?"

### 第六阶段 —— 保存进度 / Phase 6 — Save progress

> "迁移完成。要保存吗？提交信息：`Migrate [data] to add/change/rename [field]`"

> "Migration complete. Ready to save? Commit: `Migrate [data] to add/change/rename [field]`"
