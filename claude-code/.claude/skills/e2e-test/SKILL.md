---
name: e2e-test
description: '验证完整的用户操作流程是否从头到尾都能正常工作，通过模拟真实用户交互：点击按钮、填写表单、浏览页面。能发现单元测试遗漏的 bug，因为它测试整个系统协同工作的情况。 / Use when you want to verify complete user journeys work end-to-end by simulating real user interactions: clicking buttons, filling forms, navigating pages. Catches bugs that unit tests miss because they test the whole system together. Keywords: vibe coding, e2e, end-to-end, integration test, user journey, simulate, browser, Playwright, Cypress, non-technical.'
argument-hint: 'What user journey to test, e.g.: test the full login flow, test creating and saving a customer, test the complete invoice creation process'
user-invocable: true
---

# 端到端测试技能 / E2E Test Skill

编写和运行端到端测试，模拟真实用户在应用中点击操作。这些测试能发现单元测试遗漏的 bug，因为它们测试整个系统协同工作 —— 前端、后端和数据库一起跑。

Write and run end-to-end tests that simulate a real user clicking through the app. These tests catch bugs that unit tests miss because they test the whole system working together — frontend, backend, and database.

## 何时使用 / When to Use

- 关键用户流程（登录、结账、数据创建）需要自动化验证

- A critical user journey (login, checkout, data creation) needs automated verification

- 应用功能多到每次改动后手动全部测一遍太慢了

- The app has enough features that manually testing everything after each change takes too long

- 发现了一个线上 bug，单元测试没有捕获到（因为它只有多个部分协同工作时才会出现）

- A bug was found in production that unit tests did not catch (because it only appeared when multiple parts worked together)

- 重大发布前，验证所有关键路径仍然正常

- Before a major deployment, to verify all critical paths still work

## 何时不使用 / When Not to Use

- 功能还没实现 —— 先实现再说

- The feature is not yet implemented — implement first

- 单元测试还没到位 —— 先用 `test-generation` 技能加单元测试，再加端到端测试

- Unit tests are not yet in place — add unit tests first with `test-generation` skill, then E2E

- 你只是想隔离测试一个函数 —— 改用 `test-generation` 技能

- You just want to test one function in isolation — use `test-generation` skill instead

## 核心概念（大白话） / Key Concepts (plain language)

- **端到端测试 / E2E test:** 打开真实浏览器，点击按钮、填写表单、检查屏幕上显示的内容 —— 就像真实用户操作一样

- **E2E test:** A test that opens a real browser, clicks buttons, fills in forms, and checks what appears on screen — just like a real user would

- **测试场景 / Test scenario:** 一个完整的用户操作流程，从头到尾（例如："用户登录、创建一个客户、然后在列表中看到这个客户"）

- **Test scenario:** One complete user journey from start to finish (e.g., "user logs in, creates a customer, and sees them in the list")

- **断言 / Assertion:** 确认某件事为真的检查（例如："页面上显示'客户已保存'"）

- **Assertion:** A check that confirms something is true (e.g., "the page shows 'Customer saved'")

- **无头模式 / Headless:** 浏览器在后台不可见地运行（更快，用于自动化运行）

- **Headless:** Running the browser invisibly in the background (faster, used in automated runs)

## 执行步骤 / Procedure

### 第一阶段 —— 选择端到端测试框架 / Phase 1 — Choose the E2E framework

阅读 `docs/design/tech-selection.md` 了解技术栈。根据技术栈推荐：

Read `docs/design/tech-selection.md` for the tech stack. Recommend based on stack:

| 技术栈 / Stack | 推荐工具 / Recommended tool | 原因 / Why |
|-------|-----------------|-----|
| 任何 Web 应用 / Any web app | **Playwright** | 现代、快速、支持所有浏览器，适合大多数项目 / Modern, fast, works with all browsers, good for most projects |
| React/Next.js | Playwright 或 Cypress | 两者都很好 / Both work well |
| Vue/Nuxt | Playwright | 支持最好 / Best support |
| 简单的本地应用 / Simple local app | Playwright | 搭建最简单 / Easiest setup |

如果还没有安装端到端测试框架：

If no E2E framework is installed:

> "要像真实用户一样测试，我推荐 Playwright。它能自动打开浏览器，在你的应用里点击操作，如果有什么坏了就报告出来。要不要我搭好？大概需要 5 分钟。"

> "To test like a real user would, I recommend Playwright. It automatically opens a browser, clicks through your app, and reports if anything breaks. Should I set it up? It takes about 5 minutes."

等待批准后再安装。

Wait for approval before installing.

### 第二阶段 —— 确定要测试的用户流程 / Phase 2 — Identify the journeys to test

阅读 `docs/contracts/` 中的行为契约。找出最关键的几个用户流程 —— 通常是：

Read the behavior contracts in `docs/contracts/`. Identify the most critical user journeys — usually:

1. 核心操作（应用的主要功能）

1. The primary action (the main thing the app does)

2. 认证流程（如果有登录/登出的话）

2. The auth flow (login/logout if the app has it)

3. 任何涉及不可恢复数据的流程（删除、支付、发送）

