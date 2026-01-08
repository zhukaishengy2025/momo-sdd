# cc-sdd: 为团队工作流打造的规格驱动开发 (Spec-driven development)

<!-- npm badges -->
[![npm version](https://img.shields.io/npm/v/cc-sdd?logo=npm)](https://www.npmjs.com/package/cc-sdd?activeTab=readme)
[![install size](https://packagephobia.com/badge?p=cc-sdd)](https://packagephobia.com/result?p=cc-sdd)
[![license: MIT](https://img.shields.io/badge/license-MIT-green.svg)](./LICENSE)

<div align="center" style="font-size: 1.1rem; margin-bottom: 1rem;"><sub>
<a href="./README.md">English</a> | <a href="./tools/cc-sdd/README_ja.md">日本語</a> | <a href="./tools/cc-sdd/README_zh-TW.md">繁體中文</a> | 简体中文
</sub></div>

## 将 AI 编程代理转变为生产级规格驱动开发流程

**一个命令。小时级而非周级交付。需求 → 设计 → 任务 → 实施。**

👻 **受 Kiro 启发** — 采用与 Kiro IDE 类似的规格驱动 (Spec-Driven) 和 AI-DLC 风格，确保现有的 Kiro 规格保持兼容与可移植。

告别 70% 浪费在会议、文档仪式和碎片化上下文中的开发时间。cc-sdd 为 Claude Code, Cursor, Gemini CLI, Codex CLI, GitHub Copilot, Qwen Code 和 Windsurf 带来了结构化的 **AI-DLC** (AI 驱动开发生命周期) 和 **规格驱动开发 (Spec-Driven Development)**。

### 你将获得：
- ✅ **规格优先保证** — 预先确认需求/设计，AI 严格按照规格实施
- ✅ **支持并行执行** — 任务被分解为具有依赖跟踪的并发实施项
- ✅ **团队对齐模板** — 一次自定义，所有代理生成的文档均符合团队审批流程
- ✅ **项目记忆 (Project Memory)** — AI 会跨会话记住你的架构、模式和标准
- ✅ **7 种代理，统一工作流** — 在 Claude, Cursor, Gemini, Codex, Copilot, Qwen, Windsurf 上使用相同的规格驱动流程
- ✅ **小时级交付** — 借助 AI 辅助规格，功能规划从几天缩短至几小时

## 🚀 快速开始

```bash
# 在你的项目根目录下运行
cd your-project
npx cc-sdd@latest --claude --lang zh ## Claude Code (简体中文)

# ✅ 就这么简单！现在运行：/kiro:spec-init <要构建的内容>
```

**安装仅需 30 秒。** 支持 7 种代理（Claude (Commands / Subagents), Cursor, Gemini, Codex, Copilot, Qwen, Windsurf）以及 13 种语言。

📖 **后续步骤：** [所有安装选项](#%EF%B8%8F-高级安装) | [命令参考](docs/guides/command-reference.md) | [规格驱动指南](docs/guides/spec-driven.md)

## 📋 实战演示

### 示例：构建新的“相册 (Photo Albums)”功能

```bash
/kiro:spec-init 具有上传、标签和共享功能的相册
/kiro:spec-requirements photo-albums-zh
/kiro:spec-design photo-albums-zh -y
/kiro:spec-tasks photo-albums-zh -y
```

**10 分钟内生成：**
- ✅ [requirements.md](.kiro/specs/photo-albums-zh/requirements.md) — 15 条 EARS 格式需求
- ✅ [design.md](.kiro/specs/photo-albums-zh/design.md) — 包含 Mermaid 图表的架构设计
- ✅ [tasks.md](.kiro/specs/photo-albums-zh/tasks.md) — 12 个带有依赖关系的实施任务

想要查看复杂的、大规模的需求集？请跳转到高级示例 [customer-support-rag-backend-en](.kiro/specs/customer-support-rag-backend-en/)，查看端到端的需求 → 设计 → 任务流程。

![示例：design.md 系统流程](assets/design-system_flow.png)

## 🎯 使用场景

| 场景 | 工作流 |
|----------|----------|
| **新功能开发 (Greenfield)** | `spec-init` → `spec-requirements` → `spec-design` → `spec-tasks` → `spec-impl` |
| **增强现有代码 (Brownfield)** | `steering` → `spec-init` → (`validate-gap` →) `spec-design` → (`validate-design` →) `spec-tasks` → `spec-impl` |
| **团队流程对齐** | 在 `.kiro/settings/templates/` 中自定义一次模板 → 所有代理遵循相同格式 |

## 🎨 自定义

在 `{{KIRO_DIR}}/settings/` 中自定义模板和规则，以匹配团队的工作流程：

- **templates/** - 定义文档结构（需求、设计、任务）
- **rules/** - 定义 AI 生成原则和评判标准

常见用例：PRD 风格需求、API/数据库架构、审批门禁、JIRA 集成、领域特定标准。

📖 **[自定义指南](docs/guides/customization-guide.md)** — 包含实际案例的完整指南

## ⚙️ 高级安装

### 选择你的代理

```bash
npx cc-sdd@latest --claude         # Claude Code (11 个命令) [默认]
npx cc-sdd@latest --claude-agent   # Claude Code 子代理 (12 个命令 + 9 个子代理)
npx cc-sdd@latest --cursor         # Cursor IDE
npx cc-sdd@latest --gemini         # Gemini CLI
npx cc-sdd@latest --codex          # Codex CLI
npx cc-sdd@latest --copilot        # GitHub Copilot
npx cc-sdd@latest --qwen           # Qwen Code
npx cc-sdd@latest --windsurf       # Windsurf IDE
```

### 选择你的语言

```bash
npx cc-sdd@latest --lang zh        # 简体中文
npx cc-sdd@latest --lang zh-TW     # 繁体中文
npx cc-sdd@latest --lang ja        # 日语
npx cc-sdd@latest --lang es        # 西班牙语
# 支持：en, ja, zh-TW, zh, es, pt, de, fr, ru, it, ko, ar, el
```

### 高级选项

```bash
# 在应用更改前预览
npx cc-sdd@latest --dry-run

# 自定义规格目录
npx cc-sdd@latest --kiro-dir docs
```

---

## 📚 文档与支持

### 📖 完整指南 (英语 | 日本語)

| 指南 | 你将学到什么 | 链接 |
|-------|-------------------|-------|
| **命令参考** | 所有 11 个 `/kiro:*` 命令的详细用法、参数和示例 | [English](docs/guides/command-reference.md) \| [日本語](docs/guides/ja/command-reference.md) |
| **自定义指南** | 7 个实际示例：PRD 需求、前/后端设计、JIRA 集成 | [English](docs/guides/customization-guide.md) \| [日本語](docs/guides/ja/customization-guide.md) |
| **规格驱动指南** | 从需求到实施的完整工作流方法论 | [English](docs/guides/spec-driven.md) \| [日本語](docs/guides/ja/spec-driven.md) |
| **Claude 子代理** | 高级：针对复杂项目使用 9 个专门的子代理 | [English](docs/guides/claude-subagents.md) \| [日本語](docs/guides/ja/claude-subagents.md) |
| **迁移指南** | 从 v1.x 升级到 v2.0.0 | [English](docs/guides/migration-guide.md) \| [日本語](docs/guides/ja/migration-guide.md) |

### 软件包文档
- 英语: [tools/cc-sdd/README.md](tools/cc-sdd/README.md)
- 日本語: [tools/cc-sdd/README_ja.md](tools/cc-sdd/README_ja.md)
- 繁體中文: [tools/cc-sdd/README_zh-TW.md](tools/cc-sdd/README_zh-TW.md)

---

## 📚 相关资源

📝 **文章与演讲**
- [在 Claude Code 中彻底再现 Kiro 的规格驱动开发流程](https://zenn.dev/gotalab/articles/3db0621ce3d6d2) - Zenn 文章 (日语)
- [Claude Code 不做规格驱动的梦](https://speakerdeck.com/gotalab555/claude-codehashi-yang-qu-dong-nomeng-wojian-nai) - Speaker Deck 演讲稿 (日语)

🔗 **外部资源**
- [Kiro IDE](https://kiro.dev) - 增强的规格管理和团队协作
- [Kiro 的规格方法论](https://kiro.dev/docs/specs/) - 经过验证的规格驱动开发方法论
- [AI 辅助 SDD：使用 Gemini, Claude 和 cc-sdd 进行规格驱动开发](https://www.amazon.com/dp/B0CW19YX9R) - 亚马逊上的完整书籍

## 📦 软件包信息

此存储库包含位于 [`tools/cc-sdd/`](tools/cc-sdd/) 的 **cc-sdd** NPM 软件包。

有关详细文档、安装说明和使用示例，请参阅：
- [**工具文档**](tools/cc-sdd/README.md) - 完整的 cc-sdd 工具指南
- [**日语文档**](tools/cc-sdd/README_ja.md) - 日本語版ツール説明


## 许可证

MIT License
