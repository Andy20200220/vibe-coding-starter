---
name: code-review
description: '功能实现完毕后，用大白话检查代码质量。检查 bug、安全问题、性能问题和可维护性问题，不需要用户读代码。 / Use when a feature has been implemented and the user wants a plain-language code quality review before moving on. Checks for bugs, security issues, performance problems, and maintainability issues without requiring the user to read code. Keywords: vibe coding, code review, quality, security, bug, risk, non-technical, audit, check.'
argument-hint: 'What to review, e.g.: the login feature, the files changed in the last step, the entire project'
user-invocable: true
---

# 代码审查技能 / Code Review Skill

对最近实现的代码进行**面向用户的大白话审查**。重点是把风险、问题和改进建议讲清楚，让不看代码的人也能理解现在哪里稳、哪里不稳。

Run a **plain-language, user-facing review** of recently implemented code. The goal is to explain risks, issues, and improvement opportunities clearly so someone who does not read code can still understand what is solid and what is not.

## 定位 / Positioning

这个技能偏“解释型审查”：
- 适合给用户看结果
- 适合把技术问题翻译成大白话
- 适合判断要不要继续修、补、改
- **不负责** 给出正式的“是否可以继续推进 / 是否通过质量关卡”结论

This skill is the **explanatory review**:
- best when the user needs a readable explanation
- best for translating technical issues into plain language
- best for deciding what probably needs more work
- **not** the formal gate for “ready to proceed” or “passed the quality checkpoint”

如果需要正式质量关卡，请使用 `.claude/skills/requesting-code-review/SKILL.md`。

If you need the formal quality gate, use `.claude/skills/requesting-code-review/SKILL.md`.

## 何时使用 / When to Use

- 一个功能刚刚实现完毕，想用大白话解释目前质量情况给用户听 / A feature was just implemented and you want to explain the quality situation to the user in plain language
- 感觉不对劲但还没有明显的 bug 时 / When something feels off but there is no obvious bug yet
- 用户想知道“这块代码靠谱吗”但不需要正式放行结论 / The user wants to know whether an area feels solid, without needing a formal go/no-go gate
- 在已有代码上添加新功能之前，先做一轮可读性/风险盘点 / Before layering new work on top of existing code, do a readable risk review first

## 何时改用正式审查 / When to Use Formal Review Instead

以下情况优先使用 `.claude/skills/requesting-code-review/SKILL.md`：

Use `.claude/skills/requesting-code-review/SKILL.md` instead when:

- 准备交给 `release-agent` / Before handing off to `release-agent`
- 准备说“可以继续推进 / 可以提交 / 可以发布” / Before saying “ready to proceed / commit / release”
- 需要明确区分“契约/设计是否通过”和“代码质量是否通过” / When you need separate verdicts for contract/design pass and code-quality pass
- 这是一个正式质量关卡，而不是解释型建议 / When this is a formal quality gate, not an explanatory review

## 何时不使用 / When Not to Use

- 已经报告了具体的 bug —— 改用 `Diagnose and Fix` 提示

- A specific bug has already been reported — use `Diagnose and Fix` prompt instead

- 用户想要完整的项目概览 —— 改用 `project-health-check` 技能

- The user wants a full project overview — use `project-health-check` skill instead

## 执行步骤 / Procedure

### 第一阶段 —— 确定检查范围 / Phase 1 — Identify scope

如果用户没有指定检查范围，先问清楚：

Ask the user what to review if not specified:

> "我应该检查：(a) 上一个功能改动过的文件，(b) 某个特定功能区域，还是 (c) 整个项目？"

> "Should I review: (a) just the files changed in the last feature, (b) a specific feature area, or (c) the whole project?"

阅读相关的源文件。同时阅读 `docs/contracts/` 中对应的行为契约，了解代码应该做什么。

Read the relevant source files. Also read the corresponding behavior contract in `docs/contracts/` to understand what the code is supposed to do.

### 第二阶段 —— 执行检查 / Phase 2 — Run the review

逐项检查以下每个类别。对每个问题评定严重程度：🔴 必须修复 / 🟡 应该修复 / 🟢 建议修复。

Check each category below. Rate each issue: 🔴 Must fix / 🟡 Should fix / 🟢 Nice to fix.

