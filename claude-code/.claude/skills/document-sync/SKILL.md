---
name: document-sync
description: '代码修改后，确认行为契约和验证清单是否仍与实现一致。找出代码已更新但文档未同步、或文档描述了已不存在的功能的地方。 / Use after code changes to make sure behavior contracts and verification checklists still match the implementation. Identifies gaps where the code was updated but the docs were not, or where docs describe behavior that no longer exists. Keywords: vibe coding, documentation, sync, contract, verification, outdated, mismatch, non-technical, doc maintenance.'
argument-hint: '刚刚改了什么，例如：刚修了一个保存功能的 bug、刚改了登录逻辑、重构后检查所有文档 / What was just changed, e.g.: just fixed a bug in the save feature, just changed how login works, check all docs after this refactor'
user-invocable: true
---

# 文档同步技能 / Document Sync Skill

代码修改后，检查行为契约和验证清单是否仍然准确描述了应用的实际行为。更新任何过时的文档。

After code changes, check that behavior contracts and verification checklists still accurately describe what the app does. Update any docs that are out of date.

## 何时使用 / When to Use

- 修复了一个 bug，这个 bug 揭示出合同中有遗漏或错误的条目

- After fixing a bug that revealed a missing or incorrect contract item

- 重构后内部结构变了，但行为不应该变

- After a refactor that changed internal structure but should not have changed behavior

- 添加了边界情况或错误处理，而原合同中没有涵盖

- After adding an edge case or error handling that was not in the original contract

- 定期执行，随着项目增长保持文档可信度

- Periodically, to keep docs trustworthy as the project grows

## 何时不使用 / When Not to Use

- 正在添加新功能——应该在实现之前写行为契约，而非之后

- A new feature is being added — write the behavior contract BEFORE implementation, not after

- 用户想要添加新行为——应使用「行为契约」技能

- The user wants to add new behaviors — use `behavior-contract` skill instead

## 操作步骤 / Procedure

### 第一阶段——确定改了什么 / Phase 1 — Identify what changed

如果用户未指定，则询问："上次会话改了什么？我来检查文档是否还匹配。"

Ask if not specified: "What was changed in the last session? I'll check if the docs still match."

阅读：

Read:

- 相关源代码文件（或文件 diff，如果有的话）

- The relevant source files (or file diff if available)

- `docs/contracts/` 中的当前行为契约

- The current behavior contract in `docs/contracts/`

- `docs/verification/` 中的当前验证清单

- The current verification checklist in `docs/verification/`

### 第二阶段——代码与文档对比 / Phase 2 — Compare code vs. docs

逐条检查合同项与当前实现的匹配情况：

Check each contract item against the current implementation:

**A. 缺失的合同条目**——代码做了合同中没有提到的事情

**A. Missing contract items** — code does something the contract does not mention

- 修 bug 时新增的错误提示

- New error messages added during bug fixing

- 新增的验证规则

- New validation rules added

- 新增的边界情况处理

- New edge case handling added

- 未记录的默认值或回退逻辑

- Default values or fallbacks not documented

**B. 过时的合同条目**——合同描述了已经不存在的东西

**B. Outdated contract items** — contract describes something that no longer exists

- 已修改或删除的错误提示

- Error messages that were changed or removed

- 修 bug 时改掉的行为

- Behaviors that were modified during a fix

- 已删除或替换的功能

- Features that were removed or replaced

**C. 缺失的验证步骤**——某个行为存在但没有验证方式

**C. Missing verification steps** — a behavior exists but there is no way to verify it

- 新增的错误情况，没有测试说明

- New error cases with no test instructions

- 边界情况，没有验证步骤

- Edge cases with no verification steps

**D. 不再有效的验证步骤**——步骤引用了已改掉的 UI 标签或流程

**D. Verification steps that no longer work** — the steps reference old UI labels or flows

- 已改名的按钮

- Button names that changed

- 已变更的页面路由

- Page routes that changed

- 已添加或删除的表单字段

- Form fields that were added or removed

### 第三阶段——报告差距 / Phase 3 — Report the gaps

---
## 文档同步报告 / Document Sync Report

已审查文件：[列表] / Files reviewed: [list]

### 合同差距 / Contract gaps

| 类型 / Type | 位置 / Location | 文档当前描述 / Current doc says | 代码实际行为 / What the code actually does |
|------|----------|-----------------|----------------------------|
| 缺失 / Missing | 合同条目 # / contract item # | （未记录）/ (not documented) | [用大白话描述] / [plain language description] |
| 过时 / Outdated | 合同条目 # / contract item # | [文档说] / [what doc says] | [代码实际做的] / [what code actually does] |

### 验证差距 / Verification gaps

| 类型 / Type | 位置 / Location | 问题 / Issue |
|------|----------|-------|
| 缺失步骤 / Missing step | [功能] / [feature] | [有行为但没有验证] / [behavior with no verification] |
| 已失效步骤 / Broken step | [步骤 #] / [step #] | [为什么不再有效] / [why it no longer works] |

---

### 第四阶段——更新文档 / Phase 4 — Update the docs

对于每个发现的差距：

For each gap found:

1. 用大白话向用户展示建议的修改

   Show the user the proposed update in plain language

2. 确认："我应该把合同改成[新内容]来替代[旧内容]，可以吗？"

   Confirm: "Should I update the contract to say [new text] instead of [old text]?"

3. 逐条应用修改

   Apply updates one item at a time

不要悄悄重写整个文档，只更新实际变化的具体条目。

Do not silently rewrite entire documents — only update the specific items that changed.

### 第五阶段——最终检查 / Phase 5 — Final check

修改完成后：

After updates:

> "文档现在和代码同步了。总结：
> - [N] 条合同条目已更新
> - [N] 条验证步骤已更新
>
> `docs/contracts/` 中的行为契约现在准确描述了应用的工作方式。"

> "Docs are now in sync with the code. Summary:
> - [N] contract items updated
> - [N] verification steps updated
>
> The behavior contracts in `docs/contracts/` now accurately describe how the app works."

建议提交：

Suggest a commit:

> "准备好保存了吗？提交信息：`同步文档，跟进[什么变动]`"

> "Ready to save? Commit: `Sync docs after [what changed]`"
