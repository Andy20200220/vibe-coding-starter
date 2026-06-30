# 测试规范

> 本文件放在 	ests/ 目录下，Codex 在 tests/ 内写测试时自动生效。

## 测试框架 / Test Framework

- [FILL IN：示例：vitest / jest / pytest]
- 运行命令：[FILL IN：示例：npm test]

## 测试文件命名 / Test File Naming

- 测试文件与源文件同名，加 .test 或 .spec 后缀
- 示例：login.ts → login.test.ts
- 测试文件放在 	ests/ 目录下，镜像源码结构

## 测试结构 / Test Structure

每个测试文件按以下结构组织：

`
describe('模块名 / 功能名')
  describe('正常路径 / Happy Path')
    it('应该... / should...')
    it('应该... / should...')
  describe('边界情况 / Edge Cases')
    it('空值时应该... / should... when empty')
    it('最大值时应该... / should... at max')
  describe('错误处理 / Error Handling')
    it('网络异常时应该... / should... on network error')
`

## 测试编写规则 / Test Writing Rules

- **每个行为契约条目至少对应一个测试用例**
- 测试用例命名：用大白话描述「什么情况下应该发生什么」
- 优先写可独立运行的单元测试，再写集成测试
- 不写测试之间的依赖（每个 it 独立可运行）
- Mock 外部依赖（API、数据库），不 Mock 项目内部模块

## 覆盖率要求 / Coverage Requirements

- 核心业务逻辑：不低于 80%
- UI 组件：覆盖关键交互路径
- 工具函数：100%

## TDD 流程 / TDD Workflow

新功能严格按以下顺序：

1. **红灯 / Red** — 先写一个失败的测试
2. **绿灯 / Green** — 写最少代码让测试通过
3. **验证 / Verify** — 运行全部测试确保没有回归
4. **提交 / Commit** — 绿灯后提交
