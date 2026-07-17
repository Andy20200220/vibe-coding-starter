# Update Handoff Skill

更新交接文档和执行记录。

## 触发条件

- 用户说"更新交接文档"、"写交接文档"
- 每次会话压缩前
- 每次有实质产出后

## 执行步骤

1. 获取当前真实时间：`date "+%Y-%m-%d %H:%M"`
2. 更新 `docs/plans/execution-log.md`：追加一行记录
3. 更新 `docs/handoff.md`：更新模块状态表、关键决策、日期
4. 如有新文件产出，更新 handoff.md 的核心文件清单
5. 保持 handoff.md 在 70 行以内
