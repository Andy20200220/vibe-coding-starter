---
name: tech-selection
description: '当非技术用户已确认产品定义、需要在写代码之前选择技术栈时使用。询问约束条件、用大白话推荐具体方案、解释利弊，并保存决策供后续参考。 / Use when a non-technical user has a confirmed product definition and needs to choose a technology stack before any code is written. Asks about constraints, recommends a concrete stack in plain language, explains tradeoffs, and saves the decision for future reference. Keywords: vibe coding, tech selection, technology, framework, stack, platform, non-technical, recommend, plain language.'
argument-hint: '需要为其选技术的产品定义或想法 / Product definition or idea to select tech for, e.g.: my customer follow-up tracker, the product defined in docs/contracts/product-definition.md'
user-invocable: true
---

# 技术选型技能 / Tech Selection Skill

帮助非技术用户为项目选择合适的技术栈。通过询问约束条件、用大白话推荐具体方案并保存决策。

Help a non-technical user choose the right technology stack for their project by asking about constraints, recommending a concrete option in plain language, and saving the decision.

## 何时使用 / When to Use

- 产品定义已确认（保存在 `docs/contracts/product-definition.md` 或在对话中已讨论） / A product definition is confirmed (in `docs/contracts/product-definition.md` or discussed in chat)
- 还没有选定任何技术 / No technology has been chosen yet
- 用户不知道该用什么框架、语言或平台 / The user does not know what framework, language, or platform to use

## 何时不使用 / When Not to Use

- 产品想法仍模糊 — 先用"澄清我的需求" / The product idea is still vague — use `Clarify My Requirement` prompt first
- 技术已选定 — 检查 `docs/design/tech-selection.md` / Tech has already been selected — check `docs/design/tech-selection.md`
- 用户是在问某个具体技术概念，不是选整体方案 / The user is asking about a specific technical concept, not selecting a stack

## 操作流程 / Procedure

### 阶段 1 — 了解约束条件 / Phase 1 — Understand constraints

向用户询问以下问题。不要假设答案。等收到回复后再给出推荐。 / Ask the user the following questions. Do not assume answers. Wait for replies before making recommendations.

1. **平台 / Platform：** 这个应用在哪里运行？ / Where should this run?
   - 我电脑上的本地应用 / Local app on my computer
   - 网页（浏览器打开，任何人都能访问） / Web page (open in browser, anyone can access)
   - 手机应用（iPhone / Android） / Mobile app (iPhone / Android)
   - 不确定 / Not sure

2. **电脑 / Computer：** 你用什么电脑？ / What computer do you use?
   - Windows
   - Mac
   - 都用 / Both

3. **用户 / Users：** 谁会使用？ / Who will use this?
   - 只有我 / Just me
   - 小团队（2-10人） / A small team (2–10 people)
   - 互联网上所有人 / Anyone on the internet

4. **数据存储 / Data storage：** 数据放在哪？ / Where should data live?
   - 只在我电脑上（本地文件） / On my computer only (local file)
   - 云上（任何地方都能访问） / In the cloud (accessible from anywhere)
   - 不确定 / Not sure

5. **服务器 / Server：** 你有服务器或托管账号吗？ / Do you have a server or hosting account?
   - 没有，我想所有东西都在我电脑上运行 / No, I want everything to run on my computer
   - 有，我有（或可以弄到）托管账号 / Yes, I have (or can get) a hosting account
   - 不确定 / Not sure

6. **未来规模 / Future scale：** 可选的但很有用 — 你预计这个项目会保持小规模还是明显增长？ / Optional but helpful — do you expect this to grow significantly or stay small?

### 阶段 2 — 阅读产品定义 / Phase 2 — Read the product definition

如果存在就阅读 `docs/contracts/product-definition.md`。注意： / Read `docs/contracts/product-definition.md` if it exists. Note:
- 功能总数（完整产品，不只看 MVP） / Total feature count (full product, not just MVP)
- 任何有特定技术含义的功能（如实时更新、文件上传、邮件通知、支付） / Any features that have specific technical implications (e.g., real-time updates, file uploads, email notifications, payments)
- MVP 范围 / MVP scope

