---
name: error-logging
description: '用于设置错误日志，以便在生产环境中出现问题时，有记录可查发生了什么。没有日志，线上应用的 bug 就如隐形一般不可见。 / Use to set up error logging so when something breaks in production, there is a record of what happened. Without logging, bugs in live apps are invisible. Keywords: vibe coding, logging, error tracking, production, debug, monitor, alert, non-technical, visibility.'
argument-hint: 'What to log, e.g.: set up basic error logging, track API errors, log when payments fail, set up error alerts'
user-invocable: true
---

# 错误日志技能 / Error Logging Skill

设置日志记录，让生产环境中的错误留下痕迹。当应用上线后出现问题时，日志会准确告诉你哪里出了问题，让你能快速修复，而不是靠猜。

Set up logging so errors in production leave a trail. When something breaks after the app is live, logs tell you exactly what went wrong so you can fix it quickly instead of guessing.

## 适用场景 / When to Use

- 在首次部署应用之前

- Before deploying the app for the first time

- 在生产环境中发现了一个 bug，但因为没有任何日志而无法定位

- After a bug was discovered in production that was invisible without logs

- 应用开始出现异常行为，但完全不知道原因

- When the app starts behaving strangely and there is no way to find out why

- 关键操作（支付、数据保存）需要审计追踪

- When critical actions (payments, data saves) need an audit trail

## 不适用场景 / When Not to Use

- 在开发环境中调试已知 bug —— 请改用 `诊断并修复（Diagnose and Fix）` 提示

- Debugging a known bug in development — use `Diagnose and Fix` prompt instead

- 应用尚未部署，日志不是当前优先事项

- The app has not been deployed yet and logging is not a priority

## 关键概念（大白话） / Key Concepts (plain language)

- **日志（Log）：** 保存下来的记录，记录了应用发生了什么（就像应用的日记本）

- **Log:** A saved record of what happened (like a diary for the app)

- **错误日志（Error log）：** 记录什么时候、为什么出了问题

- **Error log:** A record of when and why something went wrong

- **日志级别（Log level）：** 事件的严重程度（INFO = 正常活动，WARN = 出现异常，ERROR = 出故障了）

- **Log level:** How serious the event is (INFO = normal activity, WARN = something odd, ERROR = something broke)

- **日志目标位置（Log destination）：** 日志保存到哪里（控制台、文件、云服务）

- **Log destination:** Where logs are saved (console, a file, a cloud service)

## 流程 / Procedure

### 阶段一 —— 了解背景 / Phase 1 — Understand the context

阅读 `docs/design/tech-selection.md` 了解技术栈和部署环境。

Read `docs/design/tech-selection.md` to understand the tech stack and deployment environment.

询问以下问题：

Ask:

- 这个应用已经部署了，还是正在为首次部署做准备？

- Is this app already deployed, or being set up for first deployment?

- 是否有某些操作出错时代价最大？（支付、数据保存、认证）

- Are there specific actions where errors are most costly? (payments, data saves, auth)

- 用户想要简单的文件/控制台日志，还是要监控服务？

- Does the user want simple file/console logging or a monitoring service?

对于大多数小型项目：从结构化控制台日志 + 免费服务（如 Sentry 或 Logtail）开始。

For most small projects: start with structured console logging + a free service like Sentry or Logtail.

### 阶段二 —— 确定要记录什么 / Phase 2 — Define what to log

将需要记录的事件分类：

Categorize events to log:

**始终记录（ERROR 级别）：**

**Always log (ERROR level):**

- 未处理的异常和崩溃

- Unhandled exceptions and crashes

- 数据库连接失败

- Database connection failures

- 外部 API 调用失败（支付网关、邮件服务等）

- Failed external API calls (payment gateway, email service, etc.)

- 看起来像攻击的认证失败（同一 IP 多次登录失败）

- Auth failures that look like attacks (many failed logins from the same IP)

- 静默失败的数据写入

- Data writes that failed silently

**有用时记录（WARN 级别）：**

**Log when helpful (WARN level):**

- 慢速数据库查询（超过 1 秒）

- Slow database queries (over 1 second)

- 使用了已废弃的功能

- Deprecated feature usage

- 被验证拒绝的异常输入值

- Unusual input values that were rejected by validation

- 频繁访问数据的缓存未命中

