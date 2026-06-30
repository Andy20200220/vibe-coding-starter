# 架构设计文档编写规范

> 本文件放在 `docs/design/` 目录下，Codex 在 design/ 内写设计文档时自动生效。

## 语言要求 / Language Requirement

- **所有内容必须中英双语**：标题、说明、条目同时提供中文和英文
- 格式：中文在前，英文紧跟其后

## 文件命名 / File Naming

- 一份设计文档对应一个功能，文件名：`[功能名]-architecture.md`
- 技术选型文档：`tech-selection.md`
- 用户流程文档：`user-flows.md`

## 技术选型文档 / Tech Selection Document

`docs/design/tech-selection.md` 必须包含：

```markdown
# 技术选型 / Technology Selection

日期 / Date：YYYY-MM-DD

## 约束条件 / Constraints
- 用户的限制条件（如：必须在 Windows 运行、不需要服务器等）

## 推荐方案 / Recommended Stack
| 层级 / Layer | 选择 / Choice | 理由 / Rationale |
|-------------|-------------|----------------|
| 前端框架 | React | 用大白话解释为什么选它 |
| 样式 | Tailwind CSS | ... |
| 数据库 | SQLite | ... |

## 备选方案 / Alternatives Considered
| 方案 / Option | 不选的理由 / Why Not |
|-------------|-------------------|
| Vue | ... |
```

## 功能架构文档 / Feature Architecture Document

`docs/design/[功能名]-architecture.md` 必须包含以下六个段落：

### 1. 影响范围 / Impact Scope

```markdown
## 影响范围 / Impact Scope

| 改动类型 / Change Type | 文件 / File | 说明 / Description |
|----------------------|-----------|------------------|
| 新增 / New | src/pages/login.tsx | 登录页面 |
| 修改 / Modify | src/services/auth.ts | 增加登录接口 |
| 不变 / No Change | src/pages/home.tsx | 不受影响 |
```

### 2. 数据流 / Data Flow

用大白话描述数据怎么流转，画出关键路径：

> 用户点击登录按钮 → 前端调 /api/login → 后端查数据库 → 返回 token → 前端存到 localStorage → 跳转首页

### 3. 接口契约 / Interface Contract

每个 API 接口必须定义：

```markdown
## 接口 / API：POST /api/login

请求 / Request：
| 字段 / Field | 类型 / Type | 必填 / Required | 说明 / Description |
|-------------|-----------|---------------|------------------|
| username | string | 是 | 用户名 |
| password | string | 是 | 密码 |

成功响应 / Success Response (200)：
| 字段 | 类型 | 说明 |
|-----|-----|-----|
| token | string | 登录凭证 |
| user | object | 用户信息 |

错误响应 / Error Responses：
| 状态码 / Status | 说明 / Description |
|---------------|------------------|
| 401 | 用户名或密码错误 |
| 429 | 登录尝试过多，请稍后再试 |
```

### 4. 边界处理 / Edge Handling

```markdown
## 边界处理 / Edge Handling

| 情况 / Scenario | 处理策略 / Strategy |
|---------------|------------------|
| 用户已登录再次访问登录页 | 直接跳转首页 |
| 网络超时 | 提示"网络不给力，请重试"，保留已输入内容 |
| 并发登录 | 后一次登录使前一次 token 失效 |
| 密码为空 | 前端拦截，不发送请求 |
```

### 5. 验证方式 / Verification Approach

```markdown
## 验证方式 / Verification Approach

- 单元测试：auth.ts 的登录函数覆盖正常/异常路径
- 集成测试：通过 Playwright 模拟完整登录流程
- 手工验证：打开登录页 → 输入错误密码 → 确认看到错误提示 → 输入正确密码 → 确认跳转首页
```

### 6. 交付边界 / Delivery Boundary

```markdown
## 交付边界 / Delivery Boundary

本次交付包含 / In Scope：
- 用户名+密码登录

本次不包含 / Out of Scope：
- 第三方登录（微信、Google）
- 密码找回
- 记住我功能
```