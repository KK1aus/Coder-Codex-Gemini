---
name: ccg-workflow
description: |
  Coder-Reviewer-MCP collaboration for code and document tasks.
  Use when: writing/modifying code, editing documents, implementing features, fixing bugs, refactoring, or code review.
  协调 Coder 执行代码/文档改动，Reviewer 审核代码质量。
---

# Coder-Reviewer-MCP 协作流程

## 角色分工

- **Claude**：架构师 + 验收者 + 最终决策者
- **Coder**：执行者（代码/文档改动，使用 claude-glm/GLM-4.7）
- **Reviewer**：审核者 + 代码质量把关（使用 Gemini）

## 任务拆分原则（分发给 Coder）

> ⚠️ **一次调用，一个目标**。禁止向 Coder 堆砌多个不相关需求。

- **精准 Prompt**：目标明确、上下文充分、验收标准清晰
- **按模块拆分**：相关改动可合并，独立模块分开
- **阶段性 Review**：每模块 Claude 验收，里程碑后 Reviewer 审核

## 核心流程

### 1. 执行：Coder 处理所有改动

所有代码、文档等内容改动任务，**直接委托 Coder 执行**。

调用前（复杂任务推荐）：
- 搜索受影响的文件/符号
- 在 PROMPT 中列出修改清单
- **复杂问题可先与 Reviewer 沟通**：架构设计或复杂方案可先咨询后再委托 Coder 执行

### 2. 验收：Claude 快速检查

Coder 执行完毕后，Claude 快速读取验收：
- **无误** → 继续下一任务
- **有误** → Claude 自行修复

### 3. 审核：Reviewer 阶段性 Review

阶段性开发完成后，调用 Reviewer review：
- 检查代码质量、潜在 Bug
- 结论：✅ 通过 / ⚠️ 优化 / ❌ 修改

## 工具参考

| 工具 | 用途 | sandbox | 重试 |
|------|------|---------|------|
| Coder (claude-glm) | 执行改动 | workspace-write | 默认不重试 |
| Reviewer (gemini) | 代码审核 | read-only | 默认 1 次 |

**会话复用**：保存 `SESSION_ID` 保持上下文。各工具的 SESSION_ID 相互独立。

## 独立决策

Coder/Reviewer 的意见仅供参考。你（Claude）是最终决策者，需批判性思考，做出最优决策。

详细参数：[coder-guide.md](coder-guide.md) | [reviewer-guide](reviewer-guide.md)
