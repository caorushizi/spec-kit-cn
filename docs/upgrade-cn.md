# 升级指南

> 您已安装了 Spec Kit，并希望升级到最新版本以获取新功能、错误修复或更新的斜杠命令。本指南涵盖了升级 CLI 工具和更新项目文件两部分内容。

---

## 快速参考

| 升级对象 | 命令 | 适用场景 |
|----------------|---------|-------------|
| **仅限 CLI 工具** | `uv tool install specify-cli --force --from git+https://github.com/github/spec-kit.git` | 获取最新的 CLI 功能而不触动项目文件 |
| **项目文件** | `specify init --here --force --ai <your-agent>` | 更新项目中的斜杠命令、模板和脚本 |
| **两者** | 先运行 CLI 升级，再运行项目更新 | 推荐用于重大版本更新 |

---

## 第一部分：升级 CLI 工具

CLI 工具 (`specify`) 与您的项目文件是分开的。升级它可以获取最新的功能和错误修复。

### 如果您使用 `uv tool install` 安装

```bash
uv tool install specify-cli --force --from git+https://github.com/github/spec-kit.git
```

### 如果您使用一次性 `uvx` 命令

不需要升级 —— `uvx` 总是会获取最新版本。只需照常运行命令：

```bash
uvx --from git+https://github.com/github/spec-kit.git specify init --here --ai copilot
```

### 验证升级

```bash
specify check
```

这将显示已安装的工具并确认 CLI 正在工作。

---

## 第二部分：更新项目文件

当 Spec Kit 发布新功能（如新的斜杠命令或更新的模板）时，您需要刷新项目的 Spec Kit 文件。

### 哪些内容会被更新？

运行 `specify init --here --force` 将更新：

- ✅ **斜杠命令文件** (`.claude/commands/`, `.github/prompts/` 等)
- ✅ **脚本文件** (`.specify/scripts/`)
- ✅ **模板文件** (`.specify/templates/`)
- ✅ **共享内存文件** (`.specify/memory/`) - **⚠️ 请参阅下方的警告**

### 哪些内容是安全的？

这些文件在升级过程中**绝不会被触动** —— 模板包中甚至不包含它们：

- ✅ **您的规格说明 (Specs)** (`specs/001-my-feature/spec.md` 等) - **确认安全**
- ✅ **您的实现计划** (`specs/001-my-feature/plan.md`, `tasks.md` 等) - **确认安全**
- ✅ **您的源代码** - **确认安全**
- ✅ **您的 git 历史记录** - **确认安全**

`specs/` 目录完全排除在模板包之外，在升级期间永远不会被修改。

### 更新命令

在您的项目目录内运行：

```bash
specify init --here --force --ai <your-agent>
```

将 `<your-agent>` 替换为您的 AI 助手。参考[受支持的 AI Agent 列表](../README-cn.md#-支持的-ai-agent)。

**示例：**

```bash
specify init --here --force --ai copilot
```

### 理解 `--force` 标志

如果没有 `--force`，CLI 会发出警告并要求确认：

```text
Warning: Current directory is not empty (25 items)
Template files will be merged with existing content and may overwrite existing files
Proceed? [y/N]
```

使用 `--force`，它将跳过确认并立即执行。

**重要提示：您的 `specs/` 目录始终是安全的。** `--force` 标志仅影响模板文件（命令、脚本、模板、内存）。`specs/` 中的功能规格说明、计划和任务绝不会包含在升级包中，因此无法被覆盖。

---

## ⚠️ 重要警告

### 1. 宪法文件将被覆盖

**已知问题**：`specify init --here --force` 目前会使用默认模板覆盖 `.specify/memory/constitution.md`，从而擦除您所做的任何自定义。

**解决方法：**

```bash
# 1. 在升级前备份您的宪法文件
cp .specify/memory/constitution.md .specify/memory/constitution-backup.md

# 2. 运行升级
specify init --here --force --ai copilot

# 3. 恢复您的自定义宪法文件
mv .specify/memory/constitution-backup.md .specify/memory/constitution.md
```

或者使用 git 来恢复它：

```bash
# 升级后，从 git 历史记录中恢复
git restore .specify/memory/constitution.md
```

### 2. 自定义模板修改

如果您自定义了 `.specify/templates/` 中的任何模板，升级将覆盖它们。请先备份：

```bash
# 备份自定义模板
cp -r .specify/templates .specify/templates-backup

# 升级后，手动合并您的更改
```

### 3. 重复的斜杠命令（基于 IDE 的 agent）

某些基于 IDE 的 agent（如 Kilo Code, Windsurf）在升级后可能会显示**重复的斜杠命令** —— 旧版本和新版本都会出现。

**解决方案**：手动从 agent 文件夹中删除旧的命令文件。

**Kilo Code 示例：**

```bash
# 进入 agent 的命令文件夹
cd .kilocode/rules/

# 列出文件并识别重复项
ls -la

# 删除旧版本（文件名示例 —— 您的文件名可能不同）
rm speckit.specify-old.md
rm speckit.plan-v1.md
```

重启您的 IDE 以刷新命令列表。

---

## 常见场景

### 场景 1：“我只想要新的斜杠命令”

```bash
# 升级 CLI（如果使用持久安装）
uv tool install specify-cli --force --from git+https://github.com/github/spec-kit.git

# 更新项目文件以获取新命令
specify init --here --force --ai copilot

# 如果有自定义宪法，请恢复它
git restore .specify/memory/constitution.md
```

### 场景 2：“我自定义了模板和宪法”

```bash
# 1. 备份自定义内容
cp .specify/memory/constitution.md /tmp/constitution-backup.md
cp -r .specify/templates /tmp/templates-backup

# 2. 升级 CLI
uv tool install specify-cli --force --from git+https://github.com/github/spec-kit.git

# 3. 更新项目
specify init --here --force --ai copilot

# 4. 恢复自定义内容
mv /tmp/constitution-backup.md .specify/memory/constitution.md
# 如果需要，手动合并模板更改
```

### 场景 3：“我在 IDE 中看到了重复的斜杠命令”

这发生在基于 IDE 的 agent（Kilo Code, Windsurf, Roo Code 等）上。

```bash
# 找到 agent 文件夹（例如：.kilocode/rules/）
cd .kilocode/rules/

# 列出所有文件
ls -la

# 删除旧的命令文件
rm speckit.old-command-name.md

# 重启您的 IDE
```

### 场景 4：“我在没有使用 Git 的项目上工作”

如果您在初始化项目时使用了 `--no-git`，您仍然可以升级：

```bash
# 手动备份您自定义的文件
cp .specify/memory/constitution.md /tmp/constitution-backup.md

# 运行升级
specify init --here --force --ai copilot --no-git

# 恢复自定义内容
mv /tmp/constitution-backup.md .specify/memory/constitution.md
```

`--no-git` 标志跳过 git 初始化，但不影响文件更新。

---

## 使用 `--no-git` 标志

`--no-git` 标志告诉 Spec Kit **跳过 git 仓库初始化**。这在以下情况很有用：

- 您使用不同的版本控制系统（Mercurial, SVN 等）
- 您的项目是具有现有 git 设置的大型单体仓库 (monorepo) 的一部分
- 您正在进行实验，尚不需要版本控制

**在初始设置期间：**

```bash
specify init my-project --ai copilot --no-git
```

**在升级期间：**

```bash
specify init --here --force --ai copilot --no-git
```

### `--no-git` 不会执行的操作

❌ 不会阻止文件更新
❌ 不会跳过斜杠命令安装
❌ 不会影响模板合并

它**仅**跳过运行 `git init` 和创建初始提交。

### 在没有 Git 的环境下工作

如果您使用 `--no-git`，则需要手动管理功能目录：

在运用规划命令之前，**设置 `SPECIFY_FEATURE` 环境变量**：

```bash
# Bash/Zsh
export SPECIFY_FEATURE="001-my-feature"

# PowerShell
$env:SPECIFY_FEATURE = "001-my-feature"
```

这告诉 Spec Kit 在创建规格说明、计划和任务时应使用哪个功能目录。

**为什么这很重要**：没有 git，Spec Kit 无法检测到您的当前分支名称来确定活动功能。环境变量手动提供了该上下文。

---

## 故障排除

### “升级后不显示斜杠命令”

**原因**：Agent 未重新加载命令文件。

**解决方法：**

1. **完全重启您的 IDE/编辑器**（而不仅仅是重新加载窗口）
2. **对于基于 CLI 的 agent**，验证文件是否存在：

   ```bash
   ls -la .claude/commands/      # Claude Code
   ls -la .gemini/commands/       # Gemini
   ls -la .cursor/commands/       # Cursor
   ```

3. **检查特定于 agent 的设置：**
   - Codex 需要 `CODEX_HOME` 环境变量
   - 某些 agent 需要重启工作区或清除缓存

### “我丢失了自定义的宪法内容”

**解决方法**：从 git 或备份中恢复：

```bash
# 如果在升级前已提交
git restore .specify/memory/constitution.md

# 如果已手动备份
cp /tmp/constitution-backup.md .specify/memory/constitution.md
```

**预防措施**：在升级前始终提交或备份 `constitution.md`。

### “警告：当前目录不为空 (Warning: Current directory is not empty)”

**完整警告消息：**

```text
Warning: Current directory is not empty (25 items)
Template files will be merged with existing content and may overwrite existing files
Do you want to continue? [y/N]
```

**这意味着什么：**

当您在已经包含文件的目录中运行 `specify init --here`（或 `specify init .`）时，会出现此警告。它在告诉您：

1. **该目录已有内容** —— 在示例中为 25 个文件/文件夹
2. **文件将被合并** —— 新的模板文件将与您现有的文件并存
3. **某些文件可能会被覆盖** —— 如果您已经拥有 Spec Kit 文件（`.claude/`, `.specify/` 等），它们将被替换为新版本

**哪些内容会被覆盖：**

仅 Spec Kit 基础设施文件：

- Agent 命令文件 (`.claude/commands/`, `.github/prompts/` 等)
- `.specify/scripts/` 中的脚本
- `.specify/templates/` 中的模板
- `.specify/memory/` 中的内存文件（包括宪法）

**哪些内容保持不变：**

- 您的 `specs/` 目录（规格说明、计划、任务）
- 您的源代码文件
- 您的 `.git/` 目录和 git 历史记录
- 任何不属于 Spec Kit 模板的其他文件

**如何响应：**

- **输入 `y` 并按回车** —— 继续合并（建议在升级时使用）
- **输入 `n` 并按回车** —— 取消操作
- **使用 `--force` 标志** —— 完全跳过此确认：

  ```bash
  specify init --here --force --ai copilot
  ```

**何时会看到此警告：**

- ✅ 升级现有的 Spec Kit 项目时**在意料之中**
- ✅ 向现有代码库添加 Spec Kit 时**在意料之中**
- ⚠️ 如果您以为是在空目录中创建新项目，则为**意料之外**

**预防小贴士**：在升级前，如果您自定义了 `.specify/memory/constitution.md`，请先提交或备份它。

### “CLI 升级似乎不起作用”

验证安装：

```bash
# 检查已安装的工具
uv tool list

# 应显示 specify-cli

# 验证路径
which specify

# 应指向 uv 工具安装目录
```

如果未找到，请重新安装：

```bash
uv tool uninstall specify-cli
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git
```

### “每次打开项目时都需要运行 specify 吗？”

**简短回答**：不需要。每个项目只需运行一次 `specify init`（或在升级时运行）。

**详细说明：**

`specify` CLI 工具用于：

- **初始设置**：使用 `specify init` 在您的项目中引导 Spec Kit
- **升级**：使用 `specify init --here --force` 更新模板和命令
- **诊断**：使用 `specify check` 验证工具安装

一旦运行了 `specify init`，斜杠命令（如 `/speckit.specify`, `/speckit.plan` 等）就会**永久安装**在您的项目 agent 文件夹中（`.claude/`, `.github/prompts/` 等）。您的 AI 助手会直接读取这些命令文件 —— 无需再次运行 `specify`。

**如果您的 agent 无法识别斜杠命令：**

1. **验证命令文件是否存在：**

   ```bash
   # 对于 GitHub Copilot
   ls -la .github/prompts/

   # 对于 Claude
   ls -la .claude/commands/
   ```

2. **完全重启您的 IDE/编辑器**（而不仅仅是重新加载窗口）

3. **检查您是否在运行 `specify init` 的正确目录中**

4. **对于某些 agent**，您可能需要重新加载工作区或清除缓存

**相关问题**：如果 Copilot 无法打开本地文件或意外使用 PowerShell 命令，这通常是 IDE 上下文问题，与 `specify` 无关。尝试：

- 重启 VS Code
- 检查文件权限
- 确保工作区文件夹已正确打开

---

## 版本兼容性

Spec Kit 的重大发布遵循语义化版本控制。CLI 和项目文件被设计为在同一个大版本内兼容。

**最佳实践**：在重大版本变更期间，通过同时升级 CLI 和项目文件来保持同步。

---

## 后续步骤

升级后：

- **测试新的斜杠命令**：运行 `/speckit.constitution` 或其他命令以验证一切正常
- **查看发布说明**：检查 [GitHub Releases](https://github.com/github/spec-kit/releases) 以了解新功能和破坏性变更
- **更新工作流**：如果添加了新命令，请更新您团队的开发工作流
- **查看文档**：访问 [github.io/spec-kit](https://github.github.io/spec-kit/) 获取更新的指南
