---
name: refactor-safe
description: '项目变得难以修改、代码乱或重复时使用。在不破坏已有功能的前提下清理和改进代码结构。通过小步验证的方式做结构优化，不改变任何用户可见的行为。 / Use when the project has become slow to change, has messy or duplicated code, or the user wants to clean up without breaking existing features. Makes structural improvements in small verified steps without changing any user-visible behavior. Keywords: vibe coding, refactor, clean up, technical debt, reorganize, improve, non-technical, safe, no behavior change.'
argument-hint: 'What to clean up, e.g.: the whole project feels messy, the login file is too long, there is a lot of duplicated code'
user-invocable: true
---

# 安全重构技能 / Safe Refactor Skill

清理和改进代码结构，不改变任何用户可见的行为。每一步都很小、用大白话解释、并且验证确认已有功能仍然正常。

Clean up and improve code structure without changing any user-visible behavior. Every step is small, explained in plain language, and verified to ensure existing features still work.

## 何时使用 / When to Use

- 添加新功能变得比以前慢

- Adding new features has become slower than expected

- 同一个 bug 在多个地方反复出现

- The same bug keeps appearing in multiple places

- 代码文件变得非常大

- Code files have grown very large

- 用户（或 AI）总要反复解释同样的变通方法

- The user (or AI) keeps having to explain the same workarounds

- `project-health-check` 发现了可维护性问题

- A `project-health-check` identified maintainability issues

## 何时不使用 / When Not to Use

- 有活跃的 bug 要修 —— 先把 bug 修好

- There is an active bug to fix — fix the bug first

- 用户想添加新功能 —— 先加功能，重构分开做

- The user wants to add a new feature — add it first, clean up separately

- 项目当前是坏的状态 —— 先恢复到能跑再说

- The project is in a broken state — restore it to working first

## 核心规则 / Core Rule

**重构绝对不能改变用户看到或感受到的任何东西。** 之前能用的每个功能，重构后必须一模一样地正常工作。如果有行为契约，每一条在每一步后都必须仍然通过。

**Refactoring must never change what the user sees or experiences.** Every behavior that worked before must work exactly the same after. If a behavior contract exists, all items must still pass after each step.

## 执行步骤 / Procedure

### 第一阶段 —— 了解范围 / Phase 1 — Understand the scope

如果用户没具体说，先问用户觉得哪里有问题。留意以下情况：

Ask the user what feels problematic if not specified. Look for:

- 文件太大（建议拆分）

- Very large files (suggest splitting)

- 多处理重复的逻辑（建议提取共享函数）

- Duplicated logic in multiple places (suggest extracting a shared function)

- 命名难以理解（建议重命名）

- Hard-to-follow naming (suggest rename)

- 一个文件里掺了多种不同的职责（建议分离）

- Mixed concerns in one file (suggest separation)

- 没有用到的代码（建议删除）

- Unused code (suggest deletion)

如果范围不明确，先运行 `project-health-check`。

Run `project-health-check` first if the scope is unclear.

### 第二阶段 —— 制定重构计划 / Phase 2 — Plan the refactor

把工作拆成尽可能小的独立步骤。每一步：

Break the work into the smallest possible independent steps. Each step:

- 最多改动 3 个文件

- Changes no more than 3 files

- 只做一种类型的清理（重命名、提取、移动、删除）

- Does exactly one type of cleanup (rename, extract, move, delete)

- 可以通过运行已有测试或手动检查 UI 来验证

- Can be verified by running existing tests or manually checking the UI

用大白话展示计划：

Present the plan in plain language:

> "这是我想清理的内容，分 [N] 步：
> 1. [大白话描述 —— 现在是什么样，改完之后是什么样]
> 2. ...
>
> **重要提醒：** 这些改动不会加新功能，也不会改变任何现有的功能行为。我只是在调整代码内部的整理方式。
>
> 可以开始吗？"

> "Here's what I'd like to clean up, in [N] steps:
> 1. [Plain language description — what it is now, what it will be after]
> 2. ...
>
> **Important:** None of these changes will add new features or change how anything works. I'm only changing how the code is organized internally.
>
> Shall I start?"

等待批准。

Wait for approval.

### 第三阶段 —— 逐步执行 / Phase 3 — Execute step by step

每一步按以下流程：

For each step:

1. **先声明要做什么** —— 并明确说明不会改变什么：

1. **Announce what you will do** — and explicitly state what will NOT change:

   > "第 [N] 步：我要 [描述]。[功能名称] 还是会一模一样地正常工作。我不会去动 [相关的东西]。"

   > "Step N: I'm going to [description]. The [feature name] will still work exactly the same. I'm NOT changing [related thing]."

2. **做修改** —— 最多 3 个文件。

2. **Make the change** — maximum 3 files.

3. **用大白话解释改了什么：**

3. **Explain the result in plain language:**

   > "搞定。[改了什么，为什么这样更好，用大白话说。]"

   > "Done. [What changed and why it is better now, in plain language.]"

4. **验证没有搞坏任何东西：**

4. **Verify nothing broke:**

   - 如果有自动化测试：运行它们。必须全部仍然通过。

   - If automated tests exist: run them. All must still pass.

   - 如果没有自动化测试：提供受影响功能的手动验证步骤。

   - If no automated tests: provide manual verification steps covering the affected feature.

   > "要确认一切正常：
   > 1. [打开 / 前往 ...]
   > 2. [操作 ...]
   > 3. 你应该仍然能看到 [...] —— 跟之前一样"

   > "To confirm everything still works:
   > 1. [Open / Go to ...]
   > 2. [Do ...]
   > 3. You should still see [...] — same as before"

5. **等待确认** 然后再进行下一步。

5. **Wait for confirmation** before the next step.

### 第四阶段 —— 收尾 / Phase 4 — Wrap up

所有步骤完成后：

After all steps:

> "清理完成了。这是改动的总结：
> - [之前 → 之后，用大白话]
> - [之前 → 之后]
>
> 现在项目应该更容易改了，因为 [大白话解释]。"

> "The cleanup is complete. Here's a summary of what changed:
> - [Before → After, in plain language]
> - [Before → After]
>
> The project should now be easier to change going forward because [plain language explanation]."

建议一个 git 提交信息：

Suggest a git commit:

> "要保存吗？我建议这样提交：`Refactor [area] for easier maintenance`"

> "Ready to save? I'd commit this as: `Refactor [area] for easier maintenance`"
