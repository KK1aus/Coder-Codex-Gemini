---
name: ccg-workflow
description: |
  MANDATORY workflow for all code/document tasks. Use when: writing/modifying code, editing documents, implementing features, fixing bugs, refactoring, or completing tasks.
  This skill enforces: 1) Delegate changes to Coder, 2) Verify quality yourself, 3) MUST call Reviewer after milestones (modules, 3+ tasks, before commits, complex bug fixes, refactoring, plan completion).
  🚫 NEVER skip Reviewer without explicit user confirmation.
  协调 Coder 执行代码/文档改动，强制要求在里程碑后调用 Reviewer 审核。
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

## ⚠️ 何时必须调用 Reviewer

### 强制触发（Mandatory）

**必须**在以下时机调用 Reviewer 审核：

- ✅ **Coder 完成一个独立功能模块后**
  - 例如：完成一个 API 接口、一个 UI 组件、一个数据处理流程
- ✅ **完成 3 个及以上连续任务后**
  - 即使每个任务很小，累计 3 个也必须 review
- ✅ **准备提交代码或创建 PR 前**
  - 任何即将推送到远程仓库的代码
- ✅ **修复复杂 bug 后**
  - 非简单 typo 的 bug 修复
- ✅ **重构代码后**
  - 任何涉及架构调整的重构
- ✅ **实现计划文档中的所有任务后**
  - 完整执行完 docs/plans/*.md 或 docs/designs/*.md 中的任务列表

### 可选但有价值（Optional but Valuable）

以下情况强烈建议调用 Reviewer：

- 🔹 遇到困难需要 fresh perspective（新视角）
- 🔹 不确定代码质量或设计合理性
- 🔹 重要功能开发前（baseline check）
- 🔹 大规模代码改动前（确认方向）

### 🚫 禁止跳过

**绝不**因为以下理由跳过 review：

- ❌ "看起来很简单" - 简单代码也可能有隐藏问题
- ❌ "赶时间" - 质量比速度更重要
- ❌ "只是小修改" - 小修改也可能引入 bug
- ❌ "我很确信没问题" - 所有人都会犯错
- ❌ "Reviewer 可能会反对" - Reviewer 的意见是帮助改进

### 📋 调用 Reviewer 的标准流程

1. **准备审核信息**：
   - 列出改动的文件列表
   - 说明改动目的和背景
   - 提供相关的计划文档或需求

2. **调用 mcp__ccg_reviewer 工具**：
   ```
   PROMPT: 请 review 以下代码改动：
   **改动文件**：[文件列表]
   **改动目的**：[简要描述]
   **请检查**：
   1. 代码质量（可读性、可维护性、潜在 bug）
   2. 需求完成度
   3. 给出明确结论：✅ 通过 / ⚠️ 优化 / ❌ 修改
   ```

3. **处理审核结果**：
   - ✅ 通过 → 继续下一任务
   - ⚠️ 建议优化 → 根据反馈委托 Coder 修复，然后重新 review
   - ❌ 需要修改 → 委托 Coder 修复，必须重新 review 直至通过

## 工具参考

| 工具 | 用途 | sandbox | 重试 |
|------|------|---------|------|
| Coder (claude-glm) | 执行改动 | workspace-write | 默认不重试 |
| Reviewer (gemini) | 代码审核 | read-only | 默认 1 次 |

**会话复用**：保存 `SESSION_ID` 保持上下文。各工具的 SESSION_ID 相互独立。

## 独立决策

Coder/Reviewer 的意见仅供参考。你（Claude）是最终决策者，需批判性思考，做出最优决策。

详细参数：[coder-guide.md](coder-guide.md) | [reviewer-guide](reviewer-guide.md)
