# Codex 工具详细规范

## 参数说明

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| PROMPT | string | ✅ | 审核任务描述 |
| cd | Path | ✅ | 工作目录 |
| sandbox | string | | **必须** `read-only` |
| SESSION_ID | string | | 会话 ID |
| return_all_messages | boolean | | 调试时设为 True |
| image | List[Path] | | 附加图片 |
| model | string | | 指定模型 |

## 返回值

```json
// 成功
{
  "success": true,
  "tool": "codex",
  "SESSION_ID": "uuid-string",
  "result": "Codex 审核结论"
}

// 失败
{
  "success": false,
  "tool": "codex",
  "error": "错误信息"
}
```

## Prompt 模板

```
请 review 以下代码改动：

**改动文件**：[文件列表]
**改动目的**：[简要描述]

**请检查**：
1. 代码质量（可读性、可维护性）
2. 潜在 Bug 或边界情况
3. 需求完成度

**请给出明确结论**：
- ✅ 通过：代码质量良好，可以合入
- ⚠️ 建议优化：[具体建议]
- ❌ 需要修改：[具体问题]
```

## 使用规范

1. **严格边界**：必须 `sandbox="read-only"`，Codex 严禁修改代码
2. **必须保存** `SESSION_ID` 以便多轮对话
3. 检查 `success` 字段判断审核是否成功
4. 从 `result` 字段获取审核结论
