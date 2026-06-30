---
name: verification-before-completion
description: '在声称任务完成、问题修复、测试通过、准备提交或准备发布之前使用。要求先跑完整验证，再根据真实输出给结论。 / Use before claiming work is complete, a bug is fixed, tests pass, or code is ready to commit or release. Requires fresh verification before any success claim. Keywords: verification, completion, evidence, ready, pass, done, release.'
argument-hint: '要完成前验证的内容 / What needs final verification, e.g.: login feature, invoice import fix, release readiness for demo'
user-invocable: true
---

# 完成前验证技能 / Verification Before Completion Skill

任何“完成了”“修好了”“可以提交了”“可以发布了”的说法，都必须先拿到新鲜验证证据。

Any claim like “done”, “fixed”, “ready to commit”, or “ready to release” must be backed by fresh verification evidence first.

## 铁律 / Iron Law

```text
没有新鲜验证证据，就不能宣布完成。
No completion claims without fresh verification evidence.
```

## 何时使用 / When to Use

- 准备说“完成了”之前 / Before saying work is done
- 准备说“修好了”之前 / Before saying a bug is fixed
- 准备提交前 / Before commit
- 准备交给下一个 Agent 前 / Before handoff to the next agent
- 准备发布或演示前 / Before release or demo

## 操作流程 / Procedure

1. **先定义证明命令。 / Identify the proving command.** 哪个命令、哪个检查、哪个手动步骤可以证明当前说法？ / What command, check, or manual flow proves the claim?
2. **立刻运行。 / Run it now.** 必须是当前这次新跑的，不算旧结果。 / It must be a fresh run, not an old result.
3. **完整阅读输出。 / Read the full output.** 不只看最后一句成功，要看失败数、警告、退出码。 / Check failures, warnings, and exit code, not just the final success line.
4. **按结果说话。 / State the actual result.** 成功就带证据说明成功；不成功就老实说明哪里没过。 / If it passes, say so with evidence; if not, state exactly what failed.
5. **给出交付状态。 / State the delivery status.** 只能从以下里选：`继续开发` / `等待确认` / `可提交` / `可发布`。 / Choose one: `continue development`, `waiting for confirmation`, `ready to commit`, `ready to release`.

## 常见场景 / Common Cases

- **测试通过 / Tests pass**：必须有最新测试输出，看到失败数为 0。 / Must have fresh test output showing zero failures.
- **构建成功 / Build succeeds**：必须有最新构建命令成功退出。 / Must have a fresh successful build command.
- **bug 修复 / Bug fixed**：必须重跑原始复现步骤或回归测试。 / Must rerun the original reproduction or regression test.
- **需求满足 / Requirements met**：必须回看行为契约和设计，确认没有漏项。 / Must re-check the behavior contract and design for missing items.

## 和本仓库流程的关系 / How This Fits This Repository

- 在 `test-driven-development` 之后，用于做最终完成前验证
- 在 `git-commit` 之前，用于确认当前状态真能保存
- 在 `release-agent` 或发布前检查之前，用于确认可以交付
- 在 `test-agent` 质量关卡结束时，用于支撑“通过/不通过”的结论

## 硬规则 / Hard Rules

- **不能靠感觉说完成 / No completion by intuition**
- **不能引用旧结果冒充新验证 / No stale verification**
- **不能只看局部成功就宣布整体完成 / No partial-check claims**
- **不能跳过契约和设计回看 / No skipping contract/design re-check for completion claims**

## 输出模板 / Output Template

- 验证对象 / What was verified
- 使用的验证方法 / Verification method used
- 实际结果 / Actual result
- 仍存在的问题 / Remaining gaps, if any
- 当前交付状态 / Current delivery state: `继续开发` / `等待确认` / `可提交` / `可发布`