基于**完整产品功能列表**做推荐，而不只看 MVP。为 3 个功能选的技术可能不适合 15 个功能。 / Make recommendations based on the **full product feature list**, not just the MVP. Tech selected for 3 features may be wrong for 15.

### 阶段 3 — 推荐技术栈 / Phase 3 — Recommend a stack

根据约束条件和产品定义，推荐**一个具体方案**。不要列出选项菜单而不给出明确推荐——用户无法评估技术利弊。 / Based on constraints and the product definition, recommend **one concrete stack**. Do not present a menu of options without a clear recommendation — the user cannot evaluate technical tradeoffs.

推荐的格式如下： / Format the recommendation like this:

---
**我的推荐 / My recommendation：[方案名称 / Stack Name]**

它是什么（大白话） / What it is (in plain language)：
- [组件1 / Component 1]：[用大白话说明它是干什么的，不用术语] / [what it does in plain language, no jargon]
- [组件2 / Component 2]：[用大白话说明它是干什么的] / [what it does in plain language]
- [组件3 / Component 3]：[用大白话说明它是干什么的] / [what it does in plain language]

为什么适合你的项目 / Why this fits your project：
- [原因1，关联到用户的具体约束或产品需求] / [Reason 1 tied to a specific user constraint or product requirement]
- [原因2] / [Reason 2]
- [原因3] / [Reason 3]

它不擅长什么（坦诚说明） / What it cannot do well (honest tradeoffs)：
- [局限1] / [Limitation 1]
- [局限2] / [Limitation 2]

你需要安装什么 / What you'll need to install：
- [工具1 / Tool 1] — [一句话说明它是什么] / [one sentence on what it is]
- [工具2 / Tool 2] — [一句话说明它是什么] / [one sentence on what it is]

---

问用户："这样清楚吗？你想按这个来，还是有什么约束条件我没注意到？" / Ask the user: "Does this make sense? Do you want to go ahead with this, or is there a constraint I missed?"

如果用户提出改变推荐的约束条件，修改后重新推荐。 / If the user raises a constraint that changes the recommendation, revise and recommend again.

### 阶段 4 — 保存决策 / Phase 4 — Save the decision

用户批准后，将决策保存到 `docs/design/tech-selection.md`： / Once the user approves, save the decision to `docs/design/tech-selection.md`:

```markdown
# 技术选型 / Tech Selection

日期 / Date：YYYY-MM-DD
状态 / Status：已确认 / Confirmed

## 技术栈 / Stack

- [组件/Component]：[名称和版本（如果知道）/ name and version if known]
- [组件/Component]：[名称和版本（如果知道）/ name and version if known]

## 用户约束 / User Constraints

- 平台 / Platform：[答案/answer]
- 电脑 / Computer：[答案/answer]
- 用户 / Users：[答案/answer]
- 数据存储 / Data storage：[答案/answer]
- 服务器 / Server：[答案/answer]

## 为什么选这个方案 / Why This Stack

[用大白话总结选型理由 / Summary of reasoning in plain language]

## 已知局限 / Known Limitations

[坦诚列出此方案不擅长的方面 / Honest list of what this stack does not do well]

## 需安装的工具 / Installation Required

- [工具/Tool]：[安装方式 — 一句话 / how to install — one line]
- [工具/Tool]：[安装方式 — 一句话 / how to install — one line]
```

### 阶段 5 — 说明下一步 / Phase 5 — State next step

保存后： / After saving:
> "技术选型已保存到 `docs/design/tech-selection.md`。下一步是初始化项目——我来创建文件夹结构并确保它能跑起来。可以开始了吗？ / Tech selection is saved to `docs/design/tech-selection.md`. The next step is to initialize the project — I'll create the folder structure and make sure it runs. Ready to start?"
