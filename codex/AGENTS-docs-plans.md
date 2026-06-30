# 任务计划与执行日志编写规范

> 本文件放在 `docs/plans/` 目录下，Codex 在 plans/ 内写计划和日志时自动生效。

## 语言要求 / Language Requirement

- **所有内容必须中英双语**
- `execution-log.md` 可以只用中文（它是操作记录，不是交付文档）

---

## 一、任务计划文档 / Task Plan Document

### 文件命名

- `docs/plans/[功能名]-plan.md`

### 计划结构

```markdown
# 任务计划：[功能名] / Task Plan: [Feature Name]

对应契约 / Contract：docs/contracts/[功能名].md
对应设计 / Design：docs/design/[功能名]-architecture.md
创建日期 / Created：YYYY-MM-DD
状态 / Status：执行中 / In Progress | 已完成 / Completed

## 步骤列表 / Steps

| 步骤 / Step | 目标 / Goal | 涉及文件 / Files | 依赖 / Depends On | 验证方法 / How to Verify | 状态 / Status |
|-----------|-----------|----------------|-----------------|---------------------|-------------|
| 1 | 创建登录页面骨架 | login.tsx | 无 | 打开页面看到表单 | ✅ |
| 2 | 实现登录接口调用 | auth.ts, login.tsx | 步骤1 | 输入正确密码后跳转首页 | 🔄 |
| 3 | 添加错误提示 | login.tsx | 步骤2 | 输入错误密码看到提示 | ⬜ |

状态标记：⬜ 待做 / 🔄 进行中 / ✅ 已完成 / ❌ 阻塞
```

### 规则

- 每个步骤最多改 3 个文件
- 步骤之间依赖关系必须标注
- 每一步都有可操作的验证方法
- 计划写完须和用户确认后再开始实现

---

## 二、执行日志 / Execution Log

### 文件

- `docs/plans/execution-log.md`（只有一个文件，持续追加）

### 日志格式

```markdown
# 执行日志 / Execution Log

| 时间 / Time | Agent | 完成了什么 / What Was Done | 产出文件 / Output Files |
|------------|-------|-------------------------|---------------------|
| 2026-06-30 14:30 | product-agent | 为登录功能撰写行为契约 | docs/contracts/login.md |
| 2026-06-30 15:00 | syseng-agent | 输出登录功能架构设计 | docs/design/login-architecture.md |
| 2026-06-30 15:30 | planning-agent | 拆解登录功能为 3 个步骤 | docs/plans/login-plan.md |
| 2026-06-30 16:00 | dev-agent | 实现步骤1：登录页面骨架 | src/pages/login.tsx |
```

### 规则

- **每完成一个实质步骤立刻追加，不等最后拼凑**
- **时间必须用 `Get-Date -Format "yyyy-MM-dd HH:mm"` 获取真实时间**
- 简单沟通、回答问题不需要记录
- 有实质产出（新建文件、修改代码、通过测试）才记录
- 一条记录一行，不合并多步骤

---

## 三、问题清单 / Issue Tracker

### 文件

- `docs/issue-tracker.md`（只有一个文件，持续维护）

### 格式

```markdown
# 问题清单 / Issue Tracker

| # | 发现时间 | 问题描述 | 状态 | 修复时间 | 修复说明 |
|---|---------|---------|------|---------|---------|
| 1 | 06-30 14:00 | 登录页在手机上看不到按钮 | ✅ | 06-30 16:00 | 给按钮加了移动端适配样式 |
| 2 | 06-30 16:30 | 输错密码没提示 | 🔧 | - | 等待用户验证 |

状态标记：🔬 待分析 / ❌ 未修复 / 🔧 修复待验证 / ✅ 已修复
```

### 规则

- 发现新问题立即添加
- 修复完成后立即更新状态
- 状态为 `🔧` 的条目，用户验证通过后才能改成 `✅`