---
name: accessibility-check
description: '用于检查和修复无障碍问题：键盘导航、屏幕阅读器支持、颜色对比度和 ARIA 标签。让残障人士也能使用应用，同时提升所有人的整体可用性。 / Use to check and fix accessibility issues: keyboard navigation, screen reader support, color contrast, and ARIA labels. Makes the app usable by people with disabilities and improves overall usability for everyone. Keywords: vibe coding, accessibility, a11y, keyboard, screen reader, contrast, ARIA, usability, non-technical.'
argument-hint: 'What to check, e.g.: the whole app, the login form, the navigation menu, before launch'
user-invocable: true
---

# 无障碍检查技能 / Accessibility Check Skill

检查应用的无障碍问题并修复。让应用可以通过键盘、屏幕阅读器使用，并且对视力障碍用户友好。同时也能提升所有人的可用性。

Check the app for accessibility issues and fix them. Makes the app usable with keyboards, screen readers, and for users with visual impairments. Also improves usability for everyone.

## 适用场景 / When to Use

- 在部署或公开分享应用之前

- Before deploying or sharing the app publicly

- 构建完新功能后，作为最终质量检查

- After building a new feature, as a final quality check

- 用户反馈使用应用有困难（键盘、缩放、对比度）

- When a user reports difficulty using the app (keyboard, zoom, contrast)

- 用户明确希望应用具备无障碍能力

- When the user specifically wants the app to be accessible

## 不适用场景 / When Not to Use

- 功能还没构建完成 —— 先构建，再检查无障碍

- The feature is not built yet — build it first, then check accessibility

- 有功能性 bug 导致无法使用 —— 先修复 bug

- A functional bug is preventing use — fix the bug first

## 关键概念（大白话） / Key Concepts (plain language)

- **键盘导航（Keyboard navigation）：** 能不能只用 Tab、回车和方向键就能操作整个应用？（不用鼠标）

- **Keyboard navigation:** Can you use the whole app using only the Tab, Enter, and arrow keys? (no mouse)

- **屏幕阅读器（Screen reader）：** 为盲人用户把页面内容朗读出来的软件 —— 需要所有元素都有正确的标签

- **Screen reader:** Software that reads the page aloud for blind users — requires proper labels on all elements

- **颜色对比度（Color contrast）：** 文字必须与背景有足够的深浅差，让低视力用户也能看清

- **Color contrast:** Text must be dark enough against its background to be readable by people with low vision

- **焦点指示器（Focus indicator）：** 使用键盘时，当前选中的元素周围可见的轮廓

- **Focus indicator:** The visible outline showing which element is currently selected when using keyboard

- **ARIA 标签（ARIA label）：** 按钮或图标上隐藏的文本标签，屏幕阅读器会将其朗读出来

- **ARIA label:** A hidden text label on a button or icon that screen readers read aloud

## 流程 / Procedure

### 阶段一 —— 确定检查范围 / Phase 1 — Scope the check

如果用户没有明确说明，询问应该检查：(a) 整个应用，(b) 特定页面或功能，还是 (c) 只检查最关键的路径（登录、主要操作）？

Ask if not specified: should I check (a) the whole app, (b) a specific page or feature, or (c) just the most critical paths (login, main action)?

阅读相关的源代码文件和行为契约。

Read the relevant source files and behavior contracts.

### 阶段二 —— 执行审计 / Phase 2 — Run the audit

逐一检查每个类别：

Check each category:

**A. 键盘导航**

**A. Keyboard navigation**

- 每个可交互元素（按钮、链接、输入框、下拉菜单）都能通过 Tab 键到达吗？

- Can every interactive element (buttons, links, inputs, dropdowns) be reached by pressing Tab?

- 所有操作都能不用鼠标完成吗？（点击 = 回车键，下拉菜单 = 方向键）

- Can every action be performed without a mouse? (click = Enter, dropdowns = arrow keys)

- Tab 顺序是否合理？（从上到下，从左到右）

- Is the Tab order logical? (top to bottom, left to right)

