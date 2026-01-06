# 实现计划：[功能名称]

**分支**: `[###-功能名称]` | **日期**: [日期] | **规格说明**: [链接]
**输入**: 来自 `/specs/[###-功能名称]/spec.md` 的功能规格说明

**注意**: 此模板由 `/speckit.plan` 命令填充。有关执行工作流，请参阅 `.specify/templates/commands/plan.md`。

## 摘要 (Summary)

[提取自功能规格说明：主要需求 + 来自研究的技术方案]

## 技术上下文 (Technical Context)

<!--
  需要操作：请将此部分的内容替换为项目的技术细节。
  此处的结构仅作为建议，用于指导迭代过程。
-->

**语言/版本**: [例如：Python 3.11, Swift 5.9, Rust 1.75 或 需澄清]  
**主要依赖**: [例如：FastAPI, UIKit, LLVM 或 需澄清]  
**存储**: [如果适用，例如：PostgreSQL, CoreData, 文件或 不适用]  
**测试**: [例如：pytest, XCTest, cargo test 或 需澄清]  
**目标平台**: [例如：Linux server, iOS 15+, WASM 或 需澄清]
**项目类型**: [单体/Web/移动 - 决定源代码结构]  
**性能目标**: [特定于领域，例如：1000 req/s, 10k lines/sec, 60 fps 或 需澄清]  
**约束条件**: [特定于领域，例如：<200ms p95, <100MB 内存, 支持离线或 需澄清]  
**规模/范围**: [特定于领域，例如：1万用户, 100万行代码, 50个界面 或 需澄清]

## 宪法检查 (Constitution Check)

*门禁：必须在第 0 阶段研究之前通过。在第 1 阶段设计之后重新检查。*

[基于宪法文件确定的门禁项]

## 项目结构 (Project Structure)

### 文档 (此功能)

```text
specs/[###-功能名称]/
├── plan.md              # 此文件 (/speckit.plan 命令输出)
├── research.md          # 第 0 阶段输出 (/speckit.plan 命令)
├── data-model.md        # 第 1 阶段输出 (/speckit.plan 命令)
├── quickstart.md        # 第 1 阶段输出 (/speckit.plan 命令)
├── contracts/           # 第 1 阶段输出 (/speckit.plan 命令)
└── tasks.md             # 第 2 阶段输出 (/speckit.tasks 命令 - 不是由 /speckit.plan 创建)
```

### 源代码 (仓库根目录)
<!--
  需要操作：请将下方的占位树替换为此功能的具体布局。
  删除未使用的选项，并使用实际路径（例如 apps/admin, packages/something）扩展所选结构。
  交付的计划中不应包含“选项 (Option)”标签。
-->

```text
# [如果未使用请删除] 选项 1：单项目 (默认)
src/
├── models/
├── services/
├── cli/
└── lib/

tests/
├── contract/
├── integration/
├── unit/

# [如果未使用请删除] 选项 2：Web 应用程序 (当检测到“前端”+“后端”时)
backend/
├── src/
│   ├── models/
│   ├── services/
│   └── api/
└── tests/

frontend/
├── src/
│   ├── components/
│   ├── pages/
│   └── services/
└── tests/

# [如果未使用请删除] 选项 3：移动 + API (当检测到“iOS/Android”时)
api/
└── [同上方的 backend]

ios/ 或 android/
└── [特定于平台的结构：功能模块、UI 流、平台测试]
```

**结构决策**: [记录所选结构并引用上方捕获的实际目录]

## 复杂性跟踪 (Complexity Tracking)

> **仅当宪法检查存在必须说明理由的违规项时才填写**

| 违规项 | 为什么需要 | 拒绝更简单的替代方案是因为 |
|-----------|------------|-------------------------------------|
| [例如：第 4 个项目] | [当前需求] | [为什么 3 个项目不够] |
| [例如：Repository 模式] | [特定问题] | [为什么直接访问数据库不够] |
