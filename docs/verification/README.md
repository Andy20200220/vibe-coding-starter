# 验证清单存档 / Verification Checklists

本目录存放每个已完成功能的验证清单。每份清单是一组具体步骤，不懂代码的人可以照着操作来确认功能是否正确。

This directory stores verification checklists for each completed feature. Each checklist is a set of specific steps a non-technical user can follow to confirm the feature works correctly.

## 工作方式 / How It Works

- 一个功能一个文件（如 `login.md`、`customer-list.md`）/ One file per feature (e.g., `login.md`, `customer-list.md`)
- 功能通过全部实现步骤后自动创建 / Created automatically after a feature passes all implementation steps
- 发现 bug 时，运行对应清单定位哪个行为出了问题 / When a bug is found, run the relevant checklist to locate which behavior is broken
- 修完 bug 后，更新清单加入新的验证步骤 / After a bug fix, the checklist is updated with any new verification steps

## 文件格式 / File Format

每份清单列出 / Each checklist lists：

1. 打开哪个页面 / What page/screen to open
2. 做什么操作（点击、输入数据等）/ What action to take (click, enter data, etc.)
3. 应该看到什么（预期结果）/ What you should see (expected result)
4. 要试的边界情况（错误输入、空数据等）/ Edge cases to try (wrong input, empty data, etc.)