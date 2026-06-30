---
name: api-design
description: '当某个功能需要前后端通信，或本应用需要与外部服务交互时使用。在写任何代码之前设计 API 契约，防止反复修改接口。 / Use when a feature requires communication between frontend and backend, or between this app and an external service. Designs the API contract before any code is written, preventing repeated interface changes. Keywords: vibe coding, API, interface, endpoint, request, response, frontend, backend, contract, design, non-technical.'
argument-hint: '什么交互需要 API，例如：保存客户按钮需要调用后端、发票列表需要从服务器加载、集成支付服务 / What interaction needs an API, e.g.: the save customer button needs to call the backend, the invoice list needs to load from the server, integrate with a payment service'
user-invocable: true
---

# API 设计技能 / API Design Skill

在写任何代码之前，先设计前后端之间的接口（或本应用与外部服务之间的接口）。清晰的 API 契约可以防止因反复修改接口而浪费精力。

Design the interface between frontend and backend (or between this app and an external service) before writing any code. A clear API contract prevents wasted work from repeated interface changes.

## 何时使用 / When to Use

- 某个功能需要前端向后端获取或发送数据

- A feature requires the frontend to fetch or send data to the backend

- 需要新建一个后端接口

- A new backend endpoint needs to be created

- 本应用需要与第三方服务通信（支付、邮件、地图等）

- This app needs to talk to a third-party service (payment, email, maps, etc.)

- 前后端由不同的人分别开发

- The frontend and backend are being built separately or by different people

## 何时不使用 / When Not to Use

- 功能纯前端，无需与服务器交换数据

- The feature is entirely frontend-only (no data exchange with server)

- API 已存在，只需要正确调用——应使用「分步实现」技能

- An API already exists and just needs to be called correctly — use `guided-implementation` skill

- 调试一个出错的 API 调用——应使用「诊断并修复」流程

- Debugging a broken API call — use `Diagnose and Fix` prompt

## 操作步骤 / Procedure

### 第一阶段——了解交互需求 / Phase 1 — Understand the interaction

请用户用大白话描述功能：

Ask the user to describe the feature in plain language:

- 用户做了什么操作？（点保存、打开页面、提交表单）

- What does the user do? (click Save, open a page, submit a form)

- 什么数据需要发给服务器？（客户姓名、发票金额等）

- What data needs to go to the server? (customer name, invoice amount, etc.)

- 服务器需要返回什么？（确认信息、记录列表等）

- What does the server need to send back? (confirmation, a list of records, etc.)

- 如果服务器不可用或请求失败，该怎么办？

- What should happen if the server is unavailable or the request fails?

阅读 `docs/contracts/` 中相关的行为契约，了解 API 需要处理的所有情况。

Read the relevant behavior contract from `docs/contracts/` to understand all the cases the API must handle.

### 第二阶段——设计 API 契约 / Phase 2 — Design the API contract

为每个交互定义以下内容：

For each interaction, define:

**请求：**

**Request:**

- 方法：GET（读取数据）/ POST（创建）/ PUT/PATCH（更新）/ DELETE（删除）

- Method: GET (read data) / POST (create) / PUT/PATCH (update) / DELETE (remove)

- 路径：`/api/[资源]/[操作]`——使用简单的名词，路径中不要出现动词

- Path: `/api/[resource]/[action]` — use plain nouns, no verbs in the path

- 谁能调用：任何人 / 仅登录用户 / 仅管理员

- Who can call it: any user / logged-in user only / admin only

- 接收什么数据（请求体或 URL 参数）

- What data it receives (request body or URL parameters)

- 每个字段的数据类型和验证规则

- Data types and validation rules for each field

**响应：**

**Response:**

- 成功：返回什么数据，HTTP 状态码是什么

- Success: what data is returned, what HTTP status code

- 验证错误：哪些字段没通过、为什么

- Validation errors: which fields failed and why

- 业务规则错误：什么出了问题（重复、未找到、无权限）

- Business rule errors: what went wrong (duplicate, not found, unauthorized)

- 服务器错误：通用错误响应格式

- Server errors: generic error response format

**将契约格式化成用户可以审阅的表格：**

**Format the contract as a table the user can review:**

---
### API：[用大白话命名] / API: [Plain language name]

**它做什么：**[用一句话大白话描述] / **What it does:** [One sentence in plain language]

**请求：** / **Request:**
- 方法 + 路径：`POST /api/customers` / Method + Path: `POST /api/customers`
- 需要登录：是 / 否 / Requires login: Yes / No
- 发送的数据：/ Data sent:
  | 字段 / Field | 类型 / Type | 必填 / Required | 验证规则 / Validation |
  |-------|------|----------|-----------|
  | name | text | 是 / Yes | 1–100 个字符 / 1–100 characters |
  | phone | text | 是 / Yes | 有效手机号格式 / Valid phone format |

**响应——成功：** / **Response — success:**
- 状态码：201 已创建 / Status: 201 Created
- 返回：`{ id: 123, name: "你好", phone: "你好" }` / Returns: `{ id: 123, name: "...", phone: "..." }`

**响应——错误：** / **Response — errors:**
| 情况 / Situation | 状态码 / Status | 显示给用户的信息 / Message shown to user |
|-----------|--------|----------------------|
| 姓名为空 | 400 | "请输入姓名" |
| Name is empty | 400 | "Please enter a name" |
| 手机号格式不对 | 400 | "手机号格式无效" |
| Phone format wrong | 400 | "Phone number format is invalid" |
| 手机号已存在 | 409 | "此手机号已注册" |
| Phone already exists | 409 | "This phone number is already registered" |
| 服务器出问题 | 500 | "出了点问题，请重试" |
| Server problem | 500 | "Something went wrong, please try again" |

---

### 第三阶段——与用户一起审阅 / Phase 3 — Review with the user

用大白话逐个展示 API 契约。每个都这样确认：

Present each API contract in plain language. For each one ask:

> "当用户点击[按钮]时，应用会把[数据]发送给服务器。服务器会[做什么]，然后返回[结果]。如果[出错了]，用户会看到[提示信息]。这符合你的预期吗？"

> "When the user clicks [button], the app will send [data] to the server. The server will [do what] and send back [result]. If [error], the user will see [message]. Does this match what you expect?"

在继续之前解决所有"待定"项。不要在还有未解决问题时就实现 API。

Resolve any "TBD" items before proceeding. Do not implement an API with unresolved questions.

### 第四阶段——保存 API 契约 / Phase 4 — Save the API contract

保存到 `docs/design/api-contracts.md`（新建或追加）：

Save to `docs/design/api-contracts.md` (create or append):

```markdown
# API 契约 / API Contracts

## [功能名称] / [Feature Name]

### [接口名称] / [Endpoint name]

- 方法：/ Method: [METHOD] [/path]
- 需要认证：是/否 / Auth required: Yes/No
- 行为契约：docs/contracts/[feature].md / Behavior contract: docs/contracts/[feature].md

#### 请求 / Request
[表格 / table]

#### 响应 / Responses
[表格 / table]

#### 备注 / Notes
[特殊行为、速率限制、外部依赖等 / Any special behavior, rate limits, external dependencies]
```

### 第五阶段——说明下一步 / Phase 5 — State next step

> "API 契约已保存到 `docs/design/api-contracts.md`。准备好实现了吗？使用「分步实现」技能来逐步构建。"

> "API contract is saved to `docs/design/api-contracts.md`. Ready to implement? Use the `guided-implementation` skill to build this step by step."