**A. 正确性 —— 代码是否做到了契约里承诺的事情？ / Correctness — does the code do what the contract says?**

- 契约中的每一条行为都有对应的实现吗？

- Does each behavior in the contract have a corresponding implementation?

- 契约中有没有遗漏或未完成的行为？

- Are there behaviors in the contract that are missing or incomplete?

- 有没有实现了但契约中没有约定的行为？

- Are there behaviors implemented that are NOT in the contract?

**B. 安全性 —— 这些代码可能被利用或泄露数据吗？ / Security — could this be exploited or expose data?**

- 源码文件中是否硬编码了密码、令牌或 API 密钥？

- Are there hardcoded passwords, tokens, or API keys in source files?

- 用户输入在使用前有没有做校验？（防止注入攻击）

- Is user input validated before being used? (prevents injection attacks)

- 错误信息有没有把系统内部细节暴露给用户？

- Are error messages exposing internal system details to users?

- 敏感数据（密码、个人信息）是否以明文存储或记录在日志中？

- Is sensitive data (passwords, personal info) stored or logged in plain text?

- 文件路径或系统命令是不是用用户输入拼接出来的？

- Are file paths or system commands constructed from user input?

**C. 错误处理 —— 出错时会发生什么？ / Error handling — what happens when things go wrong?**

- 错误有没有被捕获和处理，还是会静默崩溃？

- Are errors caught and handled, or do they silently crash the app?

- 出错时用户能看到有帮助的提示吗？

- Does the user see a helpful message when something fails?

- 有没有空的错误处理器，把失败悄悄藏起来了？

- Are there empty error handlers that hide failures?

**D. 边界情况 —— 代码能处理异常输入吗？ / Edge cases — does the code handle unusual inputs?**

- 输入为空、为零、或者非常大时会怎样？

- What happens with empty input, zero values, very large values?

- 数据库查询没返回结果时会怎样？

- What happens if a database query returns nothing?

- 网络请求失败了会怎样？

- What happens if a network request fails?

**E. 可维护性 —— 以后改起来容易吗？ / Maintainability — will this be easy to change later?**

- 有没有很大的函数（超过约 50 行）做了太多事情？

- Are there very large functions (over ~50 lines) that do too many things?

- 相同的逻辑在多处重复了吗？

- Is the same logic repeated in multiple places?

- 变量名和函数名有意义吗？

- Are variable and function names meaningful?

- 实现过程中留下的 TODO/FIXME 注释有没有没解决的？

- Are there TODO/FIXME comments from the implementation that were not resolved?

### 第三阶段 —— 生成报告 / Phase 3 — Produce the report

用大白话呈现检查结果：

Present findings in plain language:

---
## 代码审查报告 / Code Review Report

检查的功能 / Feature reviewed: [name]
检查的文件 / Files reviewed: [list]

### 做得好的地方 / What Looks Good
- [Item]

### 发现的问题 / Issues Found

#### 🔴 必须修复 / Must Fix
| # | 问题 / Issue | 白话解释 / Plain language explanation | 如何修复 / How to fix |
|---|-------|--------------------------|------------|
| 1 | ... | ... | ... |

#### 🟡 应该修复 / Should Fix
| # | 问题 / Issue | 白话解释 / Plain language explanation | 如何修复 / How to fix |
|---|-------|--------------------------|------------|

#### 🟢 建议修复 / Nice to Fix
| # | 问题 / Issue | 白话解释 / Plain language explanation | 如何修复 / How to fix |
|---|-------|--------------------------|------------|

### 结论 / Verdict
[一句话：可以发布了 / 先把阻塞问题修了 / 需要大幅修改]

[One sentence: ready to ship / fix blockers first / significant rework needed]

---

### 第四阶段 —— 询问是否需要修复 / Phase 4 — Offer to fix

> "要不要我现在把 🔴 必须修复的问题修掉？我会一个一个来，修完一个验证一个。"

> "Would you like me to fix the 🔴 must-fix items now? I'll do them one at a time and verify each one."

如果用户同意，按 `guided-implementation` 流程来处理每一个问题：先声明要改什么，再改，然后解释改了什么，提供验证步骤，等待确认。

If yes, address each issue using the `guided-implementation` flow: announce the change, make it, explain it, provide verification steps, wait for confirmation.