- Cache misses on frequently accessed data

**用于审计追踪（INFO 级别）：**

**Log for audit trail (INFO level):**

- 用户登录 / 登出

- User login / logout

- 记录创建 / 更新 / 删除（包含谁在什么时候操作的）

- Record created / updated / deleted (with who did it and when)

- 支付交易（金额、状态，但不记录卡号）

- Payment transactions (amount, status, not card numbers)

- 管理员操作

- Admin actions

**永远不要记录：**

**Never log:**

- 密码（任何形式）

- Passwords (in any form)

- 完整信用卡号

- Full credit card numbers

- 会话令牌或 API 密钥

- Session tokens or API keys

- 超出审计所需的完整个人数据

- Full personal data beyond what is needed for the audit

### 阶段三 —— 实施日志 / Phase 3 — Implement logging

**步骤 1：添加日志工具模块**
创建一个整个应用共用的日志模块。这样可以确保格式一致，以后更改日志目标位置也很方便。

**Step 1: Add a logging utility**
Create a single logging module the whole app uses. This ensures consistent format and makes it easy to change the logging destination later.

日志格式应包含：
- 时间戳
- 级别（ERROR/WARN/INFO）
- 消息
- 上下文（哪个用户、哪个记录 ID、哪个操作）
- 对于错误：错误消息和堆栈跟踪

Log format should include:
- Timestamp
- Level (ERROR/WARN/INFO)
- Message
- Context (which user, which record ID, which action)
- For errors: the error message and stack trace

**步骤 2：为关键路径添加错误日志**
用 try/catch 包裹以下操作并添加日志：
- 数据库操作
- 外部 API 调用
- 文件操作
- 认证操作

**Step 2: Add error logging to critical paths**
Wrap these in try/catch with logging:
- Database operations
- External API calls
- File operations
- Auth operations

**步骤 3：添加全局错误处理**
在顶层捕获未处理的错误，确保没有任何崩溃是静默发生的。

**Step 3: Add a global error handler**
Catch unhandled errors at the top level so nothing crashes silently.

**步骤 4：（可选）连接到监控服务**
对于生产环境应用，连接到免费层级的服务：
- **Sentry** —— 自动捕获并报告错误，有免费层级
- **Logtail / BetterStack** —— 日志聚合和搜索，有免费层级

**Step 4: (Optional) Connect to a monitoring service**
For production apps, connect to a free-tier service:
- **Sentry** — catches and reports errors automatically, free tier available
- **Logtail / BetterStack** — log aggregation and search, free tier available

向用户解释：

> "Sentry 就像给你的应用装了一个监控摄像头。当出现问题时会自动记录发生了什么、影响了谁，并通知你。十分钟左右就能搞定。"

Explain to the user:
> "Sentry works like a security camera for your app. When something breaks, it automatically records exactly what happened, who was affected, and sends you an alert. You can set it up in about 10 minutes."

### 阶段四 —— 测试日志 / Phase 4 — Test the logging

（在测试环境中）故意触发一个错误，然后验证：
- 错误被捕获了
- 日志条目包含足够的信息来理解发生了什么问题
- 敏感数据没有出现在日志中
- 面向用户的错误消息是友好的，不是原始堆栈跟踪

Intentionally trigger an error (in a test environment) and verify:
- The error is captured
- The log entry contains enough information to understand what went wrong
- Sensitive data is NOT in the log
- The user-facing error message is friendly, not a raw stack trace

### 阶段五 —— 记录告警阈值 / Phase 5 — Document alert thresholds

如果使用了监控服务，为以下情况设置告警：
- 任何 ERROR 级别事件 → 立即通知
- 每分钟超过 10 个 WARN 事件 → 通知
- 长时间没有任何记录（应用可能宕机了） → 通知

If using a monitoring service, set up alerts for:
- Any ERROR-level event → immediate notification
- More than 10 WARN events per minute → notification
- Zero activity for an extended period (app may be down) → notification

### 阶段六 —— 保存 / Phase 6 — Save

> "错误日志已设置完毕。提交信息：`Add error logging and monitoring`"

> "Error logging set up. Commit: `Add error logging and monitoring`"

更新 `docs/design/tech-selection.md`，记录当前使用的日志方案。

Update `docs/design/tech-selection.md` to record what logging solution is in use.
