---
description: 根据现有的设计产物，为该功能生成可操作的、按依赖关系排序的任务列表 tasks.md。
handoffs: 
  - label: 分析一致性
    agent: speckit.analyze
    prompt: 运行项目分析以检查一致性
    send: true
  - label: 实现项目
    agent: speckit.implement
    prompt: 开始分阶段实现
    send: true
scripts:
  sh: scripts/bash/check-prerequisites.sh --json
  ps: scripts/powershell/check-prerequisites.ps1 -Json
---

## 用户输入

```text
$ARGUMENTS
```

在继续之前，您**必须**考虑用户输入（如果不为空）。

## 纲要

1. **设置**：从仓库根目录运行 `{SCRIPT}`，并解析 FEATURE_DIR 和 AVAILABLE_DOCS 列表。所有路径必须是绝对路径。对于参数中包含单引号的情况（如 "I'm Groot"），请使用转义语法：例如 'I'\''m Groot'（或者尽可能使用双引号：