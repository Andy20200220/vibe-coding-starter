# 行为契约编写规范

> 本文件放在 `docs/contracts/` 目录下，Codex 在 contracts/ 内写契约时自动生效。

## 语言要求 / Language Requirement

- **所有内容必须中英双语**：每个标题、说明、条目都同时提供中文和英文
- 格式：中文在前，英文紧跟其后（括号或换行均可）
- 示例：`## 登录功能 / Login Feature`

## 文件命名 / File Naming

- 一份契约一个功能，文件名：`[功能名].md`（如 `login.md`、`user-profile.md`）
- 产品定义放在 `product-definition.md`

## 契约结构 / Contract Structure

每份行为契约必须包含以下段落：

```markdown
# 行为契约：[功能名] / Behavior Contract: [Feature Name]

状态 / Status：已确认 / Confirmed | 部分确认（N 条待定）
日期 / Date：YYYY-MM-DD

## 用户操作与系统响应 / User Actions and System Responses

| # | 用户操作 | 正常响应 | 错误/边界情况 | 验证 |
|---|---------|---------|-------------|------|
| 1 | 用户做什么 | 系统正常反应 | 出错时的反应 | 怎么验证 |

## 待定事项 / TBD Items

- 第 N 条：需要决策的内容描述

## 备注 / Notes

- 与用户讨论过的假设或约束
```

## 条目编写规范 / Entry Writing Rules

- 每个用户操作必须有对应的"正常响应""错误/边界情况""验证"三列
- 验证步骤必须可操作：打开哪个页面 → 输入什么 → 点击什么 → 看到什么
- 边界情况至少覆盖：空值、最大值、首次操作、已存在、网络异常
- 用大白话写，不出现代码或技术术语

## 状态标记 / Status Markers

- 已确认 / Confirmed — 所有条目用户已确认
- 部分确认 / Partially Confirmed — 部分条目待定
- 待定标记：TBD — 需要决策 / needs decision