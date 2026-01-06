# 本地开发指南

本指南展示了如何在不发布版本或不先提交到 `main` 分支的情况下，在本地对 `specify` CLI 进行迭代。

> 脚本现在同时具有 Bash (`.sh`) 和 PowerShell (`.ps1`) 变体。除非您传递 `--script sh|ps`，否则 CLI 会根据操作系统自动选择。

## 1. 克隆并切换分支

```bash
git clone https://github.com/github/spec-kit.git
cd spec-kit
# 在功能分支上工作
git checkout -b your-feature-branch
```

## 2. 直接运行 CLI（最快反馈）

您可以直接通过模块入口执行 CLI，而无需安装任何东西：

```bash
# 从仓库根目录运行
python -m src.specify_cli --help
python -m src.specify_cli init demo-project --ai claude --ignore-agent-tools --script sh
```

如果您更喜欢脚本文件式的调用（使用 shebang）：

```bash
python src/specify_cli/__init__.py init demo-project --script ps
```

## 3. 使用可编辑安装 (Editable Install)（隔离环境）

使用 `uv` 创建隔离环境，使依赖关系解析与最终用户完全一致：

```bash
# 创建并激活虚拟环境（uv 会自动管理 .venv）
uv venv
source .venv/bin/activate  # 或者在 Windows PowerShell 中：.venv\Scripts\Activate.ps1

# 以可编辑模式安装项目
uv pip install -e .

# 现在 'specify' 入口已可用
specify --help
```

代码编辑后重新运行无需重新安装，因为处于可编辑模式。

## 4. 直接从 Git 调用 uvx（当前分支）

`uvx` 可以从本地路径（或 Git 引用）运行，以模拟用户流程：

```bash
uvx --from . specify init demo-uvx --ai copilot --ignore-agent-tools --script sh
```

您还可以将 uvx 指向特定分支而不进行合并：

```bash
# 先推送您的工作分支
git push origin your-feature-branch
uvx --from git+https://github.com/github/spec-kit.git@your-feature-branch specify init demo-branch-test --script ps
```

### 4a. 绝对路径 uvx（从任何地方运行）

如果您在另一个目录中，请使用绝对路径而不是 `.`：

```bash
uvx --from /mnt/c/GitHub/spec-kit specify --help
uvx --from /mnt/c/GitHub/spec-kit specify init demo-anywhere --ai copilot --ignore-agent-tools --script sh
```

为方便起见，设置一个环境变量：

```bash
export SPEC_KIT_SRC=/mnt/c/GitHub/spec-kit
uvx --from "$SPEC_KIT_SRC" specify init demo-env --ai copilot --ignore-agent-tools --script ps
```

（可选）定义一个 shell 函数：

```bash
specify-dev() { uvx --from /mnt/c/GitHub/spec-kit specify "$@"; }
# 然后使用
specify-dev --help
```

## 5. 测试脚本权限逻辑

运行 `init` 后，检查 POSIX 系统上的 shell 脚本是否具有可执行权限：

```bash
ls -l scripts | grep .sh
# 期望有所有者执行位（例如 -rwxr-xr-x）
```

在 Windows 上，您将使用 `.ps1` 脚本（无需 chmod）。

## 6. 运行 Lint / 基础检查（添加您自己的检查）

目前尚未绑定强制执行的 lint 配置，但您可以快速进行导入性检查：

```bash
python -c "import specify_cli; print('Import OK')"
```

## 7. 在本地构建 Wheel（可选）

在发布前验证打包情况：

```bash
uv build
ls dist/
```

如果需要，将构建的产物安装到一个临时的丢弃环境中。

## 8. 使用临时工作区

在脏目录中测试 `init --here` 时，创建一个临时工作区：

```bash
mkdir /tmp/spec-test && cd /tmp/spec-test
python -m src.specify_cli init --here --ai claude --ignore-agent-tools --script sh  # 如果仓库已复制到此
```

或者如果您想要一个更轻量级的沙箱，只复制修改后的 CLI 部分。

## 9. 调试网络 / 跳过 TLS

如果您在实验时需要绕过 TLS 验证：

```bash
specify check --skip-tls
specify init demo --skip-tls --ai gemini --ignore-agent-tools --script ps
```

（仅用于本地实验。）

## 10. 快速编辑循环总结

| 动作 | 命令 |
|--------|---------|
| 直接运行 CLI | `python -m src.specify_cli --help` |
| 可编辑安装 | `uv pip install -e .` 然后运行 `specify ...` |
| 本地 uvx 运行 (仓库根目录) | `uvx --from . specify ...` |
| 本地 uvx 运行 (绝对路径) | `uvx --from /mnt/c/GitHub/spec-kit specify ...` |
| Git 分支 uvx 运行 | `uvx --from git+URL@branch specify ...` |
| 构建 wheel | `uv build` |

## 11. 清理

快速移除构建产物 / 虚拟环境：

```bash
rm -rf .venv dist build *.egg-info
```

## 12. 常见问题

| 现象 | 解决方法 |
|---------|-----|
| `ModuleNotFoundError: typer` | 运行 `uv pip install -e .` |
| 脚本不可执行 (Linux) | 重新运行 init 或 `chmod +x scripts/*.sh` |
| 跳过了 Git 步骤 | 您传递了 `--no-git` 或未安装 Git |
| 下载了错误的脚本类型 | 显式传递 `--script sh` 或 `--script ps` |
| 公司网络上的 TLS 错误 | 尝试使用 `--skip-tls`（不用于生产环境） |

## 13. 后续步骤

- 更新文档并使用修改后的 CLI 运行一遍快速入门流程
- 满意后开启 PR
- （可选）一旦更改合并到 `main`，标记发布一个版本
