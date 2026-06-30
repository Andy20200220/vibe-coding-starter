# Claude Code 配置指南 / Claude Code Setup Guide

本目录提供一套可直接复制到项目中的 **Claude Code CLI（CC）工作流模板**。

This directory provides a **ready-to-copy Claude Code CLI (CC) workflow template** for existing projects.

它不是单纯的提示词集合，而是一套“行为契约优先 + 分阶段 Agent 协作 + TDD 驱动执行 + 完成前验证”的工作方式。

It is not just a prompt bundle. It is a workflow built around contract-first delivery, phased agent handoff, TDD-driven execution, and verification before completion.

---

## 这套模板解决什么问题 / What This Template Is For

适合这些场景：

This template is useful when you want to:

- 把 Claude Code 接入一个已有项目 / add Claude Code to an existing project
- 让 AI 先看清“应该怎么做”，再开始写代码 / make the AI understand expected behavior before coding
- 把需求、设计、实现、测试、审查、收尾分阶段推进 / move through requirement, design, implementation, testing, review, and closeout in clear stages
- 避免“还没验证就说做完了” / avoid claiming work is done before verification
- 让不同子 Agent 各司其职，不混角色 / keep sub-agents focused on distinct responsibilities

---

## 配置文件说明 / What Is Included

直接复制整个 `claude-code/` 文件夹到你的项目根目录，即可获得这套工作流的基础结构。

Copy the entire `claude-code/` folder into your project root to get the base workflow structure.

复制后的项目结构：

```text
your-project/
├── CLAUDE.md                              ← 项目级说明，Claude Code 会自动读取
├── .claude/
│   ├── commands/
│   │   ├── clarify-my-requirement.md      ← 需求澄清入口
│   │   └── diagnose-and-fix.md            ← 排查 bug 入口
│   ├── agents/
│   │   ├── product-agent.md               ← 需求澄清 / 行为契约
│   │   ├── planning-agent.md              ← 技术设计 / 计划拆解
│   │   ├── dev-agent.md                   ← 按设计实现
│   │   ├── test-agent.md                  ← 测试 / 验证 / 正式审查
│   │   └── release-agent.md               ← 提交 / 发布 / 收尾
│   └── skills/
│       ├── behavior-contract/SKILL.md
│       ├── guided-implementation/SKILL.md
│       ├── using-git-worktrees/SKILL.md
│       ├── test-driven-development/SKILL.md
│       ├── systematic-debugging/SKILL.md
│       ├── verification-before-completion/SKILL.md
│       ├── requesting-code-review/SKILL.md
│       ├── finishing-a-development-branch/SKILL.md
│       └── ...
└── docs/
    ├── contracts/                         ← 行为契约 / Behavior contracts
    ├── design/                            ← 技术设计 / Technical design
    ├── plans/                             ← 执行计划 / Plans and logs
    └── verification/                      ← 验证记录 / Verification notes
```

复制后，把根目录的 `CLAUDE-template.md` 重命名为 `CLAUDE.md`，再填写项目静态信息。

After copying, rename `CLAUDE-template.md` to `CLAUDE.md` and fill in your project-specific static facts.

---

## 核心工作流 / Core Workflow

这套模板的主线不是“让 AI 自由发挥”，而是按下面顺序推进：

This template is designed around a controlled workflow, not free-form AI coding:

1. **行为契约 / Behavior contract** — 先写清楚“应该怎么表现”
2. **技术设计 / Technical design** — 再写清楚“准备怎么实现”
3. **测试先行 / Test-first or verification-first** — 先定义通过标准
4. **实现 / Implementation** — 小步改动，按设计落地
5. **完成前验证 / Verification before completion** — 用新鲜证据证明已经可用
6. **正式代码审查 / Formal review gate** — 判断是否真的可以继续推进
7. **交付收尾 / Delivery closeout** — 明确当前状态是继续开发、等待确认、可提交还是可发布

如果是新功能，推荐走：

For new features, the recommended path is:

`behavior-contract` → `planning-agent / docs/design` → `test-driven-development` → `guided-implementation` → `verification-before-completion` → `requesting-code-review` → `finishing-a-development-branch`

如果是 bug，推荐走：

For bug work, the recommended path is:

`diagnose-and-fix` / `systematic-debugging` → 修复实现 → `verification-before-completion` → `requesting-code-review` → `finishing-a-development-branch`

---

## 两个“审查”技能的区别 / Difference Between the Two Review Skills

这套模板里有两个名字相近、但职责不同的技能：

This template includes two review skills with different purposes:

- `code-review`：**大白话解释型审查**，适合给用户说明代码质量、风险和改进建议
- `requesting-code-review`：**正式质量关卡审查**，用于判断是否通过、是否可继续、是否可交给 release-agent

