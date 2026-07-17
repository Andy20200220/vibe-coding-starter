# CLAUDE.md

This file provides reusable project instructions for Claude Code.

本文件为 Claude Code 提供一套可复用的项目级说明。

## Project Overview / 项目概览

This repository uses a document-first, contract-driven, agent-assisted workflow. Before writing code, first understand the expected behavior, the relevant documentation, and how the result will be verified.

这个仓库采用“文档优先、契约驱动、Agent 协作”的工作方式。写代码前，先弄清楚预期行为、相关文档，以及最后怎么验证结果。

## Recommended Structure / 推荐目录结构

```text
docs/contracts/     # Behavior contracts and requirement definitions / 行为契约与需求定义
docs/design/        # Technical design documents / 技术设计文档
docs/plans/         # Approved implementation plans / 已确认的实施计划
docs/verification/  # Verification checklists and test notes / 验证清单与测试记录
.claude/agents/     # Specialized sub-agent definitions / 专职子 Agent 定义
.claude/commands/   # Reusable entry commands / 可复用入口命令
.claude/skills/     # Task-specific skills / 面向任务的技能定义
```

If a project uses different folders, follow the actual repository layout instead of forcing this structure.

如果项目使用的目录不同，优先遵循仓库现状，不要强行套用这套结构。

---

## Extended Constraints (Vibe Coding)

- 所有功能实现前必须有已确认的行为契约，存放在 `docs/contracts/`
- **测试先行 / 至少验证优先**：能先写测试的任务，优先先写测试；不适合先写测试的任务，也必须先明确验证方法和通过标准，再开始实现
- **小步改动**：单次改动不超过 3 个文件，超过必须拆分步骤；每一步都要能单独解释目标、验证结果和回退范围
- **单步单目标**：每一步只解决一个明确问题或交付一个明确行为，禁止把多个无关修复、优化、重构打包在同一步里
- 修 bug 时先用大白话解释根因，得到用户确认后才改代码
- 连续修 3 次未解决必须停下来，建议用户回退或开新会话
- **完成前必须验证**：任何任务在声明完成前，必须给出实际验证结果。禁止只根据代码阅读、局部运行或主观判断就宣布完成
- **代码审查不可省略**：实现完成且验证通过后，必须再从代码质量、契约一致性、边界影响三个角度检查一遍，确认没有明显遗漏
- **交付/收尾状态必须明确**：每次完成实质性改动后，要明确说明当前属于“继续开发 / 等待确认 / 可提交 / 可发布”中的哪一种状态
- **交付文档使用中英双语**：`docs/contracts/`、`docs/verification/`、`docs/plans/`、`docs/design/` 下的所有文档，每个标题、说明和条目都同时提供中文和英文
- **记录执行过程**：每个具有实质性的项目工作后（项目相关、非简单沟通、有一定记录意义），在 `docs/plans/execution-log.md` 中追加一条记录，格式：`[日期 时间] [Agent名称] 完成了什么 / 产出了什么文件`。**写入前必须执行 `date "+%Y-%m-%d %H:%M"` 获取当前真实时间，禁止编造时间戳**。时间必须精确到分钟
- **维护问题清单**：发现新问题或完成修复后，在 `docs/issue-tracker.md` 中更新对应条目状态
- **改动回溯设计文档**：任何代码改动必须回溯检查相关的设计文档（`docs/design/`）和行为契约（`docs/contracts/`）。如果改动导致设计与实现不一致，必须同步更新对应文档；如果找不到对应文档，必须在改动前补齐。禁止只改代码不管文档

---

## Available Skills

| 技能 | 文件 | 何时使用 |
|------|------|---------|
| 需求澄清 + 行为契约 | `.claude/skills/behavior-contract/SKILL.md` | 有新功能想法时 |
| 分步实现 | `.claude/skills/guided-implementation/SKILL.md` | 行为契约确认后写代码 |
| 技术选型 | `.claude/skills/tech-selection/SKILL.md` | 项目开始前选技术栈 |
| 项目健康检查 | `.claude/skills/project-health-check/SKILL.md` | 了解项目现状 |
| Git 提交 | `.claude/skills/git-commit/SKILL.md` | 功能完成后提交 |
| 隔离工作区 | `.claude/skills/using-git-worktrees/SKILL.md` | 新功能、复杂修复、高风险改动前，优先切到隔离工作区 |
| 测试驱动开发 | `.claude/skills/test-driven-development/SKILL.md` | 技术设计确认后，按红灯→绿灯→整理推进实现 |
| 完成前验证 | `.claude/skills/verification-before-completion/SKILL.md` | 声称完成、修复、可提交前先拿到新鲜验证证据 |
| 正式代码审查 | `.claude/skills/requesting-code-review/SKILL.md` | 实现和基本验证完成后做正式质量关卡，给出是否可继续推进的结论 |
| 代码审查 | `.claude/skills/code-review/SKILL.md` | 用大白话解释代码质量、风险和改进建议 |
| 生成测试 | `.claude/skills/test-generation/SKILL.md` | 为已实现功能生成自动测试 |
| E2E 测试 | `.claude/skills/e2e-test/SKILL.md` | 模拟用户操作跑通完整流程 |
| 系统化调试 | `.claude/skills/systematic-debugging/SKILL.md` | 遇到 bug、测试失败、异常行为时先找根因 |
| 开发分支收尾 | `.claude/skills/finishing-a-development-branch/SKILL.md` | 验证和审查通过后明确交付状态并决定如何收尾 |
| 安全重构 | `.claude/skills/refactor-safe/SKILL.md` | 清理代码不改功能 |
| 数据迁移 | `.claude/skills/data-migration/SKILL.md` | 修改数据结构保留旧数据 |
| 发布前检查 | `.claude/skills/deployment-check/SKILL.md` | 上线或分享前检查 |
| 文档同步 | `.claude/skills/document-sync/SKILL.md` | 代码改完后同步文档 |
| 性能检查 | `.claude/skills/performance-check/SKILL.md` | 页面/操作慢时找瓶颈修复 |
| API 设计 | `.claude/skills/api-design/SKILL.md` | 前后端交互前设计接口契约 |

---

## Active Documents

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
- `docs/issue-tracker.md` — 问题追踪

---

## Agent 调度规则

| 情境 | 调用的 Agent |
|------|-------------|
| 用户描述了新想法、新功能、新需求 | `product-agent` |
| 行为契约需要建立或确认，功能涉及新模块/接口/依赖/部署变化；用户报bug涉及整体性系统性复杂性 | `syseng-agent` |
| 行为契约已确认，需要产出技术设计文档 | `syseng-agent`（出实现级技术设计，明确每个文件的改动范围、接口合约、验证方式和交付边界） |
| 技术设计文档已确认，需要编写测试用例（TDD 红灯） | `test-agent`（根据设计文档先写测试，覆盖核心路径和边界情况；如果不适合先写测试，也必须先定义验证方式和通过标准） |
| 测试用例就绪，需要实现功能（TDD 绿灯） | `dev-agent`（严格按设计文档小步实现，跑通当前阶段全部测试，并保持改动边界清晰） |
| 实现完成，验证测试通过 + 代码审查 + 完成前确认 | `test-agent`（运行测试确认通过，审查代码质量、契约一致性、边界影响，并确认当前任务是否真的可以结束） |
| 测试通过，提交代码或准备发布 | `release-agent`（做交付收尾，明确当前状态是可提交、可发布，还是继续开发） |
| 用户报告 bug，需要诊断修复 | 直接执行 diagnose-and-fix 流程（先定位根因，再决定修复，禁止未定位就盲改） |
| 用户询问项目现状或健康状态 | 直接执行 project-health-check 技能 |

### Agent 与技能对应关系 / Agent-to-Skill Mapping

- `product-agent`：优先加载 `behavior-contract`、`user-flow-design`、`tech-selection`、`document-sync`
- `syseng-agent`：优先加载 `api-design`、`database-design`、`auth-design`
- `test-agent`：优先加载 `test-driven-development`、`test-generation`、`e2e-test`、`verification-before-completion`、`requesting-code-review`、`code-review`
- `dev-agent`：优先加载 `using-git-worktrees`、`guided-implementation`、`test-driven-development`，再按任务选择前端 / 后端 / UI / 数据类技能
- `release-agent`：优先加载 `verification-before-completion`、`finishing-a-development-branch`、`git-commit`、`deployment-check`、`document-sync`

如果某个 Agent 文件里的技能列表与这里不一致，以 Agent 文件的最新说明为准，并同步更新本节。 / If an agent file and this section ever diverge, treat the agent file as the source of truth and sync this section accordingly.

### 技能放到哪一层 / Where Skills Belong In The Flow

- **需求定义层 / Definition layer**：`behavior-contract`、`user-flow-design`、`tech-selection`
- **设计规划层 / Design & planning layer**：`api-design`、`database-design`、`auth-design`
- **执行实现层 / Execution layer**：`using-git-worktrees`、`test-driven-development`、`guided-implementation`、`form-validation`、`state-management`、`responsive-layout`
- **验证审查层 / Verification layer**：`test-generation`、`e2e-test`、`verification-before-completion`、`requesting-code-review`、`code-review`
- **交付收尾层 / Closeout layer**：`finishing-a-development-branch`、`git-commit`、`deployment-check`、`document-sync`
- **排查修复层 / Debugging layer**：`systematic-debugging`

这样看技能时，先判断它属于哪一层，再判断该交给哪个 Agent。 / When deciding how to use a skill, first identify its layer, then map it to the right agent.


### 调度原则

1. **先确认阶段再调度**：不确定当前在哪个阶段时，先问用户
2. **单次只调用一个 Agent**：不并行调用多个专职 Agent
3. **交接要明确**：每个 Agent 完成工作后，告诉用户下一步建议、验证结果和当前交付状态
3.1 **子 Agent 默认使用独立上下文**：每次任务交接时，默认把子 Agent 视为新开的、不了解当前会话历史的执行者。给任务时必须补足目标、上下文、约束、相关文件和验收标准，禁止假设它会自动继承前文理解
3.2 **子 Agent 必须返回明确状态**：每次交接时，必须使用以下状态之一：`DONE`、`DONE_WITH_CONCERNS`、`NEEDS_CONTEXT`、`BLOCKED`。禁止只回复“差不多好了”或“我改完了”这类模糊结论
3.3 **验证与审查分开给结论**：负责收尾的 Agent 在交接时，至少要分别说明“是否符合契约/设计”和“代码质量是否通过”，不能只给一个笼统的“通过”
4. **需求/合同阶段用户确认，TDD 自动推进**：product-agent 产出契约、syseng-agent 产出架构设计时需要用户确认。技术设计确认后，test-agent → dev-agent → test-agent 自动推进（TDD 红灯→绿灯→验证），无需每步等用户确认
5. **简单任务无需调度**：用户的简单问题直接回答
6. **技能优先**：执行任务前，先查看 `.claude/skills/` 目录下是否有匹配的技能文件
7. **任务提示标明目标 Agent**：给子 Agent 写任务 prompt 时，必须在任务开头写清楚"给 [Agent名称] 的任务"，让子 Agent 知道自己是谁、在什么角色下工作。例如："给 dev-agent 的任务：请实现 analyze.py..."
8. **新功能实现必须走 TDD 四段式流程**：行为契约 → 技术设计文档 → 测试先行（红灯） → 实现（绿灯）。流程详解：
9. **实现优先在隔离工作区进行**：新增功能、复杂修复或高风险改动，优先在独立分支、worktree 或其他隔离工作区中完成实现与验证；验证通过后，再决定是否合并回主线版本。若当前环境不支持隔离工作区，必须至少明确改动边界、保留可回退点，并避免直接在主线做大范围实验。

流程详解：
   - **Phase 1** — `product-agent` 产出行为契约，定义"做什么"。用户确认。
   - **Phase 2** — `syseng-agent` 产出技术设计文档，定义"怎么做"——包括数据流、接口合约、文件改动清单、边界处理策略、验证方式和交付边界。用户确认。
   - **Phase 3** — `test-agent` 根据设计文档编写测试用例（TDD 红灯），覆盖核心路径和边界情况；若不适合先写测试，也必须先明确验证方式与通过标准。自动推进。
   - **Phase 4** — `dev-agent` 严格按设计文档小步实现功能，逐步跑通测试（TDD 绿灯），每一步都要控制改动边界并可单独解释。自动推进。
   - **Phase 5** — `test-agent` 运行完整测试套件确认通过 + 代码审查 + 完成前确认。自动推进。
   禁止跳过任何阶段。禁止跳过设计直接写代码。禁止跳过测试直接实现。禁止未验证就宣布完成。

---

## Communication Style

- 始终使用简单的大白话，不用技术术语
- 必须提到文件名时，用括号解释这个文件的作用
- 不直接显示原始报错信息，先翻译成大白话再展示
- 给出选项时，明确推荐其中一个并用简单语言解释原因
- **思考过程使用中文**：分析问题、拆解步骤、做决策时，内部思考语言统一使用中文
- **回复前展开中文思考过程**：每次回复用户前，先用中文写出思考过程（分析当前状态、判断下一步、为什么这样决策）。不省略、不跳过
- **文档优先**：遇到 bug、报错、功能异常时，甚至是做测试设计、代码设计、开发工作，先读相关契约文档和设计文档理解"应该怎样"，再读代码看"实际怎样"，最后对比找差距。禁止不看文档直接翻代码
