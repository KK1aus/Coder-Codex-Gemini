# GLM 工具详细规范

## 参数说明

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| PROMPT | string | ✅ | 任务指令 |
| cd | Path | ✅ | 工作目录 |
| sandbox | string | | 默认 `workspace-write` |
| SESSION_ID | string | | 会话 ID，复用保持上下文 |
| return_all_messages | boolean | | 调试时设为 True |

## 返回值

```json
// 成功
{
  "success": true,
  "tool": "glm",
  "SESSION_ID": "uuid-string",
  "result": "GLM 回复内容"
}

// 失败
{
  "success": false,
  "tool": "glm",
  "error": "错误信息"
}
```

## Prompt 模板

```
[SYSTEM] 你是 GLM-4.7，负责执行代码任务。请直接开始工作，不要询问用户。

请执行以下代码任务：

**任务类型**：[新增功能 / 修复 Bug / 重构 / 其他]
**目标文件**：[文件路径]

**具体要求**：
1. [要求1]
2. [要求2]

**约束条件**：
- [约束1]
- [约束2]

**验收标准**：
- [标准1]

请严格按照上述范围修改代码，完成后说明改动内容。
```

## 使用规范

1. **必须保存** `SESSION_ID` 以便多轮对话
2. 检查 `success` 字段判断执行是否成功
3. 从 `result` 字段获取回复内容
4. 调试时设置 `return_all_messages=True`
