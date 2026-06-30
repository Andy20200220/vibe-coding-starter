---
name: finishing-a-development-branch
description: '当实现、验证和审查都完成后，用来做交付收尾：确认当前状态、决定提交/发布/保留、并保护主线。 / Use after implementation, verification, and review are complete to close out the work: confirm status, decide whether to commit/release/keep, and protect the main line. Keywords: finish branch, closeout, release, handoff, commit, ready state.'
argument-hint: '要收尾的工作 / What is being closed out, e.g.: login feature, bugfix branch, phase 2 implementation'
user-invocable: true
---

# 开发分支收尾技能 / Finishing a Development Branch Skill

当实现已经完成，不要直接说“结束了”。先明确验证状态，再明确当前交付状态，再决定怎么收尾。

When implementation appears complete, do not jump straight to “done”. First confirm verification status, then confirm delivery state, then decide how to close out the work.

## 核心原则 / Core Principle

先验证，再定状态，再收尾。

Verify first, decide the state second, close out third.

## 何时使用 / When to Use

- 功能实现完成 / Feature implementation is complete
- bug 修复完成 / Bug fix is complete
- 已经过了测试和代码审查 / Testing and code review are done
- 准备提交、发布、保留当前版本或结束当前阶段 / You are ready to commit, release, keep the current state, or stop at a milestone

## 何时不使用 / When Not to Use

- 还没完成验证 / Verification is not complete yet
- 还有明确阻塞问题未处理 / Blocking issues remain
- 仍在中途开发阶段 / Work is still clearly mid-stream

## 收尾流程 / Procedure

### Step 1 — 先确认验证状态 / Confirm verification first

先调用 `verification-before-completion` 的思路，确认：

Use `verification-before-completion` first and confirm:

- 相关测试或验证已重新运行 / Relevant tests or checks were rerun
- 契约和设计已重新核对 / Contract and design were rechecked
- 当前没有已知阻塞项 / No known blockers remain

### Step 2 — 明确当前交付状态 / State the delivery status

只能从以下四种里选一种：

Choose one of these four states only:

- `继续开发` / Continue development
- `等待确认` / Waiting for confirmation
- `可提交` / Ready to commit
- `可发布` / Ready to release

### Step 3 — 给出收尾选项 / Present closeout options

根据当前状态，给出清楚选项：

Present clear options based on the current state:

1. 保存当前版本（Git 提交） / Save current state with Git commit
2. 准备发布 / Prepare for release
3. 保留当前工作区，稍后继续 / Keep the working area as-is and continue later
4. 如果当前实验无效，放弃本轮改动 / Discard the current attempt if it should not continue

如果当前环境支持独立分支或 worktree，优先保留隔离工作区，验证通过后再合并回主线。

If the environment supports an isolated branch or worktree, prefer keeping the work isolated and only merging back after successful verification.

## 和本仓库流程的关系 / How This Fits This Repository

- 接在 `test-agent` 的质量关卡之后
- 接在 `requesting-code-review` 和 `verification-before-completion` 之后
- 与 `git-commit`、`deployment-check`、`release-agent` 形成收尾链路

推荐顺序：

Recommended sequence:

1. `test-agent` 验证与审查 / validation and review
2. `verification-before-completion` 完成前确认 / verify before claiming done
3. `finishing-a-development-branch` 明确收尾状态 / choose the closeout state
4. `git-commit` 或 `deployment-check` / commit or release readiness

## 硬规则 / Hard Rules

- 不得在未验证通过时宣布“可提交”或“可发布” / Do not mark work ready to commit or release without successful verification
- 不得跳过交付状态说明 / Do not skip the delivery-state declaration
- 不得把“代码写完了”当作“任务结束了” / Do not treat “code written” as “work complete”
- 如果主线保护很重要，优先在隔离工作区完成并验证 / If protecting the main line matters, prefer isolated workspace completion and verification first

## 输出模板 / Output Template

- 当前工作 / Current work item
- 已完成的验证 / Verification completed
- 当前交付状态 / Current delivery state
- 建议的下一步 / Recommended next step
- 是否需要保留隔离工作区 / Whether to preserve the isolated workspace
