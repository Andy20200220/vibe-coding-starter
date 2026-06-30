# 验证清单编写规范

> 本文件放在 docs/verification/ 目录下，Codex 在 verification/ 内写验证清单时自动生效。

## 语言要求 / Language Requirement

- **所有内容必须中英双语**
- 格式：中文在前，英文紧跟其后

## 文件命名 / File Naming

- 一份验证清单对应一个功能，文件名：[功能名]-verification.md
- 如 login-verification.md、user-profile-verification.md

## 清单结构 / Checklist Structure

`markdown
# 验证清单：[功能名] / Verification Checklist: [Feature Name]

对应契约 / Contract：docs/contracts/[功能名].md
验证日期 / Verification Date：YYYY-MM-DD
验证人 / Verified By：[姓名]

## 功能验证 / Functional Verification

| # | 验证项 | 操作步骤 | 预期结果 | 实际结果 | 状态 |
|---|-------|---------|---------|---------|------|
| 1 | 描述验证什么 | 具体操作 | 应该看到什么 | 实际看到什么 | ✅/❌ |

## 边界验证 / Edge Case Verification

| # | 边界情况 | 操作步骤 | 预期结果 | 实际结果 | 状态 |
|---|---------|---------|---------|---------|------|

## 回归验证 / Regression Check

| # | 已有功能 | 验证结果 | 状态 |
|---|---------|---------|------|
| 1 | 不受影响的功能 | 正常/异常 | ✅/❌ |

## 验证结论 / Conclusion

- 通过项 / Passed：N
- 失败项 / Failed：N
- 结论：可交付 / 需返工
`

## 状态标记 / Status Markers

- ✅ 通过 / Passed
- ❌ 失败 / Failed
- ⏭ 跳过 / Skipped（附原因）
- 🔧 已修复待重验 / Fixed, pending re-verification
