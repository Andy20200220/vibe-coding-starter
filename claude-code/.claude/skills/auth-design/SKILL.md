---
name: auth-design
description: '当某个功能需要用户登录、注册或权限控制时使用。在写任何代码之前设计认证和授权体系，涵盖登录流程、会话管理和基于角色的权限控制。 / Use when a feature requires user login, registration, or access control. Designs the authentication and authorization system before writing any code, covering login flows, session management, and role-based permissions. Keywords: vibe coding, auth, login, register, permissions, roles, session, token, security, non-technical.'
argument-hint: '需要什么认证功能，例如：用户需要登录、某些页面只对管理员可见、用户只能看到自己的数据 / What auth is needed, e.g.: users need to log in, some pages should only be visible to admins, users should only see their own data'
user-invocable: true
---

# 认证设计技能 / Auth Design Skill

在写代码之前设计登录、注册和权限体系。涵盖用户流程、会话管理、基于角色的访问控制和安全要求。

Design login, registration, and permission systems before writing code. Covers user flows, session handling, role-based access control, and security requirements.

## 何时使用 / When to Use

- 某个功能需要用户登录后才能访问

- A feature requires users to log in before accessing it

- 不同用户应该看到或做不同的事情（管理员 vs 普通用户）

- Different users should see or do different things (admin vs. regular user)

- 用户应该只能访问自己的数据，而不能看别人的

- Users should only be able to access their own data, not other users' data

- 需要注册或账号管理功能

- Registration or account management features are needed

## 何时不使用 / When Not to Use

- 认证功能已正常工作，你只需要添加新功能——应使用「行为契约」技能

- Auth is already working and you just need to add a feature — use `behavior-contract` skill

- 存在认证相关的 bug——应使用「诊断并修复」流程

- An auth bug exists — use `Diagnose and Fix` prompt

## 关键安全原则 / Critical Security Principles

以下原则不可妥协。任何违反这些原则的认证实现必须在发布前修复：

These are non-negotiable. Any auth implementation that violates them must be fixed before shipping:

1. **绝不存储明文密码**——始终使用 bcrypt 或等效算法进行哈希

   **Never store plain-text passwords** — always hash with bcrypt or equivalent

2. **绝不只在后端做认证**（注：原文是 frontend，疑为笔误）——始终在服务器端验证

   **Never put auth logic only on the frontend** — always verify on the server

3. **生产环境使用 HTTPS**——绝不通过明文 HTTP 传输凭据

   **Use HTTPS in production** — never send credentials over plain HTTP

4. **令牌必须有过期时间**——会话和 JWT 必须设置有效期

   **Tokens must expire** — sessions and JWTs must have expiry times

5. **限制登录尝试频率**——防止暴力破解攻击

   **Rate-limit login attempts** — prevent brute-force attacks

## 操作步骤 / Procedure

### 第一阶段——了解需求 / Phase 1 — Understand the requirements

询问用户：

Ask the user:

- 有哪些不同类型的用户？（例如：普通用户、管理员、访客）

- Who are the different types of users? (e.g., regular users, admins, guests)

- 每种类型的用户可以做什么？不允许做什么？

- What can each type of user do? What are they NOT allowed to do?

- 用户自己注册，还是由管理员创建账号？

- Should users register themselves, or does an admin create accounts?

- 用户如何登录？（邮箱+密码、手机号+验证码、Google 等社交登录）

- How do users log in? (email + password, phone + OTP, social login like Google)

- 登录会话持续多长时间？（直到退出登录、24 小时、30 天）

- How long should a login session last? (until they log out, 24 hours, 30 days)

- 会话过期后会发生什么？（跳转到登录页、要求重新认证）

- What happens when a session expires? (redirected to login, asked to re-authenticate)

### 第二阶段——设计认证流程 / Phase 2 — Design the auth flows

用大白话记录每个流程：

Document each flow in plain language:

**注册流程：**

**Registration flow:**

- 用户需要提供什么信息？

