---
name: test-generation
description: '功能已实现并手动验证通过后，为它生成自动化测试，防止未来改动时悄无声息地破坏已有功能。生成的测试与行为契约相匹配，并解释每个测试在检查什么。 / Use when a feature has been implemented and verified manually, and the user wants automated tests so future changes do not silently break it. Generates tests that match the behavior contract and explains what each test checks. Keywords: vibe coding, test, automated test, regression, verify, non-technical, test generation, quality assurance.'
argument-hint: 'Feature to generate tests for, e.g.: the login feature, the invoice save button, all features in docs/contracts/'
user-invocable: true
---

# 测试生成技能 / Test Generation Skill

为已完成的功能基于其行为契约生成自动化测试。测试就像一个永久的安全网 —— 如果将来的改动破坏了什么，测试会自动发现。

Generate automated tests for an implemented feature based on its behavior contract. Tests act as a permanent verification net — if future changes break something, the tests catch it automatically.

## 何时使用 / When to Use

- 功能已全部实现并手动验证通过

- A feature is fully implemented and manually verified

- 用户希望保护现有功能不被未来改动破坏

- The user wants protection against future changes breaking existing features

- 项目的功能多到每次改动后手动全部重测一遍太慢了

- The project has enough features that manually re-checking everything after each change is slow

## 何时不使用 / When Not to Use

- 功能还没实现 —— 先实现再说

- The feature is not yet implemented — implement first

- 还没有行为契约 —— 先把契约建好，再生成测试

- No behavior contract exists — create the contract first, then generate tests

- 用户正在报告当前的一个 bug —— 先把 bug 修好，再加测试防止复现

- The user is reporting a current bug — fix the bug first, then add tests to prevent recurrence

## 执行步骤 / Procedure

### 第一阶段 —— 阅读契约 / Phase 1 — Read the contract

从 `docs/contracts/<feature-name>.md` 中读取行为契约。列出每一条将被测试的行为。

Read the behavior contract from `docs/contracts/<feature-name>.md`. List every behavior item that will be tested.

阅读 `docs/design/tech-selection.md`，了解这个技术栈有哪些可用的测试工具。如果还没有设置测试框架，要记录下来。

Read `docs/design/tech-selection.md` to understand the testing tools available for this tech stack. If no test framework is set up, note this.

### 第二阶段 —— 检查测试基础设施 / Phase 2 — Check test infrastructure

检查是否已经有测试框架安装并配置好了：

Check whether a test framework is already installed and configured:

- `package.json`（或同等文件）中有测试脚本吗？

- Is there a test script in `package.json` (or equivalent)?

- 有已经存在的测试目录吗（`tests/`、`__tests__/`、`spec/`）？

- Is there an existing test directory (`tests/`, `__tests__/`, `spec/`)?

- 有没有已有的测试文件可以参考格式？

- Are there existing test files to follow as a pattern?

如果还没有测试框架：

If no test framework exists:

> "这个项目还没有搭建测试框架。对于 [技术栈]，我推荐 [框架]，因为 [白话解释的原因]。要不要我先搭好？大概需要 [N] 分钟。"

> "This project doesn't have a test framework set up yet. For [tech stack], I recommend [framework] because [plain language reason]. Should I set it up first? It takes about [N] minutes."

等待批准后再安装任何东西。

Wait for approval before installing anything.

### 第三阶段 —— 写测试 / Phase 3 — Write the tests

对契约中的每一条行为，写一个或多个测试：

For each behavior in the contract, write one or more tests:

- **正常情况测试 / Normal case test:** 正常路径能走通吗？

- **Normal case test:** Does the happy path work?

- **验证错误测试 / Validation error test:** 无效输入是否产生了正确的错误提示？

- **Validation error test:** Does invalid input produce the right error message?

- **边界情况测试 / Edge case test:** 边界输入（空值、零、最大长度）的行为正确吗？

- **Edge case test:** Does boundary input (empty, zero, max length) behave correctly?

- **业务规则测试 / Business rule test:** 违反规则（重复、未授权）时是否返回了正确的响应？

- **Business rule test:** Does a rule violation (duplicate, unauthorized) produce the right response?

写测试的规则：

Test-writing rules:

- 每个测试必须对应行为契约中具体的一条（加注释关联起来）

- Each test must map to a specific behavior contract item (add a comment linking them)

- 测试名称必须是可读的大白话：用 `"shows error message when phone number is empty"` 而不是 `"test_validation_01"`

- Test names must be readable plain English: `"shows error message when phone number is empty"` not `"test_validation_01"`

- 测试应该反映用户能看到的东西，而不是内部实现细节

- Tests should reflect what the USER sees, not internal implementation details

- 每个测试只测一个行为（保持小且精确）

- No more than one behavior per test (keep them small and specific)

### 第四阶段 —— 解释测试 / Phase 4 — Explain the tests

写完测试后，给用户看一份大白话总结：

After writing tests, show the user a plain-language summary:

> "我为 [功能] 写了 [N] 个测试：
>
> ✅ [测试名称] —— 检查 [白话解释]
> ✅ [测试名称] —— 检查 [白话解释]
> ...
>
> 以后每次你想确认一切正常时，这些测试会自动运行。
> 运行方式：[具体命令，例如：在终端输入 `npm test`]。"

> "I've written [N] tests for the [feature] feature:
>
> ✅ [Test name] — checks that [plain language explanation]
> ✅ [Test name] — checks that [plain language explanation]
> ...
>
> These tests will run automatically whenever you want to check that everything still works.
> To run them, [specific command, e.g.: 'type `npm test` in the terminal']."

### 第五阶段 —— 运行测试 / Phase 5 — Run the tests

写完后立刻运行测试：

Run the tests immediately after writing them:

- 所有测试都应该通过（功能已经实现了）

- All tests should pass (the feature is already implemented)

- 如果有测试失败，说明要么测试写错了，要么实现有漏洞

- If any test fails, it means either the test is wrong or the implementation has a gap

汇报结果：

Report the results:

> "全部 [N] 个测试通过 ✓" 或者
> "[N] 个通过，[M] 个失败。失败的原因是：[用大白话解释每个失败是什么意思]"

> "All [N] tests passed ✓" or
> "[N] tests passed, [M] failed. The failures are: [plain language explanation of what each failure means]"

如果有失败，先修好测试（如果是测试写错了）或修好实现（如果确实有漏洞），再算完成。

If failures exist, fix either the test (if the test is wrong) or the implementation (if there is a real gap) before considering the work done.

### 第六阶段 —— 更新验证清单 / Phase 6 — Update verification checklist

更新 `docs/verification/<feature-name>.md`，加入：

Update `docs/verification/<feature-name>.md` to include:

> "自动化测试：运行 `[命令]` —— 所有测试应该通过"

> "Automated tests: run `[command]` — all tests should pass"
