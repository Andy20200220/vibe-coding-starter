---
name: requesting-code-review
description: '当一个功能、修复或阶段性任务已经实现并完成基本验证后，发起正式代码审查，检查是否符合契约、设计和质量要求。 / Use after a feature, fix, or milestone has been implemented and basically verified to request a formal code review against contract, design, and quality expectations. Keywords: code review, review request, implementation review, quality gate, readiness.'
argument-hint: '要审查的内容 / What to review, e.g.: login feature implementation, invoice import fix, phase 2 changes'
user-invocable: true
---

# 请求代码审查技能 / Requesting Code Review Skill

在继续推进、提交或发布前，先做一次**正式质量关卡审查**，避免把问题带到下一阶段。

Run a **formal quality-gate review** before proceeding, committing, or releasing so issues do not cascade into later stages.

## 定位 / Positioning

这个技能偏“放行型审查”：
- 用来判断是否可以继续推进
- 用来判断是否可以交给 `release-agent`
- 用来给出正式的通过 / 不通过结论
- 默认比 `.claude/skills/code-review/SKILL.md` 更严格

This skill is the **go/no-go review**:
- it decides whether work is ready to proceed
- it decides whether work is ready for `release-agent`
- it gives a formal pass / fail style verdict
- it is stricter than `.claude/skills/code-review/SKILL.md` by default

## 核心原则 / Core Principle

越早审查，返工越少。

Review early so problems do not compound.

## 何时使用 / When to Use

- 一个阶段任务刚完成 / A task or phase was just completed
- 一个功能已实现并通过基本验证 / A feature is implemented and basic verification passed
- 准备交给 `release-agent` 前 / Before handing work to `release-agent`
- 修复复杂 bug 后 / After fixing a complex bug
- 觉得“应该没问题”，但想在继续前多一道质量关 / When you want one more quality gate before continuing

## 何时不使用 / When Not to Use

- 代码还没实现完 / The implementation is not finished yet
- 连最基本验证都还没做 / Basic verification has not been run yet
- 明确还在“红灯测试”阶段 / You are still in the failing-test phase

## 审查前准备 / Preparation

开始前要准备好：

Prepare these first:

- 本轮实现涉及的范围 / The scope of what was implemented
- 对应的行为契约 / The relevant behavior contract
- 对应的技术设计 / The relevant technical design
- 已完成的验证结果 / The verification results already obtained
- 如果有 Git 范围，提供范围；如果没有，就明确文件列表 / If a git range exists, provide it; otherwise provide the file list explicitly

## 审查重点 / What to Check

### 1. 契约一致性 / Contract alignment
- 有没有漏实现的行为？ / Are any contract items missing?
- 有没有做了契约里没写的行为？ / Was anything added that is outside the contract?
- 错误/边界情况是否覆盖？ / Are error and edge cases covered?

### 2. 设计一致性 / Design alignment
- 实现是否遵守技术设计中的接口、数据流和边界？ / Does the implementation match the design's interfaces, data flow, and boundaries?
- 有没有擅自偏离设计？ / Are there unjustified deviations from the design?

### 3. 代码质量 / Code quality
- 是否有明显 bug、脆弱逻辑或可维护性问题？ / Are there bugs, fragile logic, or maintainability issues?
- 是否有不必要的复杂度？ / Is there unnecessary complexity?
- 是否有安全风险、异常处理缺口、明显性能问题？ / Are there security, error-handling, or obvious performance issues?

### 4. 验证质量 / Verification quality
- 当前验证是否真的证明了它可用？ / Does the current verification really prove the work is correct?
- 是否还缺关键验证？ / Are critical checks still missing?

## 输出格式 / Output Format

### 做得好的地方 / Strengths
- [具体优点 / specific strengths]

### 发现的问题 / Issues

#### 🔴 必须修复 / Must Fix
- 文件 / 文件范围
- 问题是什么
- 为什么重要
- 建议怎么修

#### 🟡 应该修复 / Should Fix
- 文件 / 文件范围
- 问题是什么
- 为什么重要
- 建议怎么修

#### 🟢 可选优化 / Nice to Fix
- 文件 / 文件范围
- 问题是什么
- 为什么值得改

### 结论 / Assessment
- **契约/设计是否通过 / Contract & design pass?** 通过 / 不通过
- **代码质量是否通过 / Code quality pass?** 通过 / 不通过
- **是否可继续 / Ready to proceed?** 可以继续 / 先修问题

## 和本仓库流程的关系 / How This Fits This Repository

- 可作为 `test-agent` 在完成验证后的正式审查动作
- 可与现有 `.claude/skills/code-review/SKILL.md` 共存：后者偏大白话审查，这个技能偏正式质量关卡
- 在 `verification-before-completion` 之前或之后都可使用，但最终“可以完成”的结论必须有验证证据支撑

## 硬规则 / Hard Rules

- 不得只说“看起来没问题” / Do not say “looks fine” without specifics
- 不得跳过契约和设计回看 / Do not skip contract and design re-check
- 不得把笼统感觉当审查结论 / Do not turn vague impressions into review verdicts
- 必须把“契约/设计通过”与“代码质量通过”分开给结论 / Must separate contract/design verdict from code-quality verdict
