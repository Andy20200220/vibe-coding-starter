---
name: performance-check
description: '当应用变慢、页面加载太久或操作卡住时使用。定位瓶颈，用大白话解释原因，逐步修复。 / Use when the app feels slow, a page takes too long to load, or an action hangs. Identifies the bottleneck, explains it in plain language, and fixes it step by step. Keywords: vibe coding, performance, slow, lag, loading, speed, optimize, non-technical, bottleneck, fast.'
argument-hint: '哪里慢，例如：客户列表页加载太久了、保存发票很慢、整个应用都感觉卡 / What is slow, e.g.: the customer list page takes forever to load, saving an invoice is slow, the whole app feels sluggish'
user-invocable: true
---

# 性能检查技能 / Performance Check Skill

找出应用变慢的原因，用大白话解释原因，以小步验证的方式修复，不破坏已有功能。

Find out why the app is slow, explain the cause in plain language, and fix it in small verified steps without breaking existing features.

## 何时使用 / When to Use

- 某个页面、操作或功能明显很慢

- A page, action, or feature is noticeably slow

- 应用本来很快，但随着数据增多变慢了

- The app was fast but has gotten slower as more data was added

- 某个特定操作（搜索、保存、加载列表）卡住或超时

- A specific operation (search, save, load list) hangs or times out

- 部署前，作为预防性检查

- Before deploying, as a preventive check

## 何时不使用 / When Not to Use

- 应用坏了（不是慢，而是行为错误）——应使用「诊断并修复」流程

- The app is broken (not slow, just wrong) — use `Diagnose and Fix` prompt instead

- 用户想要添加功能——性能工作在功能正常工作之后进行

- The user wants to add features — performance work comes after features work correctly

## 核心原则 / Core Principle

绝不要瞎猜。先测量，再修复。没有针对测量到的瓶颈的"修复"只会浪费时间，还可能搞坏东西。

Never guess. Measure first, then fix. A "fix" that does not address the measured bottleneck wastes time and may break things.

## 操作步骤 / Procedure

### 第一阶段——了解症状 / Phase 1 — Understand the symptom

询问用户：

Ask the user:

- 具体什么慢？（页面加载、点击按钮、列表显示、搜索、保存）

- What specific thing is slow? (page load, button click, list display, search, save)

- 有多慢？（几秒、十秒以上、永远完成不了）

- How slow is it? (a few seconds, 10+ seconds, never finishes)

- 以前是不是更快？如果是，发生了什么变化？

- Did it used to be faster? If so, what changed?

- 数据量多大？（比如：10 个客户 vs 10000 个客户）

- How much data is there? (e.g., 10 customers vs 10,000 customers)

### 第二阶段——测量，不要猜 / Phase 2 — Measure, don't guess

根据症状判断慢的类型：

Identify the type of slowness based on the symptom:

| 症状 / Symptom | 可能的原因 / Likely cause | 排查方向 / Where to look |
|---------|-------------|---------------|
| 数据多时列表加载慢 | 缺少数据库索引或加载了太多记录 | 数据库查询、分页 |
| List loads slowly with lots of data | Missing database index or loading too many records | Database queries, pagination |
| 首次打开时页面加载慢 | 一次性加载了太多数据或太多文件 | 初始数据获取、打包体积 |
| Page loads slowly on first open | Loading too much data or too many files upfront | Initial data fetch, bundle size |
| 保存操作慢 | 同步操作阻塞了响应 | 写操作、外部调用 |
| Saving is slow | Synchronous operations blocking the response | Write operations, external calls |
| 搜索慢 | 搜索字段上没有索引 | 数据库查询计划 |
| Search is slow | No index on searched fields | Database query plan |
| 什么都慢 | 没有缓存，重复执行相同查询 | 查询模式、缓存层 |
| Everything is slow | No caching, repeated identical queries | Query patterns, caching layer |

添加计时测量，找出时间真正花在哪里：

Add timing measurements to identify where time is actually spent:

- 围绕最慢的操作记录时间戳

- Log timestamps around the slowest operation

- 检查数据库查询执行时间（SQLite 用 EXPLAIN QUERY PLAN，PostgreSQL 用 EXPLAIN ANALYZE）

- Check database query execution time (use EXPLAIN QUERY PLAN for SQLite, EXPLAIN ANALYZE for PostgreSQL)

- 如果是前端，在浏览器开发者工具中检查网络请求耗时

- Check network request times in browser dev tools if frontend

在提出任何修复方案之前先报告测量结果：

Report the measurements before proposing any fix:

> "我测量了慢的部分。结果如下：
> - [某某操作]总共花了[X]毫秒
> - 其中[具体步骤]占了[Y]毫秒
> - 瓶颈在于：[用大白话解释]"

> "I measured the slow part. Here's what I found:
> - [Operation] takes [X]ms total
> - [Specific step] accounts for [Y]ms of that
> - The bottleneck is: [plain language explanation]"

### 第三阶段——用大白话解释原因 / Phase 3 — Explain the cause in plain language

尽可能用生活中的比喻来解释：

Use a real-world analogy when possible:

- "数据库在扫描每一条记录来找匹配项，就像找一个人的名字时，不是用拼音索引查电话本，而是从头到尾一页一页翻。"

- "The database is scanning every single record to find matches, like searching for a name by reading every page of a phone book instead of using the alphabetical index."

- "应用一次性加载了 5000 条客户记录，但每次只显示 20 条，就像你只需要其中一页，却把整套百科全书都打印了出来。"

- "The app is loading 5,000 customer records when it only shows 20 at a time, like printing the entire encyclopedia when you only need one page."

然后问："这和你体验到的情况一致吗？"等待用户确认。

Ask: "Does this match what you're experiencing?" Wait for confirmation.

### 第四阶段——用最小有效改动来修复 / Phase 4 — Fix with the minimal effective change

按类别列出常见修复方案（选择能解决测量出的瓶颈的最简单方案）：

Common fixes by category (apply the simplest fix that addresses the measured bottleneck):

**数据库查询太慢：**

**Database query too slow:**

- 在查询字段上添加索引

- Add an index on the queried fields

- 加分页（每次加载 N 条，而不是一次性全加载）

- Add pagination (load N records at a time instead of all at once)

- 只选需要的列，不要 `SELECT *`

- Select only needed columns instead of `SELECT *`

**一次性加载太多数据：**

**Too much data loaded at once:**

- 实现分页或无限滚动

- Implement pagination or infinite scroll

- 只在需要时才加载详细信息（懒加载）

- Load details only when needed (lazy loading)

- 缓存不常变化的结果

- Cache results that do not change often

**重复的相同查询：**

**Repeated identical queries:**

- 在内存中短时间缓存结果

- Cache the result in memory for a short time

- 将多个查询合并为一个

- Batch multiple queries into one

**前端渲染慢：**

**Frontend rendering slow:**

- 避免在仅有一个条目变化时重新渲染整个列表

- Avoid re-rendering the whole list when only one item changes

- 虚拟化长列表（只渲染可见条目）

- Virtualize long lists (only render visible items)

每次修复：

For each fix:

1. 用大白话说明将要改什么、为什么改

   Announce what will change and why, in plain language

2. 每次改动不超过 3 个文件

   Change no more than 3 files per step

3. 修复后再次测量，确认有改善

   Measure again after the fix to confirm improvement

4. 报告修复前后对比数据：

   Report the before/after numbers:

   > "修复前：[X]毫秒。修复后：[Y]毫秒。快了[N]倍。"

   > "Before: [X]ms. After: [Y]ms. That's [N]x faster."

### 第五阶段——确认没有倒退 / Phase 5 — Verify no regression

所有修复完成后：

After all fixes:

- 如果有现成的测试，运行它们

- Run existing tests if available

- 手动确认受影响的功能仍然正常

- Manually verify the affected feature still works correctly

- 对照受影响功能的行为契约逐条检查

- Run through the behavior contract items for the affected feature

### 第六阶段——保存进度 / Phase 6 — Save progress

> "性能修复完成。准备好保存了吗？提交信息：`优化[功能]加载时间，从 X 毫秒降到 Y 毫秒`"

> "Performance fix complete. Ready to save? Commit: `Improve [feature] load time from Xms to Yms`"
