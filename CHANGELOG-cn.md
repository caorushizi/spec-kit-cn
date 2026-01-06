# 更新日志

<!-- markdownlint-disable MD024 -->

Specify CLI 和模板的所有显著更改都记录在此处。

本文件的格式基于 [Keep a Changelog](https://keepachangelog.com/en/1.0.0/)，
本项目遵循 [语义化版本 (Semantic Versioning)](https://semver.org/spec/v2.0.0.html)。

## [0.0.22] - 2025-11-07

- 支持 VS Code/Copilot agent，并从提示词转向带有移交 (hand-offs) 的正式 agent。
- 转向使用 `AGENTS.md` 来处理 Copilot 工作负载，因为它已经开箱即用支持。
- 添加了对 version 命令的支持。 ([#486](https://github.com/github/spec-kit/issues/486))
- 修复了 `create-new-feature.ps1` 脚本在确定下一个功能编号时忽略现有功能分支的潜在错误 ([#975](https://github.com/github/spec-kit/issues/975))
- 为模板获取期间的 GitHub API 速率限制添加了优雅降级和日志记录 ([#970](https://github.com/github/spec-kit/issues/970))

## [0.0.21] - 2025-10-21

- 修复了 [#975](https://github.com/github/spec-kit/issues/975) (感谢 [@fgalarraga](https://github.com/fgalarraga))。
- 添加了对 Amp CLI 的支持。
- 添加了对 VS Code 移交的支持，并将提示词迁移为完整的聊天模式。
- 添加了对 `version` 命令的支持 (解决了 [#811](https://github.com/github/spec-kit/issues/811) 和 [#486](https://github.com/github/spec-kit/issues/486)，感谢 [@mcasalaina](https://github.com/mcasalaina) 和 [@dentity007](https://github.com/dentity007))。
- 添加了在遇到速率限制错误时从 CLI 渲染该错误的支持 ([#970](https://github.com/github/spec-kit/issues/970)，感谢 [@psmman](https://github.com/psmman))。

## [0.0.20] - 2025-10-14

### 新增

- **智能分支命名**：`create-new-feature` 脚本现在支持用于自定义分支名称的 `--short-name` 参数
  - 当提供 `--short-name` 时：直接使用自定义名称（经过清理和格式化）
  - 当省略时：使用停用词过滤和基于长度的过滤自动生成有意义的名称
  - 过滤掉常见的停用词（I, want, to, the, for 等）
  - 移除短于 3 个字符的单词（除非它们是大写首字母缩略词）
  - 从描述中提取 3-4 个最有意义的单词
  - **强制执行 GitHub 的 244 字节分支名称限制**，并带有自动截断和警告
  - 示例：
    - "I want to create user authentication" → `001-create-user-authentication`
    - "Implement OAuth2 integration for API" → `001-implement-oauth2-integration-api`
    - "Fix payment processing bug" → `001-fix-payment-processing`
    - 非常长的描述会在单词边界处自动截断，以保持在限制范围内
  - 旨在为 AI agent 提供语义化的短名称，同时保持独立可用性

### 变更

- 增强了 `create-new-feature.sh` 和 `create-new-feature.ps1` 脚本的帮助文档并附带示例
- 现在根据 GitHub 的 244 字节限制验证分支名称，并在需要时自动截断

## [0.0.19] - 2025-10-10

### 新增

- 支持 CodeBuddy (感谢 [@lispking](https://github.com/lispking) 的贡献)。
- 您现在可以在 Specify CLI 中看到 Git 来源的错误。

### 变更

- 修复了 `plan.md` 中指向 constitution 的路径 (感谢 [@lyzno1](https://github.com/lyzno1) 发现)。
- 修复了 Gemini 生成的 TOML 文件中的反斜杠转义 (感谢 [@hsin19](https://github.com/hsin19) 的贡献)。
- 实现命令现在确保添加了正确的忽略文件 (感谢 [@sigent-amazon](https://github.com/sigent-amazon) 的贡献)。

## [0.0.18] - 2025-10-06

### 新增

- 支持在 `specify init .` 命令中将 `.` 用作当前目录的简写，等同于 `--here` 标志，但对用户来说更直观。
- 使用 `/speckit.` 命令前缀以轻松发现 Spec Kit 相关的命令。
- 重构了提示词和模板，以简化它们的能力以及它们的跟踪方式。不再在不需要时让测试污染环境。
- 确保按用户故事创建任务（简化测试和验证）。
- 添加了对 Visual Studio Code 提示词快捷方式和脚本自动执行的支持。

### 变更

- 所有命令文件现在都以 `speckit.` 为前缀（例如 `speckit.specify.md`, `speckit.plan.md`），以便在 IDE/CLI 命令面板和文件资源管理器中获得更好的可发现性和区分度。

## [0.0.17] - 2025-09-22

### 新增

- 新的 `/clarify` 命令模板，针对现有规格说明提出最多 5 个有针对性的澄清问题，并将答案持久化到规格说明的“澄清 (Clarifications)”部分。
- 新的 `/analyze` 命令模板，提供非破坏性的跨产物差异和对齐报告（规格说明、澄清、计划、任务、宪法），在 `/tasks` 之后和 `/implement` 之前插入。
  - 注意：宪法规则被明确视为不可协商的；任何冲突都是需要进行产物修复的“关键发现”，而不是弱化原则。

## [0.0.16] - 2025-09-22

### 新增

- 为 `init` 命令添加了 `--force` 标志，以便在非空目录中使用 `--here` 时跳过确认并继续合并/覆盖文件。

## [0.0.15] - 2025-09-21

### 新增

- 支持 Roo Code。

## [0.0.14] - 2025-09-21

### 变更

- 现在一致地显示错误消息。

## [0.0.13] - 2025-09-21

### 新增

- 支持 Kilo Code。感谢 [@shahrukhkhan489](https://github.com/shahrukhkhan489) 的 [#394](https://github.com/github/spec-kit/pull/394)。
- 支持 Auggie CLI。感谢 [@hungthai1401](https://github.com/hungthai1401) 的 [#137](https://github.com/github/spec-kit/pull/137)。
- 项目配置完成后显示 Agent 文件夹安全通知，警告用户某些 agent 可能会在其 agent 文件夹中存储凭据或身份验证令牌，并建议将相关文件夹添加到 `.gitignore` 以防止意外的凭据泄露。

### 变更

- 显示警告，以确保人们意识到他们可能需要将 agent 文件夹添加到 `.gitignore`。
- 清理了 `check` 命令的输出。

## [0.0.12] - 2025-09-21

### 变更

- 为 OpenAI Codex 用户添加了额外上下文 —— 他们需要设置一个额外的环境变量，如 [#417](https://github.com/github/spec-kit/issues/417) 中所述。

## [0.0.11] - 2025-09-20

### 新增

- 支持 Codex CLI (感谢 [@honjo-hiroaki-gtt](https://github.com/honjo-hiroaki-gtt) 在 [#14](https://github.com/github/spec-kit/pull/14) 中的贡献)
- 支持 Codex 的上下文更新工具 (Bash 和 PowerShell)，使得功能计划在无需手动编辑的情况下，能与现有助手一起刷新 `AGENTS.md`。

## [0.0.10] - 2025-09-20

### 修复

- 解决了 [#378](https://github.com/github/spec-kit/issues/378)，其中当 GitHub 令牌为空时，仍可能被附加到请求中。

## [0.0.9] - 2025-09-19

### 变更

- 改进了 agent 选择器 UI，为 agent 键使用青色高亮，为全名使用灰色括号

## [0.0.8] - 2025-09-19

### 新增

- 支持 Windsurf IDE 作为额外的 AI 助手选项 (感谢 [@raedkit](https://github.com/raedkit) 在 [#151](https://github.com/github/spec-kit/pull/151) 中的工作)
- 用于 API 请求的 GitHub 令牌支持，以处理企业环境和速率限制 (由 [@zryfish](https://github.com/@zryfish) 在 [#243](https://github.com/github/spec-kit/pull/243) 中贡献)

### 变更

- 更新了 README，增加了 Windsurf 示例和 GitHub 令牌用法
- 增强了发布工作流，以包含 Windsurf 模板

## [0.0.7] - 2025-09-18

### 变更

- 更新了 CLI 中的命令指令。
- 清理了代码，使其在信息通用的情况下不渲染 agent 特定信息。

## [0.0.6] - 2025-09-17

### 新增

- 支持 opencode 作为额外的 AI 助手选项

## [0.0.5] - 2025-09-17

### 新增

- 支持 Qwen Code 作为额外的 AI 助手选项

## [0.0.4] - 2025-09-14

### 新增

- 通过 `httpx[socks]` 依赖为企业环境提供 SOCKS 代理支持

### 修复

无

### 变更

无
