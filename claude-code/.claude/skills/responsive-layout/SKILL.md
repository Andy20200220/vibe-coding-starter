---
name: responsive-layout
description: '当页面在手机、平板或不同屏幕尺寸上显示不正常时使用。修复布局使其适应任何屏幕，同时不破坏桌面版已有的显示效果。 / Use when a page looks wrong on mobile, tablet, or different screen sizes. Fixes layout so it adapts to any screen without breaking the desktop version. Keywords: vibe coding, responsive, mobile, tablet, layout, screen size, adapt, breakpoint, non-technical.'
argument-hint: 'What looks wrong, e.g.: the customer list is cut off on mobile, buttons overlap on small screens, the sidebar disappears on tablet'
user-invocable: true
---

# 响应式布局技能 / Responsive Layout Skill

修复页面，使其在所有屏幕尺寸上（手机、平板和桌面）都能正确显示，同时不破坏已有的布局。

Fix pages so they display correctly on all screen sizes — mobile, tablet, and desktop — without breaking existing layouts.

## 适用场景 / When to Use

- 页面在手机或平板上显示不正常

- A page looks wrong on a phone or tablet

- 按钮、文字或图片在小屏幕上被截断或重叠

- Buttons, text, or images are cut off or overlapping on small screens

- 新功能从一开始就需要在桌面和移动端都能用

- A new feature needs to work on both desktop and mobile from the start

- 用户想在分享应用之前检查它是否对移动端友好

- The user wants to check if the app is mobile-friendly before sharing it

## 不适用场景 / When Not to Use

- 问题是功能坏了，而不是布局问题 —— 使用 `诊断并修复（Diagnose and Fix）` 提示

- The issue is a broken feature, not a layout problem — use `Diagnose and Fix` prompt

- 用户想改变视觉设计风格 —— 使用 `ui-component-spec` 技能

- The user wants to change the visual design style — use `ui-component-spec` skill

## 关键概念（大白话） / Key Concepts (plain language)

- **断点（Breakpoint）：** 布局切换的屏幕宽度阈值（例如，低于 768px = 移动端布局）

- **Breakpoint:** A screen width where the layout switches (e.g., below 768px = mobile layout)

- **响应式（Responsive）：** 布局根据屏幕大小自动调整

- **Responsive:** The layout adjusts automatically based on screen size

- **溢出（Overflow）：** 内容比屏幕宽，导致被截断或出现滚动条

- **Overflow:** Content is wider than the screen and gets cut off or causes scrolling

## 流程 / Procedure

### 阶段一 —— 明确问题 / Phase 1 — Identify the problem

询问用户：

Ask the user:

- 哪个页面或功能显示不正常？

- Which page or feature looks wrong?

- 哪个设备/屏幕尺寸上有问题？（手机、平板、小笔记本）

- Which device/screen size is the problem? (phone, tablet, small laptop)

- 具体哪里不对？（被截断、重叠、太小点不动、列乱了）

- What exactly looks wrong? (cut off, overlapping, too small to tap, broken columns)

如果可以，请用户描述他们看到的 vs 预期看到的效果。

If possible, ask the user to describe what they see vs. what they expect to see.

阅读受影响页面的相关源代码。
从 `docs/design/tech-selection.md` 中检查使用了什么 CSS 框架（Tailwind、Bootstrap、纯 CSS 等）。

Read the relevant source files for the affected page.
Check what CSS framework is in use (Tailwind, Bootstrap, plain CSS, etc.) from `docs/design/tech-selection.md`.

### 阶段二 —— 审计布局 / Phase 2 — Audit the layout

在受影响的页面上检查以下内容：

Check the following on the affected page:

**A. 溢出问题**

**A. Overflow issues**

- 有没有固定宽度超过了手机屏幕宽度？（例如，容器上设置了 `width: 800px`）

- Are there fixed widths that exceed mobile screen width? (e.g., `width: 800px` on a container)

- 有没有元素在小屏幕上不会换行或缩小？

- Are there elements that do not wrap or shrink on small screens?

- 是否出现了不应该有的横向滚动条？

- Is horizontal scrolling appearing when it should not?

**B. 触摸目标**

**B. Touch targets**

- 按钮和链接在移动端至少达到 44x44px 吗？（太小 = 戳不动）

- Are buttons and links at least 44x44px on mobile? (too small = hard to tap)

- 可交互元素之间的距离是否太近？

- Are interactive elements too close together?

**C. 文字可读性**

**C. Text readability**

- 移动端字体是否太小？（正文最 16px）

- Is font size too small on mobile? (minimum 16px for body text)

- 在宽屏幕上每行是否太长？（理想：每行 60-80 个字符）

- Is line length too long on wide screens? (ideal: 60-80 characters per line)

**D. 图片和媒体**

**D. Images and media**

- 图片在小屏幕上是否等比例缩放，而不是溢出？

- Do images scale down on small screens or overflow?

- 是否有固定尺寸的图片应该改为流体尺寸？

- Are there fixed-size images that should be fluid?

**E. 导航**

**E. Navigation**

- 导航在小屏幕上还能正常使用吗？

- Does the navigation still work on small screens?

- 移动端是否有汉堡菜单或类似方案？

- Is there a hamburger menu or equivalent for mobile?

**F. 列布局**

**F. Column layouts**

- 多列布局在移动端是否堆叠为单列？

- Do multi-column layouts stack to single column on mobile?

- 表格列在小屏幕上是否可读，还是需要重新组织？

- Are table columns readable on small screens, or do they need to restructure?

### 阶段三 —— 逐步修复 / Phase 3 — Fix step by step

对每个发现的问题：
1. 用大白话告知修改内容："我会让客户列表在手机上堆叠为单列，而不是两列并排"
2. 每次修改不超过 3 个文件
3. 解释改了什么："在比手机还窄的屏幕上，两列现在会上下堆叠显示"
4. 提供验证方法："检查方法：打开页面，把浏览器窗口缩到很窄（或在手机上打开）。两列现在应该竖向堆叠"
5. 等待确认

For each issue found:
1. Announce the change in plain language: "I'm going to make the customer list stack into a single column on mobile instead of two side-by-side columns"
2. Change no more than 3 files per step
3. Explain what changed: "On screens smaller than a phone width, the two columns will now stack on top of each other"
4. Provide verification: "To check: open the page, then resize your browser window to be very narrow (or open on your phone). The columns should now stack vertically"
5. Wait for confirmation

### 阶段四 —— 跨尺寸验证 / Phase 4 — Cross-size verification

所有修复完成后，检查三种尺寸：
- **手机（Mobile）：** 约 375px 宽（iPhone 尺寸）
- **平板（Tablet）：** 约 768px 宽
- **桌面（Desktop）：** 约 1280px 宽

After all fixes, check three sizes:
- **Mobile:** ~375px wide (iPhone size)
- **Tablet:** ~768px wide
- **Desktop:** ~1280px wide

> "请检查这三种尺寸，确认每种尺寸下一切显示正常。"

> "Please check these three sizes and confirm everything looks correct at each one."

### 阶段五 —— 保存 / Phase 5 — Save

> "响应式布局修复完成。提交信息：`Fix responsive layout for [page/feature] on mobile and tablet`"

> "Responsive layout fixes complete. Commit: `Fix responsive layout for [page/feature] on mobile and tablet`"
