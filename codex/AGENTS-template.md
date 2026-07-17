# 项目静态事实（Codex 专用 · 根 AGENTS.md）

> 使用说明：把这个文件复制到你项目根目录，重命名为 AGENTS.md，填写所有 [FILL IN] 部分。
> 不知道怎么填的地方，在 Codex 会话里说："帮我扫描项目，填写 AGENTS.md 里的项目信息"

---

## 项目说明

**项目是什么：**
[FILL IN：一句话描述。示例："一个帮我管理客户跟进记录、提醒我何时联系的本地上具"]

**技术栈：**
[FILL IN：示例："Next.js 14 + SQLite + Tailwind CSS，在 Windows 本地运行"]

**目录结构：**
[FILL IN：示例：
`
src/            # 应用源码
src/pages/      # 页面组件
src/api/        # 后端接口
docs/           # 项目文档
docs/contracts/ # 行为契约（正确行为的权威来源）
docs/verification/ # 验证清单
tests/          # 测试代码
`
]

---

## 常用命令

`ash
# 安装依赖
[FILL IN：示例：npm install]

# 启动项目
[FILL IN：示例：npm start]

# 运行测试
[FILL IN：示例：npm test]

# 构建
[FILL IN：示例：npm run build]
`

---

## 约束

### 全局约束（所有目录生效）

- 所有功能实现前必须有已确认的行为契约，存放在 docs/contracts/
- 单次改动不超过 3 个文件，超过必须拆分步骤
- 修 bug 时先用大白话解释根因，得到用户确认后才改代码
- 连续修 3 次未解决必须停下来，建议用户回退或开新会话
- **交付文档使用中英双语**：docs/contracts/、docs/verification/、docs/plans/、docs/design/ 下的所有文档，每个标题、说明和条目都同时提供中文和英文。格式：中文在前，英文紧跟其后。
- **记录执行过程**：每个 Agent 完成工作后，在 docs/plans/execution-log.md 中追加一条记录，格式：[日期 时间] [Agent名称] 完成了什么 / 产出了什么文件。时间必须精确到分钟。**每完成一个步骤就立刻追加，不得等到最后拼凑。**
- **维护问题清单**：发现新问题或完成修复后，在 docs/issue-tracker.md 中更新对应条目状态（✅已修复 / 🔧修复待验证 / ❌未修复 / 🔬待分析）。若文件不存在，立即新建。

---

## 沟通风格

- 始终使用简单的大白话，不用技术术语
- 必须提到文件名时，用括号解释这个文件的作用
- 不直接显示原始报错信息，先翻译成大白话再展示
- 给出选项时，明确推荐其中一个并用简单语言解释原因
- **思考过程使用中文**：分析问题、拆解步骤、做决策时，内部思考语言统一使用中文

---

## 活跃文档 / Active Documents

新会话启动时，先读启动指南建立认知，再讨论任务：

1. `docs/ai-startup-guide.md` — **新会话必读**（三步建立项目认知）
2. `docs/handoff.md` — 交接文档（最新快照：模块状态、关键决策、常用命令）
3. `docs/plans/execution-log.md` — 执行记录（完整历史流水账）

模块文档按需读取：
- `docs/reference/project-brief.md` — 项目摘要
- `docs/reference/ai-persona.md` — AI人设
- `docs/contracts/*.md` — 各功能行为契约
- `docs/design/*.md` — 各模块技术设计
- `docs/verification/*.md` — 各功能验证清单

---

## 多层 AGENTS.md 规则体系

本项目使用多层 AGENTS.md，每层管不同的事：

`
your-project/
├── AGENTS.md                       # 本文件——全局约束 + Agent 调度
├── src/
│   └── AGENTS.md                   # 代码规范（命名、import、文件组织）
├── docs/
│   ├── contracts/
│   │   └── AGENTS.md               # 行为契约编写规范
│   └── verification/
│       └── AGENTS.md               # 验证清单编写规范
└── tests/
    └── AGENTS.md                   # 测试规范（框架、覆盖率）
`

**优先级**：深层目录的 AGENTS.md 覆盖浅层。写代码时自动加载 src/AGENTS.md，写契约时自动加载 docs/contracts/AGENTS.md。

---

## 子 Agent 调度规则（自动加载人设）

本项目预设了 6 个专职子 Agent，角色文件存放在 codex/agents/ 目录中。

### 调度流程（强制执行）

**当你需要调用子 Agent 时，必须按以下步骤操作：**

1. **确定阶段** — 判断当前处于哪个工作阶段（产品定义？架构设计？编码？测试？发布？）
2. **读取人设** — 读取 codex/agents/<agent-name>.md，获取该 Agent 的角色描述、职责和硬规则
3. **传入人设** — 调用 spawn_agent 时，将角色描述作为 message 的开头
4. **传入任务** — 在角色描述后接上具体任务描述
5. **传入上下文** — 告诉子 Agent 需要参考哪些契约和文档

### 示例：调用产品 Agent

`
1. 读取 codex/agents/product-agent.md
2. spawn_agent(message="[将 product-agent.md 内容贴在这里]
   ---
   具体任务：用户想要一个登录功能，请将其转化为行为契约。")
`

### 各 Agent 调度时机

| 当前阶段 | 调度 Agent | 角色文件 | 何时调用 |
|---------|-----------|---------|---------|
| 产品定义 | product-agent | codex/agents/product-agent.md | 用户有新想法、新功能、新需求时 |
| 架构设计 | syseng-agent | codex/agents/syseng-agent.md | 行为契约确认后，功能涉及新模块/接口/依赖变化 |
| 任务规划 | planning-agent | codex/agents/planning-agent.md | 架构设计确认后需拆解任务 |
| 代码实现 | dev-agent | codex/agents/dev-agent.md | 任务计划就绪，开始写代码 |
| 质量检查 | test-agent | codex/agents/test-agent.md | 实现完成后进入验证 |
| 提交发布 | release-agent | codex/agents/release-agent.md | 测试通过后提交交付 |

### 调度原则

1. **一次只调一个** — 不并行调用多个子 Agent
2. **阶段确认** — 不确定当前阶段时先问用户
3. **交付明确** — 每个 Agent 完成后告诉用户"已完成X，建议调用 Y Agent"
4. **用户始终是决策者** — 任何 Agent 交接都需用户知晓并确认
5. **简单任务无需调度** — 用户问简单问题直接回答，不调子 Agent
6. **新功能走 TDD 流程**：行为契约 → 架构设计 → 测试先行 → 实现 → 验证。禁止跳过任何阶段

---

## 与 Claude Code / GitHub Copilot 共用

| 工具 | 配置文件 | 存放位置 |
|------|---------|---------|
| **Codex** | AGENTS.md（多层嵌套） | 各目录 |
| **Claude Code** | CLAUDE.md | 项目根目录 |
| **GitHub Copilot** | .github/copilot-instructions.md | .github/ 目录 |

三者配置共存，互不冲突。