- `code-review`: a **plain-language explanatory review** for discussing code quality, risks, and improvements
- `requesting-code-review`: a **formal quality-gate review** used to decide whether work is ready to proceed or hand off to `release-agent`

简单说：前者偏“讲清楚”，后者偏“能不能放行”。

In short: one explains, the other decides go / no-go.

---

## 已有项目接入步骤 / How To Install Into an Existing Project

### 第一步：复制文件夹 / Step 1: Copy the folder

把 `claude-code/` 目录下的内容（`CLAUDE-template.md`、`.claude/`、`docs/`）复制到你的项目根目录。

Copy the contents of `claude-code/` (`CLAUDE-template.md`, `.claude/`, and `docs/`) into your project root.

### 第二步：重命名并填写项目信息 / Step 2: Rename and fill project facts

把 `CLAUDE-template.md` 重命名为 `CLAUDE.md`，填写所有 `[FILL IN]` 项。

Rename `CLAUDE-template.md` to `CLAUDE.md` and fill every `[FILL IN]` section.

### 第三步：检查目录和技能 / Step 3: Check folders and skills

确认这些目录存在并可用：

Confirm these folders exist and are ready to use:

- `docs/contracts/`
- `docs/design/`
- `docs/plans/`
- `docs/verification/`
- `.claude/agents/`
- `.claude/skills/`

### 第四步：验证生效 / Step 4: Verify Claude Code picks it up

在项目目录下运行 `claude`，然后问：

Run `claude` in the project and ask:

> 你知道这个项目是做什么的吗？有哪些约束？下一步应该按什么流程走？
>
> Do you know what this project is for? What constraints apply? What workflow should we follow next?

如果 AI 能说出项目事实、约束和阶段性流程，说明配置已生效。

If the AI can describe the project facts, constraints, and phase-based workflow, the setup is active.

---

## 生效方式 / How CLAUDE.md Is Applied

- **自动生效 / Automatic loading**：在项目目录运行 `claude` 时，CC 会读取当前目录及上层目录中的 `CLAUDE.md`
- **多层叠加 / Layered rules**：子目录 `CLAUDE.md` 会叠加到父级规则上，局部规则覆盖同类通用规则
- **用户全局配置 / Global user config**：`~/.claude/CLAUDE.md` 可以放个人长期偏好

---

## 多层目录示例（适合 monorepo） / Multi-level Example for Monorepos

```text
your-project/
├── CLAUDE.md              # 全局规则 / shared project rules
├── frontend/
│   └── CLAUDE.md          # 前端局部规则 / frontend-specific additions
└── backend/
    └── CLAUDE.md          # 后端局部规则 / backend-specific additions
```

子目录只写该模块独有的事实和约束，公共规则留在根目录。

Put shared rules in the root, and only module-specific facts in child directories.

---

## 使用建议 / Recommended Usage Notes

- `CLAUDE.md` 里优先放**静态事实、目录结构、命令、约束、流程规则**
- 如果项目已经有自己的文档体系，不要强行替换，优先接到现有目录上
- 新功能推荐先有行为契约，再有技术设计，再进入 TDD 和实现
- 复杂修复推荐先走 `systematic-debugging`，先讲清根因，再决定怎么改
- 声称“完成 / 修复 / 可提交 / 可发布”前，先走 `verification-before-completion`
- 真正要做放行判断时，用 `requesting-code-review`，不要拿 `code-review` 代替

- Use `CLAUDE.md` for **static facts, structure, commands, constraints, and workflow rules**
- If the project already has docs, connect this template to them instead of replacing them blindly
- For new work, prefer behavior contract → technical design → TDD → implementation
- For complex bugs, prefer `systematic-debugging` first so root cause is clear before editing
- Before claiming “done / fixed / ready to commit / ready to release”, use `verification-before-completion`
- Use `requesting-code-review` for formal go / no-go review; do not substitute `code-review`

---

## 注意事项 / Important Notes

- 这套模板默认保留“行为契约优先 + 分阶段 Agent 调度 + TDD 驱动”的主框架
- 新增技能主要加强执行纪律，不是替换掉你原有的前半段流程
- 如果同时使用 GitHub Copilot，可以额外配置 `.github/copilot-instructions.md`
- Claude Code 不会自动读取 `.github/copilot-instructions.md`，但你可以在 `CLAUDE.md` 里显式引用它

- This template keeps contract-first, phased agent handoff, and TDD as the core identity
- The added skills strengthen execution discipline; they do not replace the front-half workflow
- If you also use GitHub Copilot, you can add `.github/copilot-instructions.md`
- Claude Code does not automatically read `.github/copilot-instructions.md`, but `CLAUDE.md` can point to it
