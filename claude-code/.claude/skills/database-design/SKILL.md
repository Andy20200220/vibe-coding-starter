---
name: database-design
description: '在新功能需要存储新类型数据，或现有数据结构需要在写代码之前规划时使用。设计表、关系和索引，避免后续出现结构性问题。 / Use when a new feature requires storing new types of data, or when the existing data structure needs to be planned before writing code. Designs tables, relationships, and indexes to avoid structural problems later. Keywords: vibe coding, database, schema, table, relationship, model, data structure, design, non-technical.'
argument-hint: 'What data needs to be stored, e.g.: design the database for the customer tracking app, add a new orders table, model the relationship between users and teams'
user-invocable: true
---

# 数据库设计技能 / Database Design Skill

在写代码之前设计数据的存储方式。好的数据结构让应用更容易构建和修改；差的数据结构会导致问题，修复成本随时间推移越来越高。

Design how data is stored before writing code. A good data structure makes the app easier to build and change; a poor one causes problems that get more expensive to fix over time.

## 适用场景 / When to Use

- 新功能需要存储的数据不适合现有数据结构

- A new feature requires storing data that does not fit the existing structure

- 开始一个新项目，需要决定如何组织数据

- Starting a new project and deciding how to organize data

- 当前数据结构导致 bug 或让新功能难以添加

- The current data structure is causing bugs or making new features hard to add

- 在为一个重要功能编写任何数据库代码之前

- Before writing any database code for a significant feature

## 不适用场景 / When Not to Use

- 数据结构已经存在，只需要小改动 —— 请使用 `data-migration` 技能

- The data structure already exists and just needs a small change — use `data-migration` skill

- 发生了数据库错误 —— 先使用 `诊断并修复（Diagnose and Fix）` 提示

- A database error is occurring — use `Diagnose and Fix` prompt first

## 关键概念（大白话） / Key Concepts (plain language)

- **表（Table）：** 类似电子表格的结构，每一行是一条记录（例如一个客户）

- **Table:** A spreadsheet-like structure where each row is one record (e.g., one customer)

- **列（Column）：** 表中的一个字段（例如客户姓名、电话号码）

- **Column:** A field in the table (e.g., customer name, phone number)

- **主键（Primary key）：** 每条记录的唯一 ID（通常是自动生成的）

- **Primary key:** A unique ID for each record (usually auto-generated)

- **外键（Foreign key）：** 一个列，链接到另一个表的 ID（例如订单的 `customer_id` 链接到客户表）

- **Foreign key:** A column that links to another table's ID (e.g., an order's `customer_id` links to the customers table)

- **索引（Index）：** 一种额外结构，让搜索更快（类似书的目录）

- **Index:** An extra structure that makes searching faster (like a book's index)

- **关系（Relationship）：** 表之间的连接方式：一对一、一对多、多对多

- **Relationship:** How tables connect: one-to-one, one-to-many, many-to-many

## 流程 / Procedure

### 阶段一 —— 理解数据需求 / Phase 1 — Understand the data requirements

阅读 `docs/contracts/product-definition.md` 和相关的行为契约。提取以下信息：

Read `docs/contracts/product-definition.md` and relevant behavior contracts. Extract:

- 有哪些实体？（客户、订单、产品、用户等）

- What entities exist? (customers, orders, products, users, etc.)

- 每个实体有哪些信息？

- What information does each entity have?

- 实体之间如何关联？

- How do entities relate to each other?

- 哪些查询会最常用？（列出所有客户、按客户查找订单等）

- What queries will be run most often? (list all customers, find orders by customer, etc.)

向用户提出澄清问题：

Ask the user clarifying questions:

- "一个客户可以有多个订单，还是只能有一个？"

- "Can a customer have multiple orders, or just one?"

- "一个产品可以属于多个分类吗？"

- "Can a product belong to multiple categories?"

- "你需要保留已删除的记录，还是可以永久删除？"

- "Do you need to keep deleted records, or can they be permanently removed?"

### 阶段二 —— 设计表 / Phase 2 — Design the tables

为每个实体定义：

For each entity, define:

```
Table: [name] (plural, lowercase)
- id: auto-incrementing integer, primary key
- [field]: [type] [nullable?] [notes]
- created_at: timestamp, auto-set on create
- updated_at: timestamp, auto-set on update
```

**数据类型指南（大白话）：**

**Data type guide (plain language):**

| Type | Use for |
|------|---------|
| TEXT | Names, descriptions, emails, any text |
| INTEGER | Whole numbers, counts, IDs |
| REAL/DECIMAL | Money amounts, measurements (use DECIMAL for money) |
| BOOLEAN | Yes/no flags |
| TIMESTAMP | Dates and times |

### 阶段三 —— 定义关系 / Phase 3 — Define relationships

对于表之间的每种关系：

For each relationship between tables:

**一对多（One-to-Many）**（最常见 / most common）：
> 一个客户可以有多个订单。
> One customer can have many orders.
> 在订单表中添加 `customer_id INTEGER`。
> Add `customer_id INTEGER` to the orders table.

**多对多（Many-to-Many）：**
> 一个订单可以包含多个产品，一个产品也可以出现在多个订单中。
> One order can have many products, and one product can appear in many orders.
> 创建一个中间表：`order_items (order_id, product_id, quantity, price_at_time)`
> Create a junction table: `order_items (order_id, product_id, quantity, price_at_time)`

**一对一（One-to-One）：**
> 一个用户有一个个人资料。
> One user has one profile.
> 在个人资料表中添加 `user_id INTEGER UNIQUE`。
> Add `user_id INTEGER UNIQUE` to the profiles table.

### 阶段四 —— 定义索引 / Phase 4 — Define indexes

为以下内容添加索引：

Add indexes for:

- 每个外键列（某些数据库自动处理，不是所有都自动）

- Every foreign key column (automatic in some databases, not all)

- 任何在 WHERE 子句或搜索中使用的列

- Any column used in WHERE clauses or search

- 大型表中在 ORDER BY 中使用的列

- Any column used in ORDER BY for large tables

- 唯一约束（邮箱、用户名等）

- Unique constraints (email, username, etc.)

### 阶段五 —— 检查常见错误 / Phase 5 — Review for common mistakes

检查设计中是否有以下问题：

Check the design for:

| Problem | Example | Fix |
|---------|---------|-----|
| Storing multiple values in one column | `tags: "red,blue,green"` | Create a separate tags table |
| Storing calculated values | `total_price` that is just quantity × unit_price | Calculate at query time instead |
| Missing soft delete | Permanent delete with no recovery | Add `deleted_at` timestamp column |
| No timestamps | No way to know when records were created | Add `created_at` and `updated_at` |
| Storing money as FLOAT | Float rounding errors | Use INTEGER (cents) or DECIMAL |

### 阶段六 —— 记录设计 / Phase 6 — Document the design

保存到 `docs/design/database-design.md`：

Save to `docs/design/database-design.md`:

```markdown
# Database Design

## Tables

### [table_name]
| Column | Type | Nullable | Description |
|--------|------|----------|-------------|
| id | INTEGER | No | Primary key, auto-increment |
| ... | ... | ... | ... |

## Relationships
- [Table A] → [Table B]: [plain language description]

## Indexes
- [table].[column]: [reason]
```

### 阶段七 —— 下一步 / Phase 7 — Next step

> "数据库设计已保存。下一步：使用 `guided-implementation` 创建数据库 schema 和初始迁移，如果是修改现有数据库则使用 `data-migration`。"

> "Database design is saved. Next step: use `guided-implementation` to create the database schema and initial migration, or use `data-migration` if modifying an existing database."
