# AGENTS.md

## 关于 Spec Kit 和 Specify

**GitHub Spec Kit** 是一个用于实施规格驱动开发 (Spec-Driven Development, SDD) 的综合工具包 —— 这种方法论强调在实现之前先创建清晰的规格说明。该工具包包括模板、脚本和工作流，指导开发团队通过结构化的方法来构建软件。

**Specify CLI** 是用于引导 Spec Kit 框架项目的命令行界面。它设置必要的目录结构、模板和 AI agent 集成，以支持规格驱动开发流程。

该工具包支持多种 AI 编码助手，允许团队在保持一致的项目结构和开发实践的同时使用他们偏好的工具。

---

## 通用实践

- 对 Specify CLI 的 `__init__.py` 进行任何更改都需要在 `pyproject.toml` 中更新版本号，并向 `CHANGELOG.md` 添加条目。

## 添加新的 Agent 支持

本节说明如何向 Specify CLI 添加对新 AI agent/助手的支持。在将新的 AI 工具集成到规格驱动开发流程中时，请以此指南作为参考。

### 概览

Specify 通过在初始化项目时生成特定于 agent 的命令文件和目录结构来支持多个 AI agent。每个 agent 都有自己的规范：

- **命令文件格式**（Markdown, TOML 等）
- **目录结构**（`.claude/commands/`, `.windsurf/workflows/` 等）
- **命令调用模式**（斜杠命令、CLI 工具等）
- **参数传递约定**（`$ARGUMENTS`, `{{args}}` 等）

### 当前支持的 Agent

| Agent                      | 目录                  | 格式     | CLI 工具        | 描述                       |
| -------------------------- | --------------------- | -------- | --------------- | -------------------------- |
| **Claude Code**            | `.claude/commands/`   | Markdown | `claude`        | Anthropic 的 Claude Code CLI |
| **Gemini CLI**             | `.gemini/commands/`   | TOML     | `gemini`        | Google 的 Gemini CLI       |
| **GitHub Copilot**         | `.github/agents/`     | Markdown | 无 (基于 IDE)   | VS Code 中的 GitHub Copilot |
| **Cursor**                 | `.cursor/commands/`   | Markdown | `cursor-agent`  | Cursor CLI                 |
| **Qwen Code**              | `.qwen/commands/`     | TOML     | `qwen`          | 阿里巴巴的 Qwen Code CLI     |
| **opencode**               | `.opencode/command/`  | Markdown | `opencode`      | opencode CLI               |
| **Codex CLI**              | `.codex/commands/`    | Markdown | `codex`         | Codex CLI                  |
| **Windsurf**               | `.windsurf/workflows/`| Markdown | 无 (基于 IDE)   | Windsurf IDE 工作流        |
| **Kilo Code**              | `.kilocode/rules/`    | Markdown | 无 (基于 IDE)   | Kilo Code IDE              |
| **Auggie CLI**             | `.augment/rules/`     | Markdown | `auggie`        | Auggie CLI                 |
| **Roo Code**               | `.roo/rules/`         | Markdown | 无 (基于 IDE)   | Roo Code IDE               |
| **CodeBuddy CLI**          | `.codebuddy/commands/`| Markdown | `codebuddy`     | CodeBuddy CLI              |
| **Qoder CLI**              | `.qoder/commands/`    | Markdown | `qoder`         | Qoder CLI                  |
| **Amazon Q Developer CLI** | `.amazonq/prompts/`   | Markdown | `q`             | Amazon Q Developer CLI     |
| **Amp**                    | `.agents/commands/`   | Markdown | `amp`           | Amp CLI                    |
| **SHAI**                   | `.shai/commands/`     | Markdown | `shai`          | SHAI CLI                   |
| **IBM Bob**                | `.bob/commands/`      | Markdown | 无 (基于 IDE)   | IBM Bob IDE                |

### 逐步集成指南

按照以下步骤添加新 agent（以一个假设的新 agent 为例）：

#### 1. 添加到 AGENT_CONFIG

**重要**：使用实际的 CLI 工具名称作为键，而不是缩写版本。

将新 agent 添加到 `src/specify_cli/__init__.py` 中的 `AGENT_CONFIG` 字典。这是所有 agent 元数据的**唯一事实来源**：

```python
AGENT_CONFIG = {
    # ... 现有 agent ...
    "new-agent-cli": {  # 使用实际的 CLI 工具名称（用户在终端中输入的名称）
        "name": "New Agent Display Name",
        "folder": ".newagent/",  # agent 文件的目录
        "install_url": "https://example.com/install",  # 安装文档的 URL（如果是基于 IDE 的则为 None）
        "requires_cli": True,  # 如果需要检查 CLI 工具则为 True，基于 IDE 的 agent 则为 False
    },
}
```

**核心设计原则**：字典键应与用户安装的实际可执行文件名匹配。例如：

- ✅ 使用 `