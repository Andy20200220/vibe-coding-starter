---
name: database-design
description: 'Use when a new feature requires storing new types of data, or when the existing data structure needs to be planned before writing code. Designs tables, relationships, and indexes to avoid structural problems later. Keywords: vibe coding, database, schema, table, relationship, model, data structure, design, non-technical.'
argument-hint: 'What data needs to be stored, e.g.: design the database for the customer tracking app, add a new orders table, model the relationship between users and teams'
user-invocable: true
---

# Database Design Skill

Design how data is stored before writing code. A good data structure makes the app easier to build and change; a poor one causes problems that get more expensive to fix over time.

## When to Use

- A new feature requires storing data that does not fit the existing structure
- Starting a new project and deciding how to organize data
- The current data structure is causing bugs or making new features hard to add
- Before writing any database code for a significant feature

## When Not to Use

- The data structure already exists and just needs a small change — use `data-migration` skill
- A database error is occurring — use `Diagnose and Fix` prompt first

## Key Concepts (plain language)

- **Table:** A spreadsheet-like structure where each row is one record (e.g., one customer)
- **Column:** A field in the table (e.g., customer name, phone number)
- **Primary key:** A unique ID for each record (usually auto-generated)
- **Foreign key:** A column that links to another table's ID (e.g., an order's `customer_id` links to the customers table)
- **Index:** An extra structure that makes searching faster (like a book's index)
- **Relationship:** How tables connect: one-to-one, one-to-many, many-to-many

## Procedure

### Phase 1 — Understand the data requirements

Read `docs/contracts/product-definition.md` and relevant behavior contracts. Extract:
- What entities exist? (customers, orders, products, users, etc.)
- What information does each entity have?
- How do entities relate to each other?
- What queries will be run most often? (list all customers, find orders by customer, etc.)

Ask the user clarifying questions:
- "Can a customer have multiple orders, or just one?"
- "Can a product belong to multiple categories?"
- "Do you need to keep deleted records, or can they be permanently removed?"

### Phase 2 — Design the tables

For each entity, define:

```
Table: [name] (plural, lowercase)
- id: auto-incrementing integer, primary key
- [field]: [type] [nullable?] [notes]
- created_at: timestamp, auto-set on create
- updated_at: timestamp, auto-set on update
```

**Data type guide (plain language):**
| Type | Use for |
|------|---------|
| TEXT | Names, descriptions, emails, any text |
| INTEGER | Whole numbers, counts, IDs |
| REAL/DECIMAL | Money amounts, measurements (use DECIMAL for money) |
| BOOLEAN | Yes/no flags |
| TIMESTAMP | Dates and times |

### Phase 3 — Define relationships

For each relationship between tables:

**One-to-Many** (most common):
> One customer can have many orders.
> Add `customer_id INTEGER` to the orders table.

**Many-to-Many:**
> One order can have many products, and one product can appear in many orders.
> Create a junction table: `order_items (order_id, product_id, quantity, price_at_time)`

**One-to-One:**
> One user has one profile.
> Add `user_id INTEGER UNIQUE` to the profiles table.

### Phase 4 — Define indexes

Add indexes for:
- Every foreign key column (automatic in some databases, not all)
- Any column used in WHERE clauses or search
- Any column used in ORDER BY for large tables
- Unique constraints (email, username, etc.)

### Phase 5 — Review for common mistakes

Check the design for:

| Problem | Example | Fix |
|---------|---------|-----|
| Storing multiple values in one column | `tags: "red,blue,green"` | Create a separate tags table |
| Storing calculated values | `total_price` that is just quantity × unit_price | Calculate at query time instead |
| Missing soft delete | Permanent delete with no recovery | Add `deleted_at` timestamp column |
| No timestamps | No way to know when records were created | Add `created_at` and `updated_at` |
| Storing money as FLOAT | Float rounding errors | Use INTEGER (cents) or DECIMAL |

### Phase 6 — Document the design

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

### Phase 7 — Next step

> "Database design is saved. Next step: use `guided-implementation` to create the database schema and initial migration, or use `data-migration` if modifying an existing database."
