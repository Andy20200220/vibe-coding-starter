---
name: rate-limiting
description: '用于保护 API 端点不被过于频繁地调用。防止滥用、暴力破解攻击和意外过载。 / Use to protect API endpoints from being called too many times too quickly. Prevents abuse, brute-force attacks, and accidental overload. Keywords: vibe coding, rate limit, throttle, abuse, brute force, security, API protection, too many requests, non-technical.'
argument-hint: 'What to protect, e.g.: the login endpoint, all API endpoints, the send verification code button, the search feature'
user-invocable: true
---

# 频率限制技能 / Rate Limiting Skill

对端点或操作在给定时间窗口内可被调用的次数添加限制。防范暴力破解攻击、垃圾信息和意外过载。

Add limits to how many times an endpoint or action can be called in a given time window. Protects against brute-force attacks, spam, and accidental overload.

## 适用场景 / When to Use

- 登录或注册端点没有防止重复尝试的保护措施

- The login or registration endpoint has no protection against repeated attempts

- "发送验证码"按钮可能被点击数百次

- A "send verification code" button could be clicked hundreds of times

- API 端点可能被意外或恶意循环调用

- An API endpoint could be called in a loop by accident or maliciously

- 作为部署到生产环境前的基本安全加固步骤

- Before deploying to production as a basic security hardening step

## 不适用场景 / When Not to Use

- 应用纯粹是本地运行，没有服务器（没有需要保护的网络请求）

- The app is purely local with no server (no network requests to protect)

- 频率限制已就位 —— 请使用 `deployment-check` 验证其配置是否正确

- Rate limiting is already in place — use `deployment-check` to verify it is configured correctly

## 关键概念（大白话） / Key Concepts (plain language)

- **频率限制（Rate limit）：** 一条规则，例如"每个 IP 地址每分钟最多尝试登录 5 次"

- **Rate limit:** A rule like "maximum 5 login attempts per minute per IP address"

- **节流（Throttling）：** 减慢请求速度，而不是直接完全阻止

- **Throttling:** Slowing down requests instead of blocking them outright

- **429 请求过多（429 Too Many Requests）：** 超过频率限制时返回的标准 HTTP 响应

- **429 Too Many Requests:** The standard HTTP response when a rate limit is exceeded

- **基于 IP 的限制（IP-based limiting）：** 按访问者的 IP 地址应用的限制

- **IP-based limiting:** Limits applied per visitor's IP address

- **基于用户的限制（User-based limiting）：** 按已登录用户账户应用的限制

- **User-based limiting:** Limits applied per logged-in user account

## 流程 / Procedure

### 阶段一 —— 识别需要保护的端点 / Phase 1 — Identify endpoints to protect

按风险对所有 API 端点进行分类：

Categorize all API endpoints by risk:

| Risk | Endpoint type | Recommended limit |
|------|--------------|-------------------|
| Critical | Login, password reset, OTP send | 5 attempts / 15 minutes / IP |
| High | Registration, payment, account changes | 10 requests / hour / IP |
| Medium | Search, data creation | 60 requests / minute / user |
| Low | Read-only data, public pages | 300 requests / minute / IP |

询问用户确认业务特定的限制（例如，"每个用户每天最多发送 3 张发票"）。

Ask the user to confirm any business-specific limits (e.g., "each user can send 3 invoices per day").

### 阶段二 —— 选择实现方式 / Phase 2 — Choose the implementation approach

根据 `docs/design/tech-selection.md` 中的技术栈：

Based on the tech stack from `docs/design/tech-selection.md`:

**Node.js / Express：**
- `express-rate-limit` 包 —— 简单，适用于大多数情况
- 多服务器部署使用 Redis 支持的频率限制

**Node.js / Express:**
- `express-rate-limit` package — simple, works for most cases
- Redis-backed rate limiting for multi-server deployments

**Python / FastAPI or Flask：**
- `slowapi`（FastAPI）或 `Flask-Limiter`（Flask）

**Python / FastAPI or Flask:**
- `slowapi` (FastAPI) or `Flask-Limiter` (Flask)

**Next.js：**
- 使用 `@upstash/ratelimit` 的中间件方式频率限制（适用于 edge/serverless）
- 或使用自定义服务器的 `express-rate-limit`

**Next.js:**
- Middleware-based rate limiting using `@upstash/ratelimit` (for edge/serverless)
- Or `express-rate-limit` with a custom server

**在基础设施层面（推荐用于生产环境）：**
- Nginx 或 Cloudflare 频率限制 —— 在请求到达应用之前就提供保护

**At infrastructure level (recommended for production):**
- Nginx or Cloudflare rate limiting — protects before requests reach the app

推荐适合项目规模的最简单方案。

Recommend the simplest option that fits the project's scale.

### 阶段三 —— 实施 / Phase 3 — Implement

对于每个端点组：

For each endpoint group:

1. 安装频率限制包（如需要）
2. 配置规则：`最大请求数、时间窗口、键（IP 或用户 ID）`
3. 设置超出限制时的响应：
   - HTTP 429 状态码
   - 面向用户的消息："操作过于频繁。请等待 [时间] 后再试。"
   - 包含 `Retry-After` 响应头，值为距离限制重置的秒数
4. 添加到端点

1. Install the rate limiting package (if needed)
2. Configure the rule: `max requests, window duration, key (IP or user ID)`
3. Set the response for exceeded limits:
   - HTTP 429 status
   - User-facing message: "Too many attempts. Please wait [time] before trying again."
   - Include `Retry-After` header with seconds until the limit resets
4. Add to the endpoint

每次修改不超过 3 个文件。

Change no more than 3 files per step.

### 阶段四 —— 处理超出限制时的用户体验 / Phase 4 — Handle the user experience for exceeded limits

向用户展示的错误信息必须是：
- 清晰明确：告诉他们发生了什么以及需要等待多久
- 不泄露信息：不要透露允许的尝试次数（这会被攻击者利用）
- 一致统一：与应用中其他错误信息格式一致

The error message shown to users must be:
- Clear: tell them what happened and how long to wait
- Not revealing: do not disclose how many attempts are allowed (helps attackers)
- Consistent: same message format as other app errors

好的例子："登录尝试次数过多。请等待 15 分钟后再试。"
不好的例子："频率限制已超出：已使用 5/5 次。将于 847 秒后重置。"

Good: "Too many login attempts. Please wait 15 minutes before trying again."
Bad: "Rate limit exceeded: 5/5 attempts used. Resets in 847 seconds."

如果行为契约尚未记录用户在遇到频率限制时看到的内容，请更新行为契约。

Update the behavior contract if it does not yet document what users see when rate-limited.

### 阶段五 —— 测试限制 / Phase 5 — Test the limits

对于每个已配置的限制，验证：
1. 正常使用正常通过（未超过限制，没有错误）
2. 超过限制时返回 429 状态码及正确的消息
3. 窗口重置后，请求再次成功

For each configured limit, verify:
1. Normal use works (under the limit, no errors)
2. Exceeding the limit returns 429 with the correct message
3. After the window resets, requests succeed again

> "测试方法：[触发频率限制并验证响应的具体步骤]"

> "To test: [specific steps to trigger the rate limit and verify the response]"

### 阶段六 —— 保存 / Phase 6 — Save

> "频率限制已添加。提交信息：`Add rate limiting to [endpoints]`"

> "Rate limiting added. Commit: `Add rate limiting to [endpoints]`"

在 `docs/design/tech-selection.md` 的安全（Security）部分记录这些限制。

Document the limits in `docs/design/tech-selection.md` under a Security section.