3. Any flow involving data that cannot be easily recovered (delete, payment, send)

对每个流程，写一段大白话描述：

For each journey, write a plain-language description:

> "流程 1：用户用正确的账号密码登录 → 看到仪表盘 → 创建一个新客户 → 客户出现在列表中"

> "Journey 1: User logs in with valid credentials → sees the dashboard → creates a new customer → customer appears in the list"

列出所有流程后问："我应该先测哪个？我建议从 [最关键的] 开始。"

Present the list and ask: "Which of these should I test first? I recommend starting with [most critical one]."

### 第三阶段 —— 写测试 / Phase 3 — Write the tests

对每个用户流程，写一个测试，包括：

For each journey, write a test that:

1. 准备好需要的初始状态（例如：存在一个测试用户账号）

1. Sets up any required starting state (e.g., a test user account exists)

2. 在浏览器中打开应用

2. Opens the app in a browser

3. 执行流程的每一步（点击、输入、导航）

3. Performs each step of the journey (click, type, navigate)

4. 每个重要操作后，断言预期的结果是否可见

4. After each significant action, asserts the expected result is visible

5. 测试结束后清理测试数据（不让测试残留数据）

5. Cleans up test data after the test runs (so tests do not leave residue)

**测试结构 / Test structure:**

```
测试 / Test: [大白话流程名称 / Plain language journey name]

准备工作 / Setup: [测试前需要存在什么 / what needs to exist before the test]

步骤 / Steps:
1. 打开 / Open [URL]
2. [操作 / Action] → 断言 / Assert: [应该看到什么 / what should be visible]
3. [操作 / Action] → 断言 / Assert: [应该看到什么 / what should be visible]
...

清理 / Teardown: [测试后清理什么 / what to clean up after]
```

**编写规则 / Writing rules:**

- 使用稳定的选择器（通过文本内容、ARIA 角色或 data-testid —— 不要用会变化的 CSS 类名）

- Use stable selectors (by text content, ARIA role, or data-testid — not CSS classes that change)

- 测试名称要有意义：用 `"user can create a customer and see them in the list"` 而不是 `"test_1"`

- Add meaningful test names: `"user can create a customer and see them in the list"` not `"test_1"`

- 每个测试必须独立（不依赖其他测试先运行）

- Each test must be independent (not depend on another test having run first)

- 测试必须能以任意顺序运行

- Tests must work in any order

### 第四阶段 —— 运行测试 / Phase 4 — Run the tests

运行测试套件并汇报结果：

Run the test suite and report results:

- 全部通过 → 显示摘要

- All tests pass → show summary

- 有测试失败 → 用大白话解释哪个失败了、为什么：

- A test fails → explain in plain language what failed and why:

  > "流程 [名称] 的测试在第 [N] 步失败了。它期望在页面上看到 '[预期文字]'，但实际看到的是 '[实际文字]'。这意味着 [大白话解释]。"

  > "The test for [journey] failed at step [N]. It expected to see '[expected text]' on the page, but saw '[actual text]' instead. This means [plain language explanation]."

如果测试揭示了一个真正的 bug（不是测试写错了），切换到 `Diagnose and Fix` 流程。

If a test reveals a real bug (not a test mistake), switch to `Diagnose and Fix` protocol.

### 第五阶段 —— 纳入项目工作流 / Phase 5 — Add to the project workflow

如果这些端到端测试被用来支撑“功能完成 / 修复完成 / 可以提交 / 可以发布”的结论，完成后必须继续走 `.claude/skills/verification-before-completion/SKILL.md`。 / If these E2E tests are being used to support claims like “feature complete”, “bug fixed”, “ready to commit”, or “ready to release”, continue with `.claude/skills/verification-before-completion/SKILL.md` after they pass.

如果这是发布前的关键验收流程，还应继续走 `.claude/skills/finishing-a-development-branch/SKILL.md` 或 `.claude/skills/deployment-check/SKILL.md`，明确当前交付状态和下一步动作。 / If this is a key pre-release acceptance flow, continue with `.claude/skills/finishing-a-development-branch/SKILL.md` or `.claude/skills/deployment-check/SKILL.md` to make the delivery state and next action explicit.


记录如何运行端到端测试：

Document how to run E2E tests:

> "运行所有端到端测试：在终端输入 `[命令]`。浏览器会自动打开，你会看到结果。所有测试应该在大约 [时间] 内通过。"

> "To run all E2E tests: type `[command]` in the terminal. The browser will open automatically and you will see the results. All tests should pass in about [time]."

更新 `docs/verification/`，加入：

Update `docs/verification/` to include:

> "自动化端到端测试：运行 `[命令]` —— 所有用户流程应该通过"

> "Automated E2E tests: run `[command]` — all journeys should pass"

建议把端到端测试加到发布前检查清单中。

Suggest adding E2E tests to a pre-deployment checklist.

### 第六阶段 —— 保存 / Phase 6 — Save

> "已为 [流程名称] 添加端到端测试。提交信息：`Add E2E tests for [feature/journey names]`"

> "E2E tests added for [journeys]. Commit: `Add E2E tests for [feature/journey names]`"
