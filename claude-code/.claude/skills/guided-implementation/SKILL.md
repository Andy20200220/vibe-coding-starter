---
name: guided-implementation
description: '当行为契约已确认、非技术用户需要 AI 逐步实现功能并每步验证时使用。防止一次性大量未审查的改动。 / Use when a behavior contract is confirmed and a non-technical user wants AI to implement the feature step by step with verification at each step. Prevents large unreviewed changes. Keywords: vibe coding, implement, build, code, step by step, non-technical, guided, verified implementation, small changes.'
argument-hint: '要实现的功能和契约 / Feature and contract to implement, e.g.: implement login per docs/contracts/login.md, build the invoice list feature'
user-invocable: true
---

# 引导式实现技能 / Guided Implementation Skill

基于已确认的行为契约，逐步实现功能。每一步只改少量文件，用大白话解释改动，并提供用户可以执行的验证步骤。

Implement a feature step by step based on a confirmed behavior contract. Each step changes a small number of files, explains what changed in plain language, and provides verification the user can perform before proceeding.

## 何时使用 / When to Use

- `docs/contracts/` 中已有行为契约且已确认（无阻塞性待定项） / A behavior contract exists in `docs/contracts/` and is confirmed (no unresolved TBD items blocking implementation)
- 用户想要开始构建功能 / The user wants to proceed with building the feature
- 用户不懂代码，需要大白话解释和每步验证 / The user does not know code and needs plain-language explanations and verification at each step

## 何时不使用 / When Not to Use

- 还没有行为契约 — 先用 `behavior-contract` 技能 / No behavior contract exists yet — use `behavior-contract` skill first
- 想法仍然模糊 — 先用"澄清我的需求" / The idea is still vague — use `Clarify My Requirement` prompt first
- Bug 需要修复 — 用"诊断并修复" / A bug needs fixing — use `Diagnose and Fix` prompt

## 操作流程 / Procedure

### 阶段 1 — 起飞前检查 / Phase 1 — Pre-flight check

1. **读取现有上下文。 / Read existing context.** 读取 `docs/contracts/` 中的行为契约。读取工作区说明。读取 `docs/design/` 下已有的设计文档。如果存在 `docs/contracts/product-definition.md` 也一并读取。 / Read the behavior contract from `docs/contracts/`. Read workspace instructions. Read any existing design docs in `docs/design/`. Read `docs/contracts/product-definition.md` if it exists.

2. **检查待定项。 / Check for TBD items.** 如果行为契约还有未解决的待定项，先让用户解决。不要实现标为待定的行为。 / If the behavior contract has unresolved TBD items, ask the user to resolve them first. Do not implement behaviors marked TBD.

3. **检查项目状态。 / Check project state.** 确认项目能跑起来（构建成功、应用能启动）。如果项目还没初始化，告诉用户必须先初始化。 / Confirm the project can run (build succeeds, app starts). If the project is not yet initialized, tell the user this must happen first and stop.

4. **规划步骤。 / Plan the steps.** 把实现拆分成若干步骤。每步： / Break the implementation into steps. Each step:
   - 最多改 3 个文件 / Changes no more than 3 files
   - 交付契约中一个可测试的行为 / Delivers one testable behavior from the contract
   - 用户能通过具体的 UI 操作来验证 / Can be verified by the user through a specific UI operation

   用大白话把步骤计划展示给用户： / Present the step plan to the user in plain language:
   > "我分 N 步来实现这个功能： / I'll implement this feature in N steps:
   > 1. [用大白话说明第1步做什么] / [what step 1 does, in plain language]
   > 2. [用大白话说明第2步做什么] / [what step 2 does]
   > ...
   > 每一步做完我会告诉你怎么检查。可以开始吗？ / After each step I'll tell you how to check it. Want me to start?"

   等待用户批准。 / Wait for user approval.

### 阶段 2 — 逐步实现 / Phase 2 — Step-by-step implementation

开始逐步实现前，如果是新功能、复杂修复或高风险改动，先加载 `.claude/skills/using-git-worktrees/SKILL.md`，优先在隔离工作区里完成实现与验证。然后再加载 `.claude/skills/test-driven-development/SKILL.md`，按红灯 → 绿灯 → 小范围整理推进。 / Before starting step-by-step implementation, if this is a new feature, complex fix, or high-risk change, first load `.claude/skills/using-git-worktrees/SKILL.md` and prefer isolated execution. Then load `.claude/skills/test-driven-development/SKILL.md` and progress through red → green → limited cleanup.

每一步： / For each step:

1. **宣布要改什么。 / Announce what you will change.**
   > "第 N 步：我要 [大白话描述]。会改动 [文件名]。 / Step N: I'm going to [plain language description]. This will change [file names]."

2. **修改代码。 / Make the code changes.** 每步最多改 3 个文件。 / Change no more than 3 files per step.

