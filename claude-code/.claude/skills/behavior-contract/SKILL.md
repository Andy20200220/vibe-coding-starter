---
name: behavior-contract
description: '当非技术用户确认了产品定义或功能描述后，需要将其转化为精确的、逐条的行为契约来约束 AI 实现时使用。在需求澄清之后、写任何代码之前调用。 / Use when a non-technical user has confirmed a product definition or feature description and needs it converted into a precise, item-by-item behavior contract that constrains AI implementation. Use after requirement clarification, before any code is written. Keywords: vibe coding, behavior contract, feature spec, acceptance criteria, non-technical, user action, system response, validation.'
argument-hint: '要转化为契约的功能或产品定义 / Feature or product definition to convert, e.g.: the login feature from product-definition.md, the invoice management feature'
user-invocable: true
---

# 行为契约技能 / Behavior Contract Skill

将已确认的产品定义或功能描述转化为精确的行为契约，供非技术用户逐条审查。

Convert a confirmed product definition or feature description into a precise behavior contract that a non-technical user can review item by item.

## 何时使用 / When to Use

- 功能想法已经讨论过，用户已确认大致方向 / A feature idea has been discussed and the user has confirmed the general direction
- 下一步是在写代码之前，为每个用户操作明确定义系统应该做什么 / The next step is to define exactly what the system should do for each user action before writing code
- 用户不懂代码，需要大白话规格说明 / The user does not know code and needs plain-language specifications

## 何时不使用 / When Not to Use

- 想法仍然模糊 — 先用"澄清我的需求" / The idea is still vague — use `Clarify My Requirement` prompt first
- 行为契约已存在且代码已就绪 — 用 `guided-implementation` 技能 / A behavior contract already exists and code is ready — use `guided-implementation` skill
- Bug 需要修复 — 用"诊断并修复" / A bug needs fixing — use `Diagnose and Fix` prompt

## 操作流程 / Procedure

1. **读取现有上下文。 / Read existing context.** 检查 `docs/contracts/` 下是否有已有的产品定义或行为契约。检查 `docs/design/` 下是否有技术设计文档。不要重复已有内容。 / Check `docs/contracts/` for any existing product definition or behavior contracts. Check `docs/design/` for any technical design. Do not duplicate what already exists.

2. **确定功能范围。 / Identify the feature scope.** 与用户确认要定义的功能是哪个。一份行为契约只覆盖一个功能。不要把多个功能合并在一起。 / Confirm with the user which feature is being specified. One behavior contract covers exactly one feature. Do not combine multiple features.

3. **列出所有用户操作。 / List all user actions.** 针对目标功能，列举用户可做的每一个操作： / For the target feature, enumerate every action a user can take:
   - 按钮点击 / Button clicks
   - 表单提交 / Form submissions
   - 导航操作 / Navigation actions
   - 数据输入 / Data entry
   - 选择、切换、筛选 / Selections, toggles, filters

4. **为每个操作定义系统响应。 / Define the system response for each action.** 对每一个用户操作，说明： / For every user action, state:
   - **正常情况 / Normal case：** 操作成功时用户应该看到什么 / What the user should see when the action succeeds
   - **校验错误 / Validation errors：** 输入无效时发生什么（空值、格式错误、超长等） / What happens when the input is invalid (empty, wrong format, too long, etc.)
   - **业务规则冲突 / Business rule violations：** 操作与规则冲突时发生什么（重复、未授权、超限等） / What happens when the action conflicts with a rule (duplicate, unauthorized, limit exceeded, etc.)
   - **边界情况 / Edge cases：** 边界值下发生什么（零值、最大值、首次操作、已存在等） / What happens with boundary values (zero, maximum, first time, already exists, etc.)

5. **定义验证步骤。 / Define verification steps.** 为每一条行为，写出用户可操作的验证步骤： / For each behavior item, write a concrete verification step the user can perform:
   - "打开 [页面]" / "Open [page]"
   - "输入 [具体值]" / "Enter [specific value]"
   - "点击 [按钮]" / "Click [button]"
   - "你应该看到 [预期结果]" / "You should see [expected result]"

6. **与用户一起审查。 / Review with the user.** 逐条展示行为契约。对每条： / Present the behavior contract item by item. For each item:
   - 问用户："这条对吗？" / Ask the user: "Is this correct?"
   - 如果用户不确定，把该条标记为"待定 — 需要决策"然后继续 / If the user is unsure, mark the item as "TBD — needs decision" and move on
   - 如果用户不同意，立即修改 / If the user disagrees, revise immediately

7. **保存契约。 / Save the contract.** 将确认后的行为契约按以下结构保存到 `docs/contracts/<feature-name>.md`： / Save the confirmed behavior contract to `docs/contracts/<feature-name>.md` using this structure:

```markdown
# 行为契约 / Behavior Contract：[功能名 / Feature Name]

状态 / Status：已确认 / Confirmed | 部分确认（N 条待定） / Partially confirmed (N items TBD)
日期 / Date：YYYY-MM-DD

## 用户操作与系统响应 / User Actions and System Responses

### [操作组名称 / Action Group Name]

| # | 用户操作 / User Action | 正常响应 / Normal Response | 错误/边界情况 / Error / Edge Cases | 验证 / Verification |
|---|------------|----------------|-------------------|--------------|
| 1 | ... | ... | ... | ... |

## 待定事项 / TBD Items

- 第 N 条 / Item N：[需要决定的内容描述 / description of what needs to be decided]

## 备注 / Notes

- [与用户讨论过的假设或约束 / Any assumptions or constraints discussed with the user]
```

8. **说明下一步。 / State next step.** 保存后： / After saving:
   - 如果所有条目已确认 → 下一步是先产出技术设计，再进入 `test-driven-development` + `guided-implementation` / If all items are confirmed → next step is technical design first, then `test-driven-development` + `guided-implementation`
   - 如果还有待定项 → 实现前先解决它们 / If TBD items remain → resolve them before implementation
   - 如果该功能依赖另一个未实现的功能 → 标注依赖关系 / If the feature depends on another un-built feature → note the dependency

## 输出指导 / Output Guidance

- 所有语言必须是非技术的大白话。不要出现代码、技术术语或框架名词。 / All language must be non-technical. No code, no technical jargon, no framework references.
- 每个功能一份契约。不要把多个功能合并。 / One contract per feature. Do not merge multiple features.
- 每条行为都必须有对应的验证步骤。 / Every behavior item must have a corresponding verification step.
- 不要编造用户未确认的行为。未知项标记为待定。 / Do not invent behaviors the user did not confirm. Mark unknowns as TBD.
