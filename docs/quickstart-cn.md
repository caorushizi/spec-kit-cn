# 快速入门指南

本指南将帮助您开始使用 Spec Kit 进行规格驱动开发 (Spec-Driven Development)。

> [!NOTE]
> 所有的自动化脚本现在都提供 Bash (`.sh`) 和 PowerShell (`.ps1`) 变体。除非您传递 `--script sh|ps`，否则 `specify` CLI 会根据操作系统自动选择。

## 6 步流程

> [!TIP]
> **上下文感知**：Spec Kit 命令会自动根据您当前的 Git 分支（例如 `001-feature-name`）检测活动功能。要切换不同的规格说明，只需切换 Git 分支即可。

### 第 1 步：安装 Specify

**在您的终端中**，运行 `specify` CLI 命令来初始化您的项目：

```bash
# 创建一个新的项目目录
uvx --from git+https://github.com/github/spec-kit.git specify init <PROJECT_NAME>

# 或者在当前目录中初始化
uvx --from git+https://github.com/github/spec-kit.git specify init .
```

显式选择脚本类型（可选）：

```bash
uvx --from git+https://github.com/github/spec-kit.git specify init <PROJECT_NAME> --script ps  # 强制使用 PowerShell
uvx --from git+https://github.com/github/spec-kit.git specify init <PROJECT_NAME> --script sh  # 强制使用 POSIX shell
```

### 第 2 步：定义您的宪法 (Constitution)

**在您的 AI Agent 聊天界面中**，使用 `/speckit.constitution` 斜杠命令为您的项目建立核心规则和原则。您应该提供项目特有的原则作为参数。

```markdown
/speckit.constitution 本项目遵循“库优先 (Library-First)”方法。所有功能必须首先作为独立的库实现。我们严格执行 TDD。我们偏好函数式编程模式。
```

### 第 3 步：创建规格说明 (Spec)

**在聊天中**，使用 `/speckit.specify` 斜杠命令来描述您想要构建的内容。关注**什么 (what)**和**为什么 (why)**，而不是技术栈。

```markdown
/speckit.specify 构建一个可以帮助我将照片整理到不同相册中的应用程序。相册按日期分组，并可以通过在主页上拖放来重新整理。相册永远不会嵌套在其他相册中。在每个相册内，照片以平铺式界面进行预览。
```

### 第 4 步：精炼规格说明

**在聊天中**，使用 `/speckit.clarify` 斜杠命令来识别并解决规格说明中的模糊之处。您可以提供特定的关注领域作为参数。

```bash
/speckit.clarify 关注安全性和性能要求。
```

### 第 5 步：创建技术实现计划

**在聊天中**，使用 `/speckit.plan` 斜杠命令来提供您的技术栈和架构选择。

```markdown
/speckit.plan 该应用程序使用 Vite，并尽可能减少库的数量。尽量使用原生 HTML、CSS 和 JavaScript。照片不上传到任何地方，元数据存储在本地 SQLite 数据库中。
```

### 第 6 步：分解并实现

**在聊天中**，使用 `/speckit.tasks` 斜杠命令来创建一个可操作的任务列表。

```markdown
/speckit.tasks
```

可选地，使用 `/speckit.analyze` 验证计划：

```markdown
/speckit.analyze
```

然后，使用 `/speckit.implement` 斜杠命令来执行计划。

```markdown
/speckit.implement
```

## 详细示例：构建 Taskify

以下是构建团队生产力平台的完整示例：

### 第 1 步：定义宪法

初始化项目宪法以设定基本规则：

```markdown
/speckit.constitution Taskify 是一个“安全第一”的应用程序。所有用户输入必须经过验证。我们使用微服务架构。代码必须有完整的文档。
```

### 第 2 步：使用 `/speckit.specify` 定义需求

```text
开发 Taskify，一个团队生产力平台。它应该允许用户创建项目、添加团队成员、
分配任务、发表评论并在看板风格的板块之间移动任务。在这个功能的初始阶段，
我们称之为“创建 Taskify”，让我们有多个用户，但用户将提前预定义。
我想要五个用户，分为两个不同的类别：一名产品经理和四名工程师。让我们创建三个
不同的示例项目。看板应具有标准的列来表示每个任务的状态，例如“待办 (To Do)”、“进行中 (In Progress)”、“评审中 (In Review)”和“完成 (Done)”。
这个应用程序不需要登录，因为这只是最初的测试，以确保我们的基本功能已设置好。
```

### 第 3 步：精炼规格说明

使用 `/speckit.clarify` 命令交互式地解决规格说明中的任何模糊之处。您还可以提供希望确保包含的特定细节。

```bash
/speckit.clarify 我想澄清任务卡片的细节。对于 UI 中的每个任务卡片，您应该能够更改看板工作板块中不同列之间的任务状态。您应该能够为特定的卡片留下无限数量的评论。您应该能够从该任务卡片分配一个有效用户。
```

您可以继续使用 `/speckit.clarify` 提供更多细节来精炼规格说明：

```bash
/speckit.clarify 当您第一次启动 Taskify 时，它将为您提供五个用户供您选择。不需要密码。当您点击一个用户时，您会进入主视图，其中显示项目列表。当您点击一个项目时，您会打开该项目的看板。您会看到这些列。您将能够在不同列之间来回拖放卡片。您会看到分配给您（当前登录用户）的任何卡片，其颜色与所有其他卡片不同，以便您可以快速看到自己的任务。您可以编辑自己发表的任何评论，但不能编辑别人发表的评论。您可以删除自己发表的任何评论，但不能删除别人发表的评论。
```

### 第 4 步：验证规格说明

使用 `/speckit.checklist` 命令验证规格说明检查表：

```bash
/speckit.checklist
```

### 第 5 步：使用 `/speckit.plan` 生成技术计划

明确您的技术栈和技术需求：

```bash
/speckit.plan 我们将使用 .NET Aspire 生成此应用程序，并使用 Postgres 作为数据库。前端应使用具有拖放式任务板块和实时更新功能的 Blazor server。应该创建一个 REST API，包含项目 API、任务 API 和通知 API。
```

### 第 6 步：验证并实现

让您的 AI agent 使用 `/speckit.analyze` 审计实现计划：

```bash
/speckit.analyze
```

最后，实现解决方案：

```bash
/speckit.implement
```

## 关键原则

- **明确说明**您在构建什么以及为什么
- 在规格说明阶段**不要关注技术栈**
- 在实现之前**迭代并精炼**您的规格说明
- 在开始编码之前**验证**计划
- **让 AI agent 处理**实现细节

## 后续步骤

- 阅读[完整的方法论](../spec-driven-cn.md)以获取深入指导
- 查看仓库中的[更多示例](../templates)
- 探索 [GitHub 上的源代码](https://github.com/github/spec-kit)