- What information does the user provide?

- 每个字段运行什么验证？

- What validation runs on each field?

- 是否需要邮箱/手机验证？

- Is email/phone verification required?

- 注册成功后立即发生什么？

- What happens immediately after successful registration?

**登录流程：**

**Login flow:**

- 用户输入什么凭据？

- What credentials does the user enter?

- 失败多少次后锁定？

- How many failed attempts before lockout?

- 锁定时长是多久？

- How long is the lockout?

- 是否支持"记住我"？技术上它做了什么？

- Is "remember me" supported? What does it do technically?

**会话管理：**

**Session management:**

- 会话存储在哪里？（cookie、localStorage、服务器端）

- Where is the session stored? (cookie, localStorage, server-side)

- 会话过期时间是多少？

- What is the session expiry?

- 退出登录时发生什么？（清除会话、跳转）

- What happens on logout? (clear session, redirect)

**密码重置流程：**

**Password reset flow:**

- 用户如何请求重置？

- How does the user request a reset?

- 重置链接/验证码如何送达？

- How is the reset link/code delivered?

- 重置链接有效多久？

- How long is the reset link valid?

- 同一个重置链接能用两次吗？

- Can the same reset link be used twice?

### 第三阶段——设计权限（如果有多种角色） / Phase 3 — Design permissions (if multiple roles)

创建权限矩阵：

Create a permission matrix:

| 操作 / Action | 访客 / Guest | 普通用户 / Regular User | 管理员 / Admin |
|--------|-------|-------------|-------|
| 查看公开页面 | ✅ | ✅ | ✅ |
| View public pages | ✅ | ✅ | ✅ |
| 查看自己的数据 | ❌ | ✅ | ✅ |
| View own data | ❌ | ✅ | ✅ |
| 查看所有用户数据 | ❌ | ❌ | ✅ |
| View all users' data | ❌ | ❌ | ✅ |
| 创建记录 | ❌ | ✅ | ✅ |
| Create records | ❌ | ✅ | ✅ |
| 删除任意记录 | ❌ | ❌ | ✅ |
| Delete any record | ❌ | ❌ | ✅ |

对每个受保护的资源，定义：

For each protected resource, define:

- 谁能读？

- Who can read it?

- 谁能创建？

- Who can create it?

- 谁能编辑？

- Who can edit it?

- 谁能删除？

- Who can delete it?

### 第四阶段——安全清单 / Phase 4 — Security checklist

实现前确认：

Before implementation, confirm:

- [ ] 密码将被哈希处理（bcrypt、Argon2 或等效算法）

- [ ] Passwords will be hashed (bcrypt, Argon2, or equivalent)

- [ ] 登录尝试将被频率限制

- [ ] Login attempts will be rate-limited

- [ ] 会话将有过期时间

- [ ] Sessions will have expiry

- [ ] 受保护路由在服务器端验证，不只是前端

- [ ] Protected routes verified on server, not just frontend

- [ ] 敏感操作需要重新认证（例如：修改密码、删除账号）

- [ ] Sensitive actions require re-authentication (e.g., changing password, deleting account)

- [ ] URL 和日志中不出现敏感数据

- [ ] No sensitive data in URLs or logs

### 第五阶段——保存设计方案 / Phase 5 — Save the design

保存到 `docs/design/auth-design.md`：

Save to `docs/design/auth-design.md`:

- 用户角色和权限矩阵

- User roles and permissions matrix

- 每个流程的记录

- Each flow documented

- 安全决策及其理由

- Security decisions and reasoning

### 第六阶段——下一步 / Phase 6 — Next step

> "认证设计方案已保存。下一步：使用「行为契约」技能为每个认证功能（登录、注册、密码重置）编写详细的行为契约，然后用「分步实现」技能来实现。"

> "Auth design is saved. Next step: use `behavior-contract` skill to write the detailed behavior contracts for each auth feature (login, register, password reset), then implement with `guided-implementation`."
