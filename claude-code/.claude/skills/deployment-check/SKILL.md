---
name: deployment-check
description: '发布或分享项目前检查是否就绪：环境变量已设置、密钥未暴露、依赖正确、应用在目标环境中正常运行。 / Use before publishing or sharing a project to make sure it is ready: environment variables are set, secrets are not exposed, dependencies are correct, and the app works in the target environment. Keywords: vibe coding, deploy, publish, go live, release, checklist, production, environment, non-technical, pre-launch.'
argument-hint: '部署目标，例如：分享给朋友、放到 Vercel 上、在另一台电脑上运行、发布到互联网 / Where you are deploying to, e.g.: share with a friend, put on Vercel, run on another computer, publish to the internet'
user-invocable: true
---

# 发布前检查技能 / Deployment Check Skill

运行发布前检查清单，确保项目安全且准备好分享或发布。在部署前用大白话报告问题。

Run a pre-deployment checklist to make sure the project is safe and ready to share or publish. Reports issues in plain language before anything is deployed.

## 何时使用 / When to Use

- 在将项目分享给任何人之前

- Before sharing the project with anyone

- 在将项目部署到托管服务（Vercel、Netlify、Railway 等）之前

- Before putting the project on a hosting service (Vercel, Netlify, Railway, etc.)

- 在另一台电脑上运行项目之前

- Before running the project on a different computer

- 当项目在本地可以运行但在其他机器上失败时

- When something works locally but fails on another machine

## 何时不使用 / When Not to Use

- 项目仍在开发中，尚未准备好分享

- The project is still in development and not ready for sharing

- 部署后出现了特定错误——此时应使用「诊断并修复」流程

- A specific error has occurred after deployment — use `Diagnose and Fix` prompt instead

## 操作步骤 / Procedure

### 第一阶段——了解部署目标 / Phase 1 — Understand the target

如果用户未指定，则询问：

Ask if not specified:

> "你要部署到哪里？
> (a) 分享给某人，让他在自己电脑上运行
> (b) 托管到线上（Vercel、Netlify、Railway、Heroku 等）
> (c) 在你自己的服务器上运行
> (d) 只是在演示前确认它能正常工作"

> "Where are you deploying this?
> (a) Sharing with someone to run on their computer
> (b) Hosting online (Vercel, Netlify, Railway, Heroku, etc.)
> (c) Running on your own server
> (d) Just making sure it works before a demo"

阅读 `docs/design/tech-selection.md` 了解技术栈和部署上下文。

Read `docs/design/tech-selection.md` to understand the tech stack and deployment context.

### 第二阶段——执行检查清单 / Phase 2 — Run the checklist

逐项检查以下各类。为每个问题评级：🔴 阻塞项 / 🟡 风险项 / 🟢 小问题。

Check each category below. Rate each issue: 🔴 Blocker / 🟡 Risk / 🟢 Minor.

**A. 密钥和凭据——不暴露任何敏感信息 / Secrets and credentials — nothing sensitive exposed**

- 源代码中是否有硬编码的 API 密钥、密码或令牌？

- Are there hardcoded API keys, passwords, or tokens in source files?

- 是否存在包含密钥的 `.env` 文件？它是否在 `.gitignore` 中列出？

- Is there a `.env` file with secrets? Is it listed in `.gitignore`?

- 项目是否从环境变量中读取密钥，而非硬编码的值？

- Does the project read secrets from environment variables, not hardcoded values?

- 是否有 `console.log` 语句打印了敏感数据？

- Are there any `console.log` statements printing sensitive data?

**B. 环境配置——在你的电脑之外也能正常运行 / Environment configuration — works outside your computer**

- 所有必需的环境变量是否已记录文档？（例如，在 `.env.example` 文件中）

- Are all required environment variables documented? (e.g., in a `.env.example` file)

- 是否存在只在你电脑上有效的硬编码文件路径？

- Are there hardcoded file paths that only exist on your machine?

- 是否存在硬编码的 `localhost` URL，需要改为生产环境地址？

- Are there hardcoded `localhost` URLs that need to change for production?

- 应用在生产环境和开发环境是否连接到正确的数据库/服务？

- Does the app connect to the right database/service in production vs. development?

**C. 依赖——完整且正确 / Dependencies — complete and correct**

- `package.json`（或等效文件）是否列出了所有必需的依赖？

- Does `package.json` (or equivalent) list all required dependencies?

- 是否存在本地已安装但未写入依赖文件的包？

- Are there packages installed locally but not in the dependency file?

- 依赖版本是否已锁定，还是使用了可能出问题的宽松版本范围？

- Are dependency versions pinned or are they using loose ranges that could break?

**D. 构建和启动——从零开始能干净地运行 / Build and start — runs cleanly from scratch**

- `npm install`（或等效命令）是否能无错完成？

- Does `npm install` (or equivalent) complete without errors?

- 构建命令是否成功？

- Does the build command succeed?

- 在新环境中应用是否能无错启动？

- Does the app start without errors on a fresh setup?

- 启动命令是否已记录在文档中（README 或部署指南）？

- Is the start command documented somewhere (README or deployment guide)?

**E. 数据和存储——数据正确持久化 / Data and storage — data persists correctly**

- 数据库在新环境中是否能正确初始化？

- Does the database initialize correctly on a fresh setup?

- 文件存储路径是否为相对路径（而非硬编码到你电脑上的路径）？

- Are file storage paths relative (not hardcoded to your machine)?

- 必需的数据库迁移文件是否包含在内？

- Are required database migration files included?

**F. 错误处理——失败时优雅降级 / Error handling — failures are handled gracefully**

- API 错误是否显示用户友好的提示信息，而不是直接崩溃？

- Do API errors show a user-friendly message instead of crashing?

- 外部服务不可用时是否有备用方案？

- Is there a fallback if an external service is unavailable?

**G. 最终功能检查 / Final feature check**

- 按照 `docs/verification/` 中的验证清单，逐一检查所有 MVP 功能

- Run through the verification checklists in `docs/verification/` for all MVP features

- 确认所有功能都按照其行为契约中的规定正常工作

- Confirm all features work as specified in their behavior contracts

### 第三阶段——生成报告 / Phase 3 — Report

---
## 部署就绪报告 / Deployment Readiness Report

目标：[部署目标] / Target: [deployment target]
日期：[今天] / Date: [today]

### 总体状态 / Overall Status
🔴 未就绪 / 🟡 有保留地就绪 / ✅ 可以部署
🔴 Not ready / 🟡 Ready with caveats / ✅ Ready to deploy

### 发现的问题 / Issues Found

#### 🔴 阻塞项——部署前必须修复 / Blockers — fix before deploying
| # | 问题 / Issue | 如何修复 / How to fix |
|---|-------|-----------|
| 1 | ... | ... |

#### 🟡 风险项——强烈建议修复 / Risks — strongly recommended to fix
| # | 问题 / Issue | 如何修复 / How to fix |
|---|-------|-----------|

#### 🟢 小问题——可选的改进项 / Minor — optional improvements
| # | 问题 / Issue | 如何修复 / How to fix |
|---|-------|-----------|

### 部署步骤 / Deployment Steps
[针对目标平台的逐步操作说明，用大白话写 / Step-by-step instructions specific to the target platform, in plain language]

---

### 第四阶段——修复并部署 / Phase 4 — Fix and deploy

> "你想让我现在修复这些阻塞项吗？我会逐一处理。"

> "Would you like me to fix the blockers now? I'll handle them one at a time."

解决所有阻塞项后，提供针对目标平台的具体部署步骤。

After all blockers are resolved, provide the exact deployment steps for the target platform.
