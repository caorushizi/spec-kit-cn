---
description: 使用计划模板执行实现规划工作流，以生成设计产物。
handoffs: 
  - label: 创建任务
    agent: speckit.tasks
    prompt: 将计划分解为任务
    send: true
  - label: 创建检查表
    agent: speckit.checklist
    prompt: 为以下领域创建检查表...
scripts:
  sh: scripts/bash/setup-plan.sh --json
  ps: scripts/powershell/setup-plan.ps1 -Json
agent_scripts:
  sh: scripts/bash/update-agent-context.sh __AGENT__
  ps: scripts/powershell/update-agent-context.ps1 -AgentType __AGENT__
---

## 用户输入

```text
$ARGUMENTS
```

在继续之前，您**必须**考虑用户输入（如果不为空）。

## 纲要

1. **设置**：从仓库根目录运行 `{SCRIPT}`，并解析 FEATURE_SPEC, IMPL_PLAN, SPECS_DIR, BRANCH 的 JSON。对于参数中包含单引号的情况（如 "I'm Groot"），请使用转义语法：例如 'I'\''m Groot'（或者尽可能使用双引号:"I'm Groot"）。

2. **加载上下文**：读取 FEATURE_SPEC 和 `/memory/constitution.md`。加载 IMPL_PLAN 模板（已复制）。

3. **执行计划工作流**：遵循 IMPL_PLAN 模板中的结构以：
   - 填充“技术上下文”（将未知项标记为“需澄清”）
   - 从宪法填充“宪法检查”章节
   - 评估门禁（如果违规且无合理解释，则报错 ERROR）
   - 第 0 阶段：生成 research.md（解决所有“需澄清”项）
   - 第 1 阶段：生成 data-model.md, contracts/, quickstart.md
   - 第 1 阶段：通过运行 agent 脚本更新 agent 上下文
   - 设计后重新评估“宪法检查”

4. **停止并报告**：命令在第 2 阶段规划后结束。报告分支、IMPL_PLAN 路径和生成的产物。

## 阶段

### 第 0 阶段：大纲与研究

1. **从上方的“技术上下文”中提取未知项**：
   - 对于每个“需澄清”项 → 研究任务
   - 对于每个依赖项 → 最佳实践任务
   - 对于每个集成项 → 模式任务

2. **生成并分发研究 agent**：

   ```text
   对于技术上下文中的每个未知项：
     任务: "针对 {功能上下文} 研究 {未知项}"
   对于每个技术选择：
     任务: "在 {领域} 中寻找 {技术} 的最佳实践"
   ```

3. **整合发现**在 `research.md` 中，使用以下格式：
   - 决策：[选择了什么]
   - 理由：[为什么选择]
   - 考虑过的替代方案：[还评估了什么]

**输出**：已解决所有“需澄清”项的 research.md

### 第 1 阶段：设计与契约

**前提条件：** `research.md` 已完成

1. **从功能规格说明中提取实体** → `data-model.md`：
   - 实体名称、字段、关系
   - 来自需求的验证规则
   - 状态转换（如果适用）

2. **根据功能需求生成 API 契约**：
   - 每个用户操作 → 端点
   - 使用标准的 REST/GraphQL 模式
   - 将 OpenAPI/GraphQL 模式输出到 `/contracts/`

3. **Agent 上下文更新**：
   - 运行 `{AGENT_SCRIPT}`
   - 这些脚本会检测正在使用哪个 AI agent
   - 更新相应的特定于 agent 的上下文文件
   - 仅从当前计划中添加新技术
   - 在标记之间保留手动添加的内容

**输出**：data-model.md, /contracts/*, quickstart.md, agent 特有文件

## 关键规则

- 使用绝对路径
- 门禁失败或澄清未解决时报错 ERROR
