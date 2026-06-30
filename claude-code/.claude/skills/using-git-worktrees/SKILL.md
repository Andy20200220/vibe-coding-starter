---
name: using-git-worktrees
description: '当准备实现新功能、复杂修复或高风险改动，需要先把工作隔离到独立工作区时使用。优先使用平台原生 worktree 工具；无法使用时再回退到 Git worktree 或当前目录方案。 / Use when starting new feature work, complex fixes, or high-risk changes that should happen in an isolated workspace first. Prefer native worktree tools; fall back to Git worktrees or in-place work only when necessary.'
argument-hint: '要隔离执行的任务, e.g.: implement login feature, fix checkout bug, refactor invoice import flow'
user-invocable: true
---

# 隔离工作区技能 / Using Git Worktrees Skill

在开始实现之前，先确认工作是否应该放到独立工作区里完成。优先使用平台原生的 worktree / isolation 工具；如果当前环境没有，就再考虑 Git worktree；再不行才在当前目录里做。

Before implementation starts, first decide whether the work should happen in an isolated workspace. Prefer native worktree / isolation tools when available. If the environment does not provide one, consider a Git worktree. Only work in place as the last fallback.

## 核心原则 / Core Principle

先检测，后创建；先原生，后 Git；先隔离，后实现。不要和当前运行环境对着干。

Detect before creating. Prefer native tools over Git. Prefer isolation before implementation. Do not fight the current harness.

## 何时使用 / When to Use

- 要开始一个新功能 / Starting a new feature
- 要修一个复杂 bug / Fixing a complex bug
- 改动范围较大或回退成本较高 / The change is broad or expensive to undo
- 准备按技术设计进入实现阶段 / You are about to implement an approved technical design
- 希望保护当前主线工作区不被实验性改动污染 / You want to protect the current main workspace from experimental changes

## 何时不使用 / When Not to Use

- 只是读代码、看文档、做解释，不会改文件 / You are only reading code or docs and will not edit files
- 只是一个极小的单文件小修 / The task is a tiny, low-risk, single-file tweak
- 当前环境已经在隔离工作区里 / The current environment is already isolated
- 项目或平台明确不支持隔离工作区 / The project or platform explicitly does not support isolated workspaces

## 操作流程 / Procedure

### 第 1 步：先判断是不是已经隔离 / Step 1: Check whether isolation already exists

开始前先确认当前环境：

Check the current environment before creating anything:

- 是否已经在平台提供的独立 worktree / sandbox / isolated workspace 中 / Whether you are already in a platform-provided isolated workspace
- 是否已经在单独分支或独立目录中 / Whether you are already on a dedicated branch or in a separate directory
- 如果已经隔离，不要再套一层 / If already isolated, do not nest another layer on top

对用户的说明建议：

Recommended user-facing wording:

> "我先检查一下当前是不是已经在隔离工作区里，避免重复创建。 / I'll first check whether we're already in an isolated workspace so I don't create one unnecessarily."

### 第 2 步：优先使用平台原生工具 / Step 2: Prefer native workspace tools

如果当前运行环境自带 worktree / isolation 工具，优先用它，不要自己绕过平台再手工创建一套。

If the current environment already provides a native worktree / isolation tool, use it first instead of manually creating parallel state.

优先级：

Priority order:

1. 平台原生 worktree / isolation 工具 / Native worktree or isolation tool
2. Git worktree / Git worktree fallback
3. 当前目录内实现，但要明确边界和回退点 / In-place implementation with explicit boundaries and rollback point

### 第 3 步：如果没有原生工具，再考虑 Git worktree / Step 3: Use Git worktree only if native tools are unavailable

只有在下面这些条件都满足时，才考虑 Git worktree：

Only consider a Git worktree when all of the following are true:

- 当前项目是 Git 仓库 / The project is a Git repository
- 当前环境没有原生 worktree / isolation 工具 / No native worktree or isolation tool exists
- 用户或项目规则允许这样做 / The user or project rules allow it

如果项目根下已有 `.worktrees/` 或 `worktrees/` 目录，优先沿用现有约定。没有的话，再按项目习惯决定是否新增。

If the project already uses `.worktrees/` or `worktrees/`, reuse that convention. If not, decide based on project norms before creating anything new.

### 第 4 步：如果仍然不能隔离，就原地做，但必须降风险 / Step 4: If isolation is unavailable, work in place carefully

如果平台限制、权限限制或项目环境不允许隔离工作区：

If platform limits, permissions, or project setup prevent isolation:

- 明确告诉用户这次只能在当前目录里做 / Tell the user clearly that the work must happen in the current directory
- 严格缩小改动边界 / Keep the change boundary very small
- 保留清晰的回退点 / Keep a clear rollback point
- 不要直接做大范围试验 / Do not do broad experiments on the main working copy

建议表述：

Recommended wording:

> "当前环境不支持单独隔离工作区，这次我会在当前目录里做，但会把改动控制得更小，并先确认验证方式。 / This environment does not support a separate isolated workspace, so I'll work in place, keep the change scope smaller, and confirm verification up front."

### 第 5 步：隔离好之后，先做基线确认 / Step 5: Verify a clean baseline after isolation

进入隔离工作区后，不要立刻改代码，先确认：

After entering the isolated workspace, do not start coding immediately. First confirm:

- 项目能启动 / The project starts
- 依赖可用 / Dependencies are available
- 关键测试或基线验证可以运行 / Key tests or baseline verification can run
- 当前不是“本来就坏着”的状态 / The workspace is not already broken before your change

如果基线已经失败，要先告诉用户，不要把旧问题和新问题混在一起。

If the baseline already fails, report that first. Do not mix pre-existing failures with new ones.

## 输出要求 / Expected Output

完成隔离准备后，至少要告诉用户：

After isolation setup, report at least:

- 当前是否已处于隔离工作区 / Whether isolation is active
- 使用的是哪种方式（原生工具 / Git worktree / 当前目录） / Which method is being used (native tool / Git worktree / current directory)
- 基线检查结果 / Baseline verification result
- 接下来准备实现什么 / What will be implemented next

建议输出格式：

Suggested output format:

> "隔离工作区已就绪：当前使用 [方式]。基线检查结果是 [通过 / 未通过 + 简述]。接下来我会开始实现 [功能名]。 / Isolated workspace is ready: using [method]. Baseline result: [pass / fail + short note]. Next I will implement [feature]."

## 和本仓库流程的关系 / How This Fits This Repository

- 它服务于 `CLAUDE.md` 里的规则：新增功能、复杂修复、高风险改动优先在隔离工作区完成 / It supports the `CLAUDE.md` rule that new features, complex fixes, and high-risk work should prefer isolated workspaces
- 它通常出现在技术设计确认后、正式实现前 / It usually happens after technical design is confirmed and before implementation starts
- 它可以和 `test-driven-development`、`guided-implementation`、`finishing-a-development-branch` 连起来用 / It pairs naturally with `test-driven-development`, `guided-implementation`, and `finishing-a-development-branch`
- 它不是强制要求所有小改动都开 worktree，而是让高风险工作优先隔离 / It does not require every tiny change to use a worktree; it makes isolation the default for higher-risk work

## 硬规则 / Hard Rules

- 不要在已经隔离的环境里重复创建新的隔离层 / Do not create another isolation layer when one already exists
- 不要有原生工具还绕去手工 Git worktree / Do not bypass a native isolation tool with manual Git worktrees
- 不要在没确认基线的情况下直接开始实现 / Do not start implementation before baseline verification
- 不要把“环境本来就坏了”误算成这次改动造成的问题 / Do not misattribute pre-existing failures to the current task
- 如果不能隔离，必须明确告诉用户这次是在当前目录里操作 / If isolation is unavailable, explicitly tell the user work will happen in the current directory
