# GitHub Copilot 配置指南

本目录包含在已有项目中接入 **GitHub Copilot（VS Code）** 所需的配置模板。

---

## 配置文件说明

本目录即新项目的完整结构，**直接复制整个 `github-copilot/` 文件夹到你的项目**即可获得所有配置。

复制后的项目结构：

```
your-project/
├── .github/
│   ├── copilot-instructions.md           ← AI 行为规则（填写后自动生效）
│   ├── prompts/
│   │   ├── clarify-my-requirement.prompt.md  ← 新需求入口
│   │   └── diagnose-and-fix.prompt.md        ← 修 bug 入口
│   ├── agents/
│   │   ├── product-agent.agent.md        ← 需求澄清 + 行为契约专职
│   │   ├── planning-agent.agent.md       ← 拆解任务计划专职
│   │   ├── dev-agent.agent.md            ← 全栈代码实现专职
│   │   ├── test-agent.agent.md           ← 质量检查专职
│   │   └── release-agent.agent.md        ← 提交发布专职
│   └── skills/
│       ├── behavior-contract/SKILL.md    ← 行为契约工作流
│       └── guided-implementation/SKILL.md ← 分步实现工作流
└── docs/
    ├── contracts/                        ← 行为契约存档（功能做完存这里）
    └── verification/                     ← 验证清单存档
```

---

## 已有项目接入步骤

### 第一步：复制文件夹

把整个 `github-copilot/` 目录下的内容（`.github/` 和 `docs/`）复制到你的项目根目录。

### 第二步：填写项目信息

打开 `.github/copilot-instructions.md`，填写所有 `[FILL IN]` 标记的部分：

- **项目是什么**：一句话描述你的项目
- **技术栈**：不知道就让 AI 帮你扫描填写
- **目录结构**：不知道就让 AI 帮你梳理
- **启动命令**：你平时怎么跑项目

> 不知道怎么填？在 Copilot Chat 里说：
> "帮我扫描这个项目，填写 `.github/copilot-instructions.md` 里的项目信息部分"

### 第三步：验证生效

重新打开 VS Code，在 Copilot Chat 里问：
> "你知道这个项目是做什么的吗？有什么规则需要遵守？"

如果 AI 能正确描述你的项目和规则，说明配置已生效。

---

## 生效方式

- **自动生效**：只要文件存在于 `.github/copilot-instructions.md`，VS Code 中的 Copilot 每次对话都会自动读取
- **无需命令**：不需要任何额外操作
- **仅限 VS Code**：此配置仅对 GitHub Copilot（VS Code）生效，不影响其他工具

---

## 注意事项

- 文件**必须**放在 `.github/` 目录下，放在项目根目录无效
- 每个项目只有一个 `copilot-instructions.md`，不支持多文件叠加
- 如果同时使用 Claude Code，还需要配置 `CLAUDE.md`，详见 `../claude-code/` 目录
