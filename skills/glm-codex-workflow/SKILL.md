---
name: glm-codex-workflow
description: |
  协调 GLM 和 Codex 进行代码开发与审核。

  触发场景（满足其一）：
  - 代码动作（中/英）：写/生成/新增/实现/改/修/优化/重构/补测试/格式化 | add/create/implement/fix/refactor/optimize/write code
  - 审核对象（中/英）：代码/改动/patch/diff/commit/PR 的 review | review code/diff/PR/commit
  - 技术对象：函数/类/模块/API/接口/脚本/配置（且意图是修改/实现）
  - 上下文兜底：若对话上下文包含 code/diff/patch/commit，即触发

  不触发：
  - 仅讨论概念/架构/方案，不要求写改代码
  - 文档审阅/非代码内容
  - 纯问答或解释性问题（无改动需求）

  模糊请求处理：
  - 若出现"看看/review/帮我改一下"且上下文含代码/改动 → 触发
  - 否则先询问是否需要实际改动/生成代码
---

# GLM-Codex 协作流程

## 角色定位

- **你（Claude）**：架构师 + 协调者 + 最终决策者
- **GLM**：代码执行者（量大管饱，执行力强）
- **Codex**：独立审核者（第三方视角，质量把关）

## 核心流程

### 编码前（可选）
复杂任务可先自行分析拆解，明确方案后再委托 GLM 执行。

### 编码中

| 场景 | 行动 |
|------|------|
| 简单任务 | 优先调用 GLM |
| 批量/重复任务 | 调用 GLM，给出精确 Prompt |
| 需求不明确 | 先拆解，再委托 GLM |

GLM 执行后：快速确认结果，有问题立即修复。

### 编码后（推荐）
调用 Codex review，检查：代码质量、潜在 Bug、需求完成度。

结论：✅ 通过 / ⚠️ 建议优化 / ❌ 需要修改

若需修复：修复后再次 review，迭代直到通过。

### 独立判断
GLM 和 Codex 的意见**仅供参考**，你必须有自己的判断。

## 工具快速参考

### GLM（代码执行）
- 调用时机：批量生成、重复修改、明确功能实现
- 关键参数：`PROMPT`, `cd`, `SESSION_ID`
- **仅当需要参数细节或 Prompt 模板时**，读取 [glm-guide.md](glm-guide.md)

### Codex（代码审核）
- 调用时机：代码改动完成后
- 关键参数：`PROMPT`, `cd`, `sandbox="read-only"`
- **仅当需要参数细节或 Prompt 模板时**，读取 [codex-guide.md](codex-guide.md)

## 示例参考

**仅当需要实际使用案例时**，读取 [examples.md](examples.md)

## 注意事项

- **GLM 可写**：默认 `sandbox=workspace-write`
- **Codex 只读**：必须 `sandbox=read-only`，严禁修改代码
- **会话复用**：保存 `SESSION_ID` 保持上下文连贯
- **按需加载**：不要预先读取子文件，仅在需要时加载
