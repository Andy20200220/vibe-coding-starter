# 源代码目录规则

> 本文件放在 src/ 目录下，Codex 在 src/ 内写代码时自动生效。

## 文件组织 / File Organization

- 每个组件一个文件，文件名与组件名一致
- 公共工具函数放 src/utils/，业务逻辑放 src/services/
- 类型定义放 src/types/，全局常量放 src/constants/
- 不创建超过 3 层深的目录嵌套

## 命名约定 / Naming Conventions

- 文件名：组件用 PascalCase（LoginForm.tsx），工具函数用 camelCase（ormatDate.ts）
- 变量和函数：camelCase（userName、getUserById）
- 常量和枚举：UPPER_SNAKE_CASE（MAX_RETRY_COUNT）
- 类型/接口：PascalCase（UserProfile、ApiResponse）

## Import 顺序 / Import Order

每个文件的 import 按以下顺序排列，组间空一行：

1. 第三方库（react, lodash 等）
2. 项目内部模块（@/components, @/utils 等）
3. 相对路径导入（./, ../）
4. 样式文件（./styles.css）

## 代码约束 / Code Constraints

- 每个函数不超过 50 行，超过必须拆分
- 不写超过 3 层嵌套的 if/for
- 不在组件内直接调 API，统一走 src/services/ 层
- 修改现有代码时不重构不相关的部分
- 每次改动不超过 3 个文件（根 AGENTS.md 全局规则）

## 注释 / Comments

- 默认不写注释，代码自解释优先
- 只在以下情况写注释：非显而易见的算法、临时解决方案（标记 TODO）、性能敏感代码
- 注释用中文