3. **解释改了什么。 / Explain what you changed.**
   > "我改了 [文件] 来 [大白话说明这个改动的作用]。这意味着 [用户会怎么注意到这个变化]。 / I changed [file] to [plain language description of what the change does]. This means [how the user will notice the change]."
   > "我没有动 [提一下没改的相关文件]。那些东西跟之前一样。 / I did NOT touch [mention any related files that were left alone]. Those still work the same as before."

4. **提供验证步骤。 / Provide verification steps.**
   > "来检查一下是否正常： / To check this is working:
   > 1. [打开/前往 ...] / [Open / Go to ...]
   > 2. [做 ...] / [Do ...]
   > 3. 你应该看到 [...] / You should see [...]
   > 4. 再试试 [契约中的错误/边界情况]：你应该看到 [...] / Also try [error/edge case from contract]: you should see [...]"

5. **等用户反馈。 / Wait for user feedback.** 用户确认前不要进行下一步： / Do not proceed to the next step until the user confirms:
   - "可以了" → 继续下一步 / "It works" → proceed to next step
   - "有问题" → 切换到诊断模式：解释可能是什么问题，提出最小改动的修复方案，获得批准 / "Something is wrong" → switch to diagnosis mode: explain what might be wrong, propose minimal fix, get approval
   - "我想改需求" → 先更新行为契约，再调整计划 / "I want to change the requirement" → update the behavior contract first, then adjust the plan

### 阶段 3 — 收尾 / Phase 3 — Wrap up

所有步骤完成后： / After all steps are complete:

1. **完整验证。 / Full verification.** 提供一份涵盖契约中所有行为的综合验证清单。让用户从头到尾跑一遍。 / Provide a consolidated verification checklist covering ALL behaviors from the contract. Ask the user to run through it end to end.

2. **完成前验证。 / Verification before completion.** 加载 `.claude/skills/verification-before-completion/SKILL.md`，确认“完成了 / 修好了 / 可提交”这些说法都有新鲜验证证据。 / Load `.claude/skills/verification-before-completion/SKILL.md` and confirm any “done / fixed / ready to commit” claim has fresh evidence.

3. **正式代码审查。 / Formal code review.** 加载 `.claude/skills/requesting-code-review/SKILL.md`，从契约一致性、设计一致性、代码质量三个角度给出结论。 / Load `.claude/skills/requesting-code-review/SKILL.md` and produce a verdict across contract alignment, design alignment, and code quality.

4. **保存验证清单。 / Save verification checklist.** 将验证步骤保存到 `docs/verification/<feature-name>.md`。 / Save the verification steps to `docs/verification/<feature-name>.md`.

5. **Git 提交。 / Git commit.** 建议提交当前状态： / Suggest committing the working state:
   > "这个功能已完成并验证通过。我建议保存这个版本。要我创建存档点（Git 提交）吗？ / This feature is complete and verified. I recommend saving this version. Should I create a save point (Git commit)?"

4. **更新契约状态。 / Update contract status.** 将 `docs/contracts/<feature-name>.md` 的状态更新为"已实现并验证"，标注当天日期。 / Update `docs/contracts/<feature-name>.md` status to "Implemented and verified" with the current date.

5. **说明下一步。 / State next step.** 推荐下一步做什么： / Recommend what to do next:
   - 实现下一个 MVP 功能（参考产品定义） / Implement the next MVP feature (refer to product definition)
   - 或者标注实现过程中发现的依赖或问题 / Or note any dependencies or issues found during implementation

## 硬规则 / Hard Rules

- **每步最多改 3 个文件。 / Maximum 3 files per step.** 如果某步需要更多，进一步拆分。 / If a step requires more, split it further.
- **不得偷偷改代码。 / No silent changes.** 每次改动都必须用大白话宣布和解释。 / Every file change must be announced and explained in plain language.
- **不得悄悄加范围外的东西。 / No scope creep.** 只实现已确认契约中列出的行为。如果你发现有遗漏的行为，告诉用户并提出补充契约——不要偷偷加进去。 / Only implement behaviors listed in the confirmed contract. If you notice a missing behavior, tell the user and offer to update the contract — do not silently add it.
- **实现过程中不重构。 / No refactoring during implementation.** 不要重新组织、重命名或重构现有代码，除非是实现当前契约行为所必需的。如果确实需要重构，先解释原因并获得用户批准。 / Do not reorganize, rename, or restructure existing code unless it is necessary to implement the current contract behavior. If refactoring seems needed, explain why and get user approval.
- **验证通过后才进行下一步。 / Verification before next step.** 用户确认当前步骤通过后，才进行下一步。 / Never skip to the next step without user confirmation that the current step works.
- **修复上限 3 次。 / 3-attempt limit on fixes.** 如果某步 3 次验证仍不通过，按"诊断并修复"协议处理：回退、重启或开新会话。 / If a step fails verification 3 times, follow the escalation protocol from `Diagnose and Fix`: revert, restart, or open new conversation.