- 用户会不会被"困"在某个组件里（用键盘无法退出）？

- Can the user get "trapped" in any component (no way to exit with keyboard)?

**B. 焦点可见性**

**B. Focus visibility**

- 是否始终有可见的轮廓或高亮显示当前聚焦的元素？

- Is there always a visible outline or highlight showing which element is focused?

- 默认的焦点轮廓是否被移除但没有提供替代方案？（CSS `outline: none` 但没有其他替代）

- Was the default focus outline removed without a replacement? (CSS `outline: none` without alternative)

**C. 屏幕阅读器支持**

**C. Screen reader support**

- 所有图片都有 alt 文本吗？（装饰性图片使用 `alt=""`）

- Do all images have alt text? (or `alt=""` if decorative)

- 所有纯图标按钮都有无障碍标签吗？（例如，垃圾桶图标按钮需要 `aria-label="删除"`）

- Do all icon-only buttons have an accessible label? (e.g., a trash icon button needs `aria-label="Delete"`)

- 表单输入框有对应的标签吗？（`<label for="...">` 或 `aria-label`）

- Do form inputs have associated labels? (`<label for="...">` or `aria-label`)

- 错误消息是否与对应的输入框关联？（`aria-describedby`）

- Are error messages linked to their input fields? (`aria-describedby`)

- 页面区域是否用地标标记了？（`<nav>`, `<main>`, `<header>`, `<footer>`）

- Are page sections marked with landmarks? (`<nav>`, `<main>`, `<header>`, `<footer>`)

**D. 颜色对比度**

**D. Color contrast**

- 正文文本与背景的对比度是否达到 4.5:1？

- Does body text meet 4.5:1 contrast ratio against its background?

- 大标题的对比度是否达到 3:1？

- Do large headings meet 3:1 contrast ratio?

- 输入框中的占位文本是否有足够的对比度？

- Does placeholder text in inputs have sufficient contrast?

- 链接是否不仅仅靠颜色来区分？

- Are links distinguishable by more than just color?

**E. 可交互元素尺寸**

**E. Interactive element size**

- 所有可点击/可触摸的目标是否至少达到 44x44px？

- Are all clickable/tappable targets at least 44x44px?

**F. 动画和动效**

**F. Motion and animation**

- 是否有动画持续播放而没有暂停方法？

- Does any animation play continuously without a way to pause it?

- 应用是否尊重 `prefers-reduced-motion` 设置？

- Does the app respect `prefers-reduced-motion` setting?

### 阶段三 —— 报告发现 / Phase 3 — Report findings

对每个问题评级：🔴 阻止访问 / 🟡 显著障碍 / 🟢 小改进

Rate each issue: 🔴 Blocks access / 🟡 Significant barrier / 🟢 Minor improvement

---
## 无障碍报告 / Accessibility Report

### 🔴 阻止访问 / Blocks Access
| Issue | Location | How to fix |
|-------|----------|-----------|

### 🟡 显著障碍 / Significant Barriers
| Issue | Location | How to fix |
|-------|----------|-----------|

### 🟢 小改进 / Minor Improvements
| Issue | Location | How to fix |
|-------|----------|-----------|

---

### 阶段四 —— 优先修复阻塞性问题 / Phase 4 — Fix blockers first

按标准流程逐一处理 🔴 阻塞项：告知、修改（最多 3 个文件）、解释、验证、等待确认。

Address 🔴 blockers one at a time using the standard flow: announce, change (max 3 files), explain, verify, wait for confirmation.

对于键盘导航验证：

> "请把鼠标放到一边。重复按 Tab 键，检查能否到达 [元素]。按回车键激活它。你应该 [预期结果]。"

For keyboard navigation verification:
> "Put your mouse aside. Press Tab repeatedly and check that you can reach [element]. Press Enter to activate it. You should [expected result]."

### 阶段五 —— 保存 / Phase 5 — Save

> "无障碍修复完成。提交信息：`Fix accessibility issues in [scope]`"

> "Accessibility fixes complete. Commit: `Fix accessibility issues in [scope]`"
