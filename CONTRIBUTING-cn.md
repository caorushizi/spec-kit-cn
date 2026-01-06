# 为 Spec Kit 贡献代码

你好！我们很高兴你想为 Spec Kit 做出贡献。对本项目的贡献将根据[项目的开源许可证](LICENSE)向公众[发布](https://help.github.com/articles/github-terms-of-service/#6-contributions-under-repository-license)。

请注意，本项目的发布附带了[贡献者行为准则](CODE_OF_CONDUCT.md)。参与本项目即表示您同意遵守其条款。

## 运行和测试代码的前提条件

这些是本地测试更改（作为拉取请求 PR 提交过程的一部分）所需的一次性安装。

1. 安装 [Python 3.11+](https://www.python.org/downloads/)
1. 安装 [uv](https://docs.astral.sh/uv/) 用于包管理
1. 安装 [Git](https://git-scm.com/downloads)
1. 拥有可用的 [AI 编码 agent](README-cn.md#-支持的-ai-agent)

<details>
<summary><b>💡 如果您使用 <code>VSCode</code> 或 <code>GitHub Codespaces</code> 作为 IDE 的提示</b></summary>

<br>

只要您的机器上安装了 [Docker](https://docker.com)，您就可以通过这个 [VSCode 扩展](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) 利用 [Dev Containers](https://containers.dev)，轻松设置开发环境。多亏了位于项目根目录的 `.devcontainer/devcontainer.json` 文件，上述工具已经安装并配置好。

具体操作如下：

- 检出 (Checkout) 仓库
- 使用 VSCode 打开它
- 打开 [命令面板 (Command Palette)](https://code.visualstudio.com/docs/getstarted/userinterface#_command-palette) 并选择 "Dev Containers: Open Folder in Container..."

在 [GitHub Codespaces](https://github.com/features/codespaces) 上甚至更简单，因为它在打开 codespace 时会自动利用 `.devcontainer/devcontainer.json`。

</details>

## 提交拉取请求 (Pull Request)

> [!NOTE]
> 如果您的拉取请求引入了会对 CLI 的工作或仓库其余部分产生实质性影响的大型更改（例如，您引入了新的模板、参数或其他重大更改），请确保这已得到项目维护者的**讨论并达成一致**。未经事先沟通和达成一致的大型更改拉取请求将被关闭。

1. Fork 并克隆仓库
1. 配置并安装依赖：`uv sync`
1. 确保 CLI 在您的机器上可以运行：`uv run specify --help`
1. 创建一个新分支：`git checkout -b my-branch-name`
1. 进行更改，添加测试，并确保一切仍能正常工作
1. 如果相关，使用示例项目测试 CLI 功能
1. 推送到您的 fork 并提交拉取请求
1. 等待您的拉取请求被评审和合并。

以下是一些可以增加您的拉取请求被接受可能性的事项：

- 遵循项目的编码规范。
- 为新功能编写测试。
- 如果您的更改影响用户端功能，请更新文档（`README.md`, `spec-driven.md`）。
- 保持更改尽可能集中。如果您想进行多个互不依赖的更改，请考虑将它们作为单独的拉取请求提交。
- 编写[高质量的提交信息 (commit message)](http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html)。
- 使用规格驱动开发 (Spec-Driven Development) 工作流测试您的更改，以确保兼容性。

## 开发工作流

在开发 spec-kit 时：

1. 在您选择的编码 agent 中使用 `specify` CLI 命令（`/speckit.specify`, `/speckit.plan`, `/speckit.tasks`）测试更改
2. 验证 `templates/` 目录中的模板是否正常工作
3. 测试 `scripts/` 目录中的脚本功能
4. 如果流程发生重大变化，请确保更新内存文件 (`memory/constitution.md`)

### 在本地测试模板和命令更改

运行 `uv run specify init` 会拉取已发布的包，这不会包含您的本地更改。
要本地测试您的模板、命令和其他更改，请遵循以下步骤：

1. **创建发布包**

   运行以下命令生成本地包：

   ```bash
   ./.github/workflows/scripts/create-release-packages.sh v1.0.0
   ```

2. **将相关包复制到您的测试项目**

   ```bash
   cp -r .genreleases/sdd-copilot-package-sh/. <path-to-test-project>/
   ```

3. **打开并测试 agent**

   导航到您的测试项目文件夹并打开 agent 以验证您的实现。

## Spec Kit 中的 AI 贡献

> [!IMPORTANT]
>
> 如果您正在使用**任何形式的 AI 辅助**来为 Spec Kit 做出贡献，
> 必须在拉取请求或 issue 中披露。

我们欢迎并鼓励使用 AI 工具来帮助改进 Spec Kit！许多有价值的贡献在代码生成、问题检测和功能定义方面都得到了 AI 辅助的增强。

话虽如此，如果您在为 Spec Kit 贡献时使用了任何形式的 AI 辅助（例如 agent, ChatGPT），
**必须在拉取请求或 issue 中披露**，并说明 AI 辅助的使用程度（例如，文档注释 vs. 代码生成）。

如果您的 PR 回复或评论是由 AI 生成的，也请披露。

作为例外，微小的空格或拼写错误修复不需要披露，只要更改仅限于代码的一小部分或简短短语。

披露示例：

> 此 PR 主要是由 GitHub Copilot 编写的。

或者更详细的披露：

> 我咨询了 ChatGPT 以了解代码库，但解决方案完全是由我自己手动编写的。

未披露首先是对拉取请求另一端的人类操作员的不礼貌，其次也会让我们难以确定应对该贡献应用多少程度的审查。

在一个理想的世界里，AI 辅助会产生与人类同等或更高质量的作品。但这不是我们今天生活的世界，而且在大多数没有人类监督或专业知识参与的情况下，它生成的代码是无法合理维护或进化的。

### 我们寻求的贡献

在提交 AI 辅助的贡献时，请确保包含：

- **明确披露 AI 使用情况** —— 您对 AI 的使用及其在贡献中的使用程度保持透明
- **人类的理解和测试** —— 您亲自测试了这些更改并理解其作用
- **明确的理由** —— 您可以解释为什么需要该更改，以及它如何符合 Spec Kit 的目标
- **具体证据** —— 包含展示改进的测试用例、场景或示例
- **您自己的分析** —— 分享您对端到端开发者体验的看法

### 我们会关闭的贡献

我们保留关闭以下看似贡献的权利：

- 未经验证即提交的未测试更改
- 未解决 Spec Kit 特定需求的通用建议
- 未经人类审查或理解的大批量提交

### 成功准则

关键是证明您理解并验证了您提议的更改。如果维护者可以轻易看出贡献完全是由 AI 生成且没有经过人类输入或测试，那么在提交之前它可能需要更多工作。

持续提交低质量 AI 生成更改的贡献者，维护者可酌情限制其进一步贡献。

请尊重维护者并披露 AI 辅助。

## 资源

- [规格驱动开发方法论](./spec-driven-cn.md)
- [如何为开源做贡献](https://opensource.guide/how-to-contribute/)
- [使用拉取请求](https://help.github.com/articles/about-pull-requests/)
- [GitHub 帮助](https://help.github.com)
