# Codex（OpenAI Codex CLI）配置指南

本目录包含在已有项目中接入 **Codex CLI** 所需的全部配置模板。

---

## 快速部署（推荐）

把 `codex/` 目录下的文件按以下映射复制到你项目目录：

| 模板文件 | 复制到 | 作用 |
|---------|-------|------|
| `AGENTS-template.md` | `项目根目录/AGENTS.md` | 全局约束 + Agent 调度 |
| `AGENTS-src.md` | `src/AGENTS.md` | 代码规范 |
| `AGENTS-docs-contracts.md` | `docs/contracts/AGENTS.md` | 契约编写规范 |
| `AGENTS-docs-verification.md` | `docs/verification/AGENTS.md` | 验证清单规范 |
| `AGENTS-docs-design.md` | `docs/design/AGENTS.md` | 架构设计文档规范 |
| `AGENTS-docs-plans.md` | `docs/plans/AGENTS.md` | 任务计划+执行日志规范 |
| `AGENTS-tests.md` | `tests/AGENTS.md` | 测试规范 |
| `agents/`（整个目录） | `codex/agents/` | Agent 角色预设 |

部署后的项目结构：

```
your-project/
├── AGENTS.md                       ← 根 AGENTS.md
├── codex/
│   └── agents/
│       ├── product-agent.md
│       ├── syseng-agent.md
│       ├── planning-agent.md
│       ├── dev-agent.md
│       ├── test-agent.md
│       └── release-agent.md
├── src/
│   └── AGENTS.md                   ← 代码规范
├── docs/
│   ├── contracts/
│   │   └── AGENTS.md               ← 契约规范
│   ├── verification/
│   │   └── AGENTS.md               ← 验证规范
│   ├── design/
│   │   └── AGENTS.md               ← 架构设计规范
│   └── plans/
│       └── AGENTS.md               ← 计划+日志规范
└── tests/
    └── AGENTS.md                   ← 测试规范
```

### 一键复制命令（Windows PowerShell）

```powershell
Copy-Item "codex/AGENTS-template.md" "AGENTS.md"
Copy-Item "codex/AGENTS-src.md" "src/AGENTS.md"
Copy-Item "codex/AGENTS-docs-contracts.md" "docs/contracts/AGENTS.md"
Copy-Item "codex/AGENTS-docs-verification.md" "docs/verification/AGENTS.md"
Copy-Item "codex/AGENTS-docs-design.md" "docs/design/AGENTS.md"
Copy-Item "codex/AGENTS-docs-plans.md" "docs/plans/AGENTS.md"
Copy-Item "codex/AGENTS-tests.md" "tests/AGENTS.md"
Copy-Item "codex/agents" "codex/agents" -Recurse
```

---

## 与 Claude Code / GitHub Copilot 的核心差异

| | Claude Code | GitHub Copilot | **Codex** |
|---|---|---|---|
| **主配置文件** | `CLAUDE.md`（根目录） | `.github/copilot-instructions.md` | `AGENTS.md`（**多层嵌套**） |
| **配置层级** | 单文件 | 单文件 | 多层，子目录覆盖父目录 |
| **子 Agent** | `.claude/agents/` 文件定义 | `.github/agents/` 文件定义 | `codex/agents/` 预设 + spawn_agent 调用 |
| **技能/Skills** | `.claude/skills/` 项目级 | `.github/skills/` 项目级 | `$CODEX_HOME/skills/` 全局安装 |
| **命令/Prompts** | `.claude/commands/` | `.github/prompts/` | 无对应机制 |

---

## 子 Agent 调度机制

Codex 不自动加载 agent 文件，但通过 **根 AGENTS.md 里的调度规则** 实现同等效果：

1. 主 Agent 读取根 `AGENTS.md` → 看到"调用子 Agent 前先读 `codex/agents/xxx.md`"
2. 主 Agent 读取对应 agent 文件 → 获得角色描述和人设
3. 主 Agent 调用 `spawn_agent` → 将人设+任务传入

对使用者来说，体验和 CC 的自动加载一致——不需要手动写人设。

---

## 生效方式

- **自动生效**：`AGENTS.md` 存在于目录中即可，Codex 每次会话自动读取
- **多层生效**：Codex 读取当前工作目录及所有父目录的 AGENTS.md，深层规则覆盖浅层
- **无需命令**：不需要任何额外操作

---

## Codex 技能系统

Codex 的技能是**全局安装**的，不在项目目录内。常用技能：

| 技能 | 用途 |
|------|------|
| `skill-installer` | 安装和管理技能 |
| `skill-creator` | 创建自定义技能 |
| `plugin-creator` | 创建 Codex 插件 |
| `openai-docs` | 查询 OpenAI 官方文档 |
| `browser` | 控制内置浏览器 |
| `playwright` | 浏览器自动化测试 |
| `pdf` | PDF 创建和处理 |
| `spreadsheets` | Excel/CSV 操作 |
| `presentations` | PPT 创建 |
| `documents` | Word 文档创建 |

---

## 与 CC/GC 共用

三个工具的配置互相独立，可以共存：

| 工具 | 配置文件 | 位置 |
|------|---------|------|
| **Codex** | `AGENTS.md`（多层） | 各目录 |
| **Claude Code** | `CLAUDE.md` | 项目根目录 |
| **GitHub Copilot** | `.github/copilot-instructions.md` | `.github/` |