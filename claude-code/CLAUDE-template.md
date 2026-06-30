# 项目静态事实（Claude Code 专用）

> 使用说明：把这个文件复制到你项目根目录，重命名为 `CLAUDE.md`，填写所有 `[FILL IN]` 部分。
> 不知道怎么填的地方，在 CC 会话里说："帮我扫描项目，填写 CLAUDE.md 里的项目信息"

---

## 项目说明

**项目是什么：**
[FILL IN：一句话描述。示例："一个帮我管理客户跟进记录、提醒我何时联系的本地工具"]

**技术栈：**
[FILL IN：示例："Next.js 14 + SQLite + Tailwind CSS，在 Windows 本地运行"]

**目录结构：**
[FILL IN：示例：
```
src/            # 应用源码
src/pages/      # 页面组件
src/api/        # 后端接口
docs/           # 项目文档
docs/contracts/ # 行为契约（正确行为的权威来源）
docs/verification/ # 验证清单
```
]

---

## 常用命令

```bash
# 安装依赖
[FILL IN：示例：npm install]

# 启动项目
[FILL IN：示例：npm start]

# 运行测试
[FILL IN：示例：npm test]

# 构建
[FILL IN：示例：npm run build]
```

---

## 约束

- 所有功能实现前必须有已确认的行为契约，存放在 `docs/contracts/`
- 单次改动不超过 3 个文件，超过必须拆分步骤
- 修 bug 时先用大白话解释根因，得到用户确认后才改代码
- 连续修 3 次未解决必须停下来，建议用户回退或开新会话
- **交付文档使用中英双语**：`docs/contracts/`、`docs/verification/`、`docs/plans/`、`docs/design/` 下的所有文档，每个标题、说明和条目都同时提供中文和英文。格式：中文在前，英文紧跟其后（括号或新行均可）。
- **记录执行过程**：每个 Agent 完成工作后，在 `docs/plans/execution-log.md` 中追加一条记录，格式：`[日期 时间] [Agent名称] 完成了什么 / 产出了什么文件`。时间必须精确到分钟（如 2026-04-29 18:35）。**每完成一个步骤就立刻追加，不得等到最后拼记。**
- **维护问题清单**：发现新问题或完成修复后，在 `docs/issue-tracker.md` 中更新对应条目状态（✅已修复 / 🔄修复待验证 / ❌未修复 / 🔍待分析）。用户验证通过后，将状态更新为 ✅。若文件不存在，立即新建。

---

## 可用技能（Skills）

以下技能文件位于 `.claude/skills/`，需要时读取对应 SKILL.md 执行：

| 技能 | 文件 | 何时使用 |
|------|------|---------|
| 需求澄清 + 行为契约 | `.claude/skills/behavior-contract/SKILL.md` | 有新功能想法时 |
| 分步实现 | `.claude/skills/guided-implementation/SKILL.md` | 行为契约确认后写代码 |
| 技术选型 | `.claude/skills/tech-selection/SKILL.md` | 项目开始前选技术栈 |
| 项目健康检查 | `.claude/skills/project-health-check/SKILL.md` | 了解项目现状 |
| 保存进度 | `.claude/skills/git-commit/SKILL.md` | 功能完成后提交 |
| 代码审查 | `.claude/skills/code-review/SKILL.md` | 用大白话解释代码质量、风险和改进建议 |
| 生成测试 | `.claude/skills/test-generation/SKILL.md` | 为已实现功能生成自动测试 |
| E2E 测试 | `.claude/skills/e2e-test/SKILL.md` | 模拟用户操作跑通完整流程 |
| 安全重构 | `.claude/skills/refactor-safe/SKILL.md` | 清理代码不改功能 |
| 数据迁移 | `.claude/skills/data-migration/SKILL.md` | 修改数据结构保留旧数据 |
| 发布前检查 | `.claude/skills/deployment-check/SKILL.md` | 上线或分享前检查 |
| 文档同步 | `.claude/skills/document-sync/SKILL.md` | 代码改完后同步文档 |
| 性能检查 | `.claude/skills/performance-check/SKILL.md` | 页面/操作慢时找瓶颈修复 |
| API 设计 | `.claude/skills/api-design/SKILL.md` | 前后端交互前设计接口契约 |
| **前端** | | |
| 响应式布局 | `.claude/skills/responsive-layout/SKILL.md` | 修复手机/平板显示错乱 |
| 表单验证 | `.claude/skills/form-validation/SKILL.md` | 统一表单校验和错误提示 |
| 状态管理 | `.claude/skills/state-management/SKILL.md` | 多页面共享数据不同步 |
| 无障碍检查 | `.claude/skills/accessibility-check/SKILL.md` | 键盘导航/屏幕阅读器支持 |
| **后端** | | |
| 认证设计 | `.claude/skills/auth-design/SKILL.md` | 登录/注册/权限系统设计 |
| 错误日志 | `.claude/skills/error-logging/SKILL.md` | 生产环境出错有迹可查 |
| 数据库设计 | `.claude/skills/database-design/SKILL.md` | 存储结构设计防止后期返工 |
| 频率限制 | `.claude/skills/rate-limiting/SKILL.md` | 防暴力破解和接口滥用 |
| **UI 设计** | | |
| 组件规范 | `.claude/skills/ui-component-spec/SKILL.md` | 统一按钮/颜色/字体风格 |
| 用户流程设计 | `.claude/skills/user-flow-design/SKILL.md` | 多步骤操作流程设计 |
| 反馈状态设计 | `.claude/skills/feedback-design/SKILL.md` | loading/成功/错误/空状态 |

---

## 活跃文档

- `docs/contracts/product-definition.md` — 产品定义（如果存在）
- `docs/contracts/*.md` — 各功能行为契约
- `docs/verification/*.md` — 各功能验证清单

---

## Agent 调度规则

本项目配置了 6 个专职子 Agent，位于 `.claude/agents/`。主 Agent 负责判断何时调用哪个子 Agent：

| 情境 | 调用的 Agent |
|------|-------------|
| 用户描述了新想法、新功能、新需求 | `product-agent` |
| 行为契约确认，功能涉及新模块/接口/依赖/部署变化 | `syseng-agent` |
| 行为契约已确认，需要拆解成任务计划 | `planning-agent` |
| 任务计划已就绪，开始写代码实现 | `dev-agent` |
| 实现完成，进入质量检查阶段 | `test-agent` |
| 测试通过，提交代码或准备发布 | `release-agent` |
| 用户报告 bug，需要诊断修复 | 直接执行 diagnose-and-fix 流程，不需要调用子 Agent |
| 用户询问项目现状或健康状态 | 直接执行 project-health-check 技能 |

### 调度原则

1. **先确认阶段再调度**：不确定当前在哪个阶段时，先问用户，再决定调用哪个 Agent。
2. **单次只调用一个 Agent**：不并行调用多个专职 Agent。
3. **交接要明确**：每个 Agent 完成工作后，告诉用户"已完成 X，下一步建议调用 Y Agent"。
4. **用户始终是决策者**：任何 Agent 交接都需要用户知晓并确认，不得自动串联。
5. **简单任务无需调度**：用户的简单问题直接回答，不需要调用子 Agent。
6. **技能优先**：执行任务前，先查看 `.claude/skills/`（或 `.github/skills/`）目录下是否有匹配的技能文件。有则读取 SKILL.md 并按其指导执行；无则直接处理。在回复第一行标注所用技能，例如：`【开发模式 · 使用 refactor-safe 技能】`；未用技能则仅标注模式。

---

## 沟通风格

- 始终使用简单的大白话，不用技术术语
- 必须提到文件名时，用括号解释这个文件的作用
- 不直接显示原始报错信息，先翻译成大白话再展示
- 给出选项时，明确推荐其中一个并用简单语言解释原因
- **思考过程使用中文**：分析问题、拆解步骤、做决策时，内部思考语言统一使用中文
