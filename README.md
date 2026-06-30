# Vibe Coding 项目模板

[English](#what-is-this) | 中文

面向不懂代码的人的 AI 协作开发项目模板。用这个模板创建新项目，即可获得一套完整的 AI 协作工作流。

## 这是什么

一套预配置的 AI 协作规则和工作流，让不懂代码的人也能通过 AI（GitHub Copilot / Claude Code / Codex CLI）高效开发软件产品。

它解决这些问题：
- AI 自由发挥导致实现和你的预期不一致
- 一次改动太多文件，出问题后定位不到原因
- 修 bug 越修越乱，反复打补丁
- 长对话后 AI 忘记之前定的规则

## 快速开始

### 1. 用这个模板创建你的项目

点击 GitHub 页面上的 **Use this template** → **Create a new repository**，给你的项目起个名字。

或者手动克隆：

```bash
git clone https://github.com/YOUR_USERNAME/vibe-coding-starter.git my-project
cd my-project
rm -rf .git
git init
```

### 2. 选择你的 AI 编码环境

根据你使用的 AI 工具选择对应的配置目录：

### 3. 部署配置

根据你选的环境部署对应的配置文件：

| 环境 | 操作 |
|------|------|
| **GitHub Copilot** | 把 github-copilot/ 下的 .github/ 和 docs/ 复制到项目根 |
| **Claude Code** | 把 claude-code/CLAUDE-template.md 复制到根目录，改名 CLAUDE.md，再把 .claude/ 复制到项目根 |
| **Codex CLI** | 把 codex/AGENTS-template.md 复制到根目录改名 AGENTS.md，其余 AGENTS-*.md 按文件名提示复制到对应子目录 |

### 4. 开始对话

打开你选择的 AI 工具，直接说你的想法，例如：

> "我想做一个小工具，帮我管理客户的跟进记录，能记录每次跟客户聊了什么，提醒我该联系谁了"

AI 会自动读取预配置的规则，按以下流程引导你：

1. **先问你问题**（而不是直接写代码）
2. **输出产品定义** — 你确认
3. **帮你选技术方案** — 用大白话解释，你同意就行
4. **初始化项目** — 创建代码骨架，告诉你怎么启动
5. **细化行为契约** — 每个功能拆成"用户做什么→系统做什么"，你逐条确认
6. **分步写代码** — 每次只改少量文件，每步都告诉你怎么验证

## 项目结构

```
├── github-copilot/                       ← GitHub Copilot（VS Code）配置
│   ├── README.md
│   ├── .github/
│   │   ├── copilot-instructions.md
│   │   ├── prompts/
│   │   │   ├── clarify-my-requirement.prompt.md
│   │   │   └── diagnose-and-fix.prompt.md
│   │   ├── agents/                       ← 5 个专职子 Agent
│   │   │   ├── product-agent.agent.md
│   │   │   ├── planning-agent.agent.md
│   │   │   ├── dev-agent.agent.md
│   │   │   ├── test-agent.agent.md
│   │   │   └── release-agent.agent.md
│   │   └── skills/                       ← 25 个 skills
│   └── docs/
│       ├── contracts/
│       └── verification/
├── codex/                               ← Codex CLI 配置
│   ├── README.md                        ← 接入指南
│   ├── AGENTS-template.md               ← → 根 AGENTS.md（全局约束+Agent调度）
│   ├── AGENTS-src.md                    ← → src/AGENTS.md（代码规范）
│   ├── AGENTS-docs-contracts.md         ← → docs/contracts/AGENTS.md（契约规范）
│   ├── AGENTS-docs-verification.md      ← → docs/verification/AGENTS.md（验证规范）
│   ├── AGENTS-docs-design.md            ← → docs/design/AGENTS.md（设计规范）
│   ├── AGENTS-docs-plans.md             ← → docs/plans/AGENTS.md（计划+日志规范）
│   ├── AGENTS-tests.md                  ← → tests/AGENTS.md（测试规范）
│   └── agents/                          ← 6 个 Agent 角色预设├── claude-code/                          ← Claude Code CLI 配置
│   ├── README.md
│   ├── CLAUDE-template.md                ← 重命名为 CLAUDE.md 使用
│   ├── .claude/
│   │   ├── commands/
│   │   │   ├── clarify-my-requirement.md
│   │   │   └── diagnose-and-fix.md
│   │   ├── agents/                       ← 5 个专职子 Agent
│   │   │   ├── product-agent.md
│   │   │   ├── planning-agent.md
│   │   │   ├── dev-agent.md
│   │   │   ├── test-agent.md
│   │   │   └── release-agent.md
│   │   └── skills/                       ← 25 个 skills（同 GC）
│   └── docs/
│       ├── contracts/
│       └── verification/
├── .github/
│   └── copilot-instructions.md           ← 本模板仓库自身的 AI 规则
├── docs/
│   ├── contracts/
│   └── verification/
├── vibe-coding-guide.md                  ← 完整实战指南（必读）
└── README.md
```

## 日常使用

| 你想做的事 | 怎么跟 AI 说（Copilot / Claude Code / Codex 通用） |
|-----------|-------------|
| 全新产品想法 | 直接描述你想做什么，AI 会引导你完成产品定义 |
| 给项目加功能 | "我想加一个 XX 功能"，AI 会先做行为契约再实现 |
| 发现了 bug | "我点了 XX 之后出现了 YY，但我期望看到 ZZ" |
| 了解项目状态 | "帮我看看项目现在的状态" |

## 核心原则

1. **先契约后代码** — 每个功能都先有行为契约，AI 照契约实现
2. **分步实现分步验证** — 一次最多改 3 个文件，每步都验证
3. **修 bug 有协议** — 先解释原因，最多修 3 次，3 次不行就停
4. **关键规则写成文件** — 不靠对话记忆，写进 instructions
5. **阶段性保存** — 每个功能做完用 Git 提交

## 你不需要做的事

- 不需要看代码
- 不需要懂技术术语
- 不需要自己写任何配置
- 不需要记住之前说过什么

## 你唯一需要做的事

- 用自己的话描述想法
- 逐条确认 AI 给你的行为契约
- 每步按 AI 给的步骤验证（打开页面、点按钮、看结果）
- 不对的时候说"不对"

## 详细指南

阅读 [vibe-coding-guide.md](vibe-coding-guide.md) 了解完整方法论：
- 从零做一个产品的 6 个阶段
- PRD 和行为契约的关系
- 为什么传统对话式开发效果差
- 常见失败模式和解决方法
- AI 协作脚手架全解析
- GitHub Copilot 与 Claude Code 的配置差异、与 Codex 的配置差异

## 前置要求

- [VS Code](https://code.visualstudio.com/)（使用 GitHub Copilot 时）
- [GitHub Copilot](https://github.com/features/copilot) 订阅（含 Copilot Chat），或 [Claude Code CLI](https://docs.anthropic.com/en/docs/claude-code)，或 [Codex CLI](https://github.com/openai/codex)
- Git（用于保存进度）

## 使用哪个环境？

| 环境 | 配置目录 | 适用场景 |
|------|---------|---------|
| **GitHub Copilot**（VS Code） | `github-copilot/` | 日常在 VS Code 内开发 |
| **Codex CLI** | `codex/` | 使用 Codex 命令行工具 |
| **Claude Code CLI** | `claude-code/` | 使用 `claude` 命令行工具 |

三个环境可以同时配置，互不冲突。详见各目录下的 `README.md`。


## 局限性

### 适用场景边界

| 场景 | 适合度 | 说明 |
|------|:---:|------|
| 不懂代码的产品人 | ⭐⭐⭐ | 核心场景：大白话流程，不用看代码 |
| 小团队快速做原型 | ⭐⭐⭐ | 契约驱动 + 分步验证很适合快速迭代 |
| 有工程经验的个人开发者 | ⭐⭐ | 流程偏重，可能觉得约束太多 |
| 专业工程团队 | ⭐ | 缺少 CI/CD、lint、monorepo 等模板，建议搭配 Superpowers 等工程化方案 |

### 具体局限

**测试基础设施无现成模板**
提到了 TDD 和验证流程，但没有提供 jest/vitest/playwright 的现成配置文件。需要 AI 在项目初始化时当场生成——能力有，开箱即用度不如 Superpowers。

**缺少 CI/CD 和工程化配置**
没有 GitHub Actions 模板、Git hooks、lint 规则、monorepo 支持。这些对有工程规范的团队是刚需，目前需要自行补充。

**12 阶段流程对小项目偏重**
完整的「行为契约 → 架构设计 → 测试先行 → 分步实现 → 审查 → 交付」流程，对轻量级 demo 或单文件脚本来说过于正式，可以按需裁剪。

**依赖用户自律**
规则写进了文件，但能否严格执行取决于用户是否遵守流程。模板提供的是约束框架，不是强制执行机制。

**Codex 的代理系统差异**
Codex 的子 Agent 不自动加载人设文件——需要主 Agent 按 AGENTS.md 中的调度规则手动读取并传入。体验上和 CC 的自动加载有细微差异。

**三工具对齐的代价**
为了让 GC、CC、Codex 三个工具共用同一套流程，部分工具专属特性被有意简化。例如 CC 的 `.claude/commands/` 和 GC 的 `.github/prompts/` 在 Codex 侧没有对等机制，直接用自然语言替代。

**非技术用户的天花板**
这套方案能让不懂代码的人控制项目质量，但不等于能处理所有问题。性能调优、安全漏洞、复杂重构仍需技术判断——AI 能帮你写，但不能替你判断。

---

> 如果你需要更强的工程化能力（CI/CD、lint、Git hooks、monorepo），建议搭配 [Superpowers](https://github.com/transloadit/superpowers) 使用。两者互补：你管流程，它管工程。
## License

MIT
