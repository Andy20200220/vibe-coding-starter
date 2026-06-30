---
name: test-driven-development
description: '当行为契约和技术设计已确认，需要按 TDD 红灯→绿灯→重构 的顺序实现功能或修复 bug 时使用。 / Use when the behavior contract and technical design are confirmed and implementation or bug fixing must follow TDD red → green → refactor in order. Keywords: TDD, test first, red green refactor, feature implementation, bug fix, regression test.'
argument-hint: '要按 TDD 实现或修复的内容 / What to implement or fix with TDD, e.g.: implement login per docs/design/login.md, fix duplicate submit bug with regression test'
user-invocable: true
---

# 测试驱动开发技能 / Test-Driven Development Skill

在行为契约和技术设计确认后，先写失败测试，再写最小实现，最后在保持通过的前提下做小范围整理。

After the behavior contract and technical design are confirmed, write the failing test first, then the smallest implementation, then do limited cleanup while keeping everything green.

## 核心原则 / Core Principle

```text
先看到测试正确失败，才允许写实现代码。
No production code without first seeing the test fail correctly.
```

如果你没有亲眼看到测试因为“功能还没实现”而失败，就不能证明这个测试真的在测你想测的行为。

If you have not seen the test fail because the behavior is missing, you have not proven that the test actually verifies the intended behavior.

## 何时使用 / When to Use

- 新功能实现，且 `docs/contracts/` 中的行为契约已确认 / New feature implementation after a confirmed contract in `docs/contracts/`
- 技术设计已明确，准备开始编码 / Technical design is clear and coding is about to start
- 修 bug 时，需要先写回归测试证明问题存在 / Bug fixes that need a regression test first
- 重构前需要先锁定当前行为 / Refactors that need behavior locked down first

## 何时不使用 / When Not to Use

- 行为契约还没确认 / The behavior contract is not yet confirmed
- 技术设计还没确认 / The technical design is not yet confirmed
- 只是纯文档改动、纯配置调整，且没有可测试行为 / Pure docs or pure config changes with no meaningful behavior to test

## 使用前置条件 / Preconditions

开始前先读取：

Read these before starting:

- `docs/contracts/...` 中对应的行为契约 / The relevant behavior contract in `docs/contracts/...`
- `docs/design/...` 中对应的技术设计 / The corresponding technical design in `docs/design/...`
- 如已有验证清单，也读取 `docs/verification/...` / If a verification checklist already exists, also read `docs/verification/...`

## 操作流程 / Procedure

### Phase 1 — RED：先写失败测试 / Write the failing test first

1. 只选一个明确行为。 / Pick one explicit behavior only.
2. 先写一个最小测试来证明这个行为还没实现。 / Write one minimal test that proves the behavior is still missing.
3. 立刻运行测试。 / Run the test immediately.
4. 必须确认它是“按预期失败”，而不是因为拼写错误、环境错误、语法错误失败。 / Confirm it fails for the expected reason, not because of typos, setup problems, or syntax errors.

### Phase 2 — GREEN：写最小实现 / Write the minimum implementation

1. 只写让当前测试通过所需的最少代码。 / Write only the smallest amount of code needed to make the current test pass.
2. 不顺手加新功能。 / Do not add extra features while you are there.
3. 不顺手重构其他地方。 / Do not refactor unrelated code while you are there.
4. 每一步只解决一个问题。 / Each step solves one problem only.

### Phase 3 — VERIFY GREEN：确认真的通过 / Confirm it actually passes

1. 再运行刚才的测试。 / Run the test again.
2. 必须看到它通过。 / You must see it pass.
3. 同时确认没有把相关测试跑坏。 / Also confirm you did not break related tests.

### Phase 4 — REFACTOR：小范围整理 / Limited cleanup

1. 只有在绿色通过后才允许整理代码。 / Cleanup is allowed only after green.
2. 可以做的小整理：去重、改名、抽小函数。 / Allowed cleanup: remove duplication, improve names, extract small helpers.
3. 不允许借重构偷偷加行为。 / Do not sneak in new behavior under the name of refactor.
4. 整理后再次运行测试，保持绿色。 / Re-run tests after cleanup and keep them green.

## 硬规则 / Hard Rules

- **一次只测一个行为 / One behavior at a time**：测试名里如果出现 “和 / and”，通常说明应该拆开。
- **一次只改一个原因 / One change at a time**：不要把多个无关修复绑在同一轮里。
- **最小改动 / Smallest possible change**：实现只够让当前测试通过，不做超前设计。
- **禁止先写实现再补测试 / No implementation first**：如果先写了实现，再补测试，必须重来，不把“已写好的代码”当参考答案。
- **回归 bug 必须先复现 / Bug fixes need a failing reproduction first**：没有失败用例的 bug 修复，不算完成。

## 和本仓库流程的关系 / How This Fits This Repository

这个技能不是替代 `behavior-contract`、`planning-agent` 或 `guided-implementation`，而是接在它们后面，负责真正进入编码阶段时的执行纪律。

This skill does not replace `behavior-contract`, `planning-agent`, or `guided-implementation`. It starts after them and provides the execution discipline for actual coding.

推荐串联方式：

Recommended flow:

1. `behavior-contract` 明确做什么 / define what to build
2. `syseng-agent` 或相关设计流程明确怎么做 / define how to build it
3. `test-driven-development` 先红灯 / write failing tests
4. `dev-agent` 在 TDD 约束下实现 / implement under TDD constraints
5. `verification-before-completion` 完成前验证 / verify before claiming completion

## 输出要求 / Output Requirements

每轮都要明确说清楚：

For each cycle, clearly report:

- 当前在测哪个行为 / Which behavior is under test
- 红灯测试是什么 / What the failing test is
- 为什么它失败 / Why it fails
- 这轮最小实现改了什么 / What the minimal implementation changed
- 绿灯验证结果是什么 / What evidence shows it now passes

## 完成标准 / Done Criteria

只有满足以下全部条件，当前行为才算完成：

A behavior is only done when all of the following are true:

- 已看到红灯测试正确失败 / The failing test was seen to fail correctly
- 已看到绿灯测试通过 / The same test was seen to pass
- 相关回归检查通过 / Relevant regression checks pass
- 改动仍符合行为契约和技术设计 / The change still matches the contract and design
- 可以交给 `verification-before-completion` 继续做完成前验证 / It is ready for `verification-before-completion`
