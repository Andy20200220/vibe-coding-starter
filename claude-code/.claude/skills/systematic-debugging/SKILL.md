---
name: systematic-debugging
description: '当遇到 bug、测试失败、异常行为、构建失败或多层系统问题时使用。在提出修复方案前，必须先完成根因调查。 / Use when you hit a bug, test failure, unexpected behavior, build failure, or multi-layer system issue. Root cause investigation must happen before proposing a fix. Keywords: debug, root cause, test failure, incident, regression, troubleshoot.'
argument-hint: '要排查的问题 / What to debug, e.g.: submit button sometimes saves twice, build passes locally but fails in CI'
user-invocable: true
---

# 系统化调试技能 / Systematic Debugging Skill

不要靠猜来修 bug。先定位根因，再决定修复。

Do not fix bugs by guessing. Find the root cause first, then decide the fix.

## 铁律 / Iron Law

```text
没有完成根因调查，就不能开始修。
No fixes without root cause investigation first.
```

## 何时使用 / When to Use

- 测试失败 / Test failures
- 用户报告 bug / User-reported bugs
- 本地能跑、线上不对 / Works locally but fails elsewhere
- 构建失败 / Build failures
- 集成异常 / Integration issues
- 性能突然异常 / Unexpected performance issues

## 操作流程 / Procedure

### Phase 1 — 先调查，不要先改 / Investigate before changing code

1. **认真读报错。 / Read errors carefully.** 记录文件、行号、调用链、错误码。 / Capture file names, line numbers, stack traces, and codes.
2. **稳定复现。 / Reproduce consistently.** 没法稳定复现时，先补信息，不要猜。 / If you cannot reproduce it, gather more data instead of guessing.
3. **对比预期与实际。 / Compare expected vs actual.** 先说清本来应该怎样，再确认实际上发生了什么。 / State the expected behavior, then confirm what actually happened.
4. **沿数据流往回查。 / Trace the data flow backward.** 不要只盯报错点，要继续往上追输入和调用来源。 / Do not stop at the symptom location; trace the input and call path backward.
5. **多层系统逐层排查。 / Investigate multi-layer systems layer by layer.** 前端 → API → 服务 → 存储 / 进程，每层都可能断。 / Frontend → API → service → storage / process; any layer can be the break.

### Phase 2 — 形成单一假设 / Form one hypothesis

1. 用一句话说清楚： / State in one sentence:
   - “我认为根因是 X，因为我看到了 Y。” / “I think X is the root cause because I observed Y.”
2. 一次只验证一个假设。 / Test one hypothesis at a time.
3. 先做最小实验，不要一口气改很多地方。 / Use the smallest experiment, not a big batch of changes.

### Phase 3 — 最小修复 / Apply the smallest fix

1. 先写能证明问题存在的失败测试，能自动化就自动化。 / Write a failing test or reproduction first when possible.
2. 只修根因，不修症状。 / Fix the root cause, not the symptom.
3. 一次只做一个修复动作。 / Make one fix at a time.
4. 不顺手捆绑优化、重构、顺带清理。 / Do not bundle in optimizations, refactors, or opportunistic cleanup.

### Phase 4 — 验证修复 / Verify the fix

1. 重跑原始复现步骤或回归测试。 / Re-run the original reproduction or regression test.
2. 确认问题真的消失。 / Confirm the issue is actually gone.
3. 确认没有引入新问题。 / Confirm no new issue was introduced.

## 和本仓库规则对齐 / Alignment with Local Rules

- 修 bug 前先用大白话解释根因 / Explain root cause in plain language before fixing
- 连续 3 次修复未解决就停下来重判方向 / Stop and reassess after 3 failed fix attempts
- 任何“修好了”的说法都要交给 `verification-before-completion` 做最终确认 / Any “it is fixed” claim must be backed by `verification-before-completion`

## 硬规则 / Hard Rules

- **禁止未定位就盲改 / No blind fixes before root cause**
- **禁止一次改多个可疑点 / No multi-point guessing**
- **禁止把第 4 次尝试当正常流程 / 3 failed attempts means reassess, not keep thrashing**
- **禁止只汇报结论不汇报证据 / Do not report conclusions without evidence**

## 输出要求 / Output Requirements

调试交接时必须包含：

Debug handoff must include:

- 预期行为 / Expected behavior
- 实际行为 / Actual behavior
- 证据 / Evidence collected
- 根因假设 / Root-cause hypothesis
- 最小修复方案 / Minimal fix plan
- 验证方式 / Verification method
