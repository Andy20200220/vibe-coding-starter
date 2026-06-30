# 行为契约存档 / Behavior Contracts

本目录存放各功能的行为契约。行为契约用大白话定义每个用户操作下系统该做什么。

This directory stores behavior contracts for each feature. A behavior contract defines exactly what the system should do for each user action, in plain language.

## 工作方式 / How It Works

- 一个功能一个文件（如 `login.md`、`customer-list.md`）/ One file per feature (e.g., `login.md`, `customer-list.md`)
- `product-definition.md` 存放整体产品定义和功能列表 / `product-definition.md` holds the overall product definition and feature list
- 每份契约须经用户逐条审阅确认后，方可开始实现 / Each contract is reviewed and confirmed by the user before implementation begins
- 修 bug 时，AI 先查对应契约来确定"正确行为" / During bug fixing, AI checks the relevant contract to determine "correct behavior"

## 文件状态 / File Status

每份契约文件都有一个状态 / Each contract file has a status：

- **草稿 / Draft** — 已撰写，用户尚未审阅 / written but not yet reviewed by user
- **已确认 / Confirmed** — 已审阅通过，可以开始实现 / reviewed and approved, ready for implementation
- **已实现并验证 / Implemented and verified** — 功能已构建且工作正常 / feature is built and working
- **需更新 / Needs update** — 行为已变更，契约需要修订 / behavior has changed, contract needs revision