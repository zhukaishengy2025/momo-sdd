# cc-sdd: 一键让 AI 编程代理进入生产级规格开发

[![npm version](https://img.shields.io/npm/v/cc-sdd?logo=npm)](https://www.npmjs.com/package/cc-sdd?activeTab=readme)
[![install size](https://packagephobia.com/badge?p=cc-sdd)](https://packagephobia.com/result?p=cc-sdd)
[![license: MIT](https://img.shields.io/badge/license-MIT-green.svg)](../../LICENSE)

<div align="center" style="margin-bottom: 1rem; font-size: 1.1rem;"><sub>
<a href="./README.md">English</a> | <a href="./README_ja.md">日本語</a> | <a href="./README_zh-TW.md">繁體中文</a> | 简体中文
</sub></div>

✨ **将 Claude Code / Cursor IDE / Gemini CLI / Codex CLI / GitHub Copilot / Qwen Code / Windsurf 直接带入 Spec-Driven / AI-DLC 的生产级流程，需求、设计、任务、指导文件一次对齐团队审核。**

👻 **Kiro 兼容** — 与 Kiro IDE 相似的 Spec-Driven / AI-DLC 风格，可沿用既有 Kiro 规格并保持互通。

**v2.0.0 新功能：**
- ✅ **易于审查的设计** — 结构化格式与摘要表让审查速度提升 5 倍
- ✅ **分离研究** — 将探索笔记（Research.md）与最终设计（Design.md）分开管理
- ✅ **质量关卡** — validate-gap/design/impl 命令在编码前捕捉集成问题
- ✅ **一次自定义** — 将模板适应至团队流程；所有代理遵循相同工作流程
- ✅ **统一工作流程** — 7 代理 × 13 语言共享相同的 11 命令流程

> 只想看安装？跳到 [安装](#-安装)。若要维持 1.1.5，使用 `npx cc-sdd@1.1.5 --claude-code ...`；升级 v2.0.0 请参考 [Migration Guide](../../docs/guides/migration-guide.md) ｜ [日文版](../../docs/guides/ja/migration-guide.md)。

## 🚀 安装

只需一个命令，即可为主要 AI 编程代理导入 **AI-DLC（AI Driven Development Life Cycle）× SDD（Spec-Driven Development）** 工作流程。需求、设计、任务、指导文件也会同步生成，并对齐团队既有批准流程。

```bash
# 基本安装（默认：英文文件，Claude Code 代理）
npx cc-sdd@latest

# 语言选项（默认：--lang en）
npx cc-sdd@latest --lang zh    # 简体中文
npx cc-sdd@latest --lang zh-TW # 繁体中文
npx cc-sdd@latest --lang ja    # 日语
npx cc-sdd@latest --lang es    # 西班牙语
...（支持语言：en, ja, zh-TW, zh, es, pt, de, fr, ru, it, ko, ar, el）

# 代理选项（默认：claude-code / --claude）
npx cc-sdd@latest --claude --lang zh           # Claude Code（11 个命令，语言可任选）
npx cc-sdd@latest --claude-agent --lang zh     # Claude Code Subagents（12 个命令 + 9 个子代理）
npx cc-sdd@latest --cursor --lang zh           # Cursor IDE
npx cc-sdd@latest --gemini --lang zh           # Gemini CLI
npx cc-sdd@latest --codex --lang zh            # Codex CLI
npx cc-sdd@latest --copilot --lang zh          # GitHub Copilot
npx cc-sdd@latest --qwen --lang zh             # Qwen Code
npx cc-sdd@latest --windsurf --lang zh         # Windsurf IDE

# 注意：@next 现已保留给未来的 alpha/beta 版本
```

## 🌐 支持语言

| 语言 | 代码 |  |
|------|------|------|
| 英语 | `en` | 🇬🇧 |
| 日语 | `ja` | 🇯🇵 |
| 繁体中文 | `zh-TW` | 🇹🇼 |
| 简体中文 | `zh` | 🇨🇳 |
| 西班牙语 | `es` | 🇪🇸 |
| 葡萄牙语 | `pt` | 🇵🇹 |
| 德语 | `de` | 🇩🇪 |
| 法语 | `fr` | 🇫🇷 |
| 俄语 | `ru` | 🇷🇺 |
| 意大利语 | `it` | 🇮🇹 |
| 韩语 | `ko` | 🇰🇷 |
| 阿拉伯语 | `ar` | 🇸🇦 |
| 希腊语 | `el` | 🇬🇷 |

**使用方法**: `npx cc-sdd@latest --lang <代码>` (例如简体中文使用 `--lang zh`)

## ✨ 快速开始

### 新项目
```bash
# 启动 AI 代理并立即开始规格驱动开发
/kiro:spec-init 使用 OAuth 构建用户认证系统  # AI 建立结构化计划
/kiro:spec-requirements auth-system                 # AI 提出澄清问题
/kiro:spec-design auth-system                      # 人类验证，AI 设计
/kiro:spec-tasks auth-system                       # 分解为实现任务
/kiro:spec-impl auth-system                        # 以 TDD 执行
```

![design.md - System Flow Diagram](https://raw.githubusercontent.com/gotalab/cc-sdd/refs/heads/main/assets/design-system_flow.png)
*Example of system flow during the design phase `design.md`*

### 现有项目（建议）
```bash
# 首先建立项目上下文，然后进行开发
/kiro:steering                                     # AI 学习现有项目上下文

/kiro:spec-init 为现有认证新增 OAuth            # AI 建立强化计划
/kiro:spec-requirements oauth-enhancement          # AI 提出澄清问题
/kiro:validate-gap oauth-enhancement               # 可选：分析现有 vs 需求
/kiro:spec-design oauth-enhancement                # 人类验证，AI 设计
/kiro:validate-design oauth-enhancement            # 可选：验证设计集成
/kiro:spec-tasks oauth-enhancement                 # 分解为实现任务
/kiro:spec-impl oauth-enhancement                  # 以 TDD 执行
```

**30 秒设定** → **AI 驱动“快速冲刺”（非冲刺）** → **小时交付结果**

### 为何团队选择 cc-sdd
1. **规格是单一真实来源** — 需求、设计、任务、Supporting References 同步产出，审查更快。
2. **Greenfield / Brownfield 皆适用** — 新功能快速起步；既有系统靠 validate 系列与 Project Memory 保持安全。
3. **可同时使用多个代理** — Claude、Cursor、Codex、Gemini、Copilot、Qwen、Windsurf 共用同一套模板/规则。
4. **自定义只要一次** — 编辑 `.kiro/settings/templates/` 或 `.kiro/settings/rules/`，所有代理立即套用。

## ✨ 主要功能

- **🚀 AI-DLC 方法论** - 具人类批准的 AI 原生流程。核心模式：AI 执行，人类验证
- **📋 规格优先开发** - 全面性规格作为单一信息源驱动整个生命周期
- **⚡ “快速冲刺”非冲刺** - [AI-DLC 术语](https://aws.amazon.com/jp/blogs/news/ai-driven-development-life-cycle/)，强度小时/天周期取代数周冲刺。脱离 70% 管理额外负担
- **🧠 持久项目记忆** - AI 通过指导文件在所有会话间维持全面上下文（架构、模式、规则、领域知识）
- **🛠 模板弹性** - 自定义 `{{KIRO_DIR}}/settings/templates`（steering / requirements / design / tasks），符合团队习惯的文件格式
- **🔄 AI 原生+人类关卡** - AI 计划 → AI 提问 → 人类验证 → AI 实现（具质量控制的快速循环）
- **🌍 团队就绪** - 具质量关卡的 13 语言跨平台标准化工作流程

## 🤖 支持的 AI 代理

| 代理 | 状态 | 命令 | 设定 |
|------|------|------|------|
| **Claude Code** | ✅ 完全支持 | 11 个斜杠命令 | `CLAUDE.md` |
| **Claude Code Subagents** | ✅ 完全支持 | 12 个命令 + 9 个子代理 | `CLAUDE.md`, `.claude/agents/kiro/` |
| **Cursor IDE** | ✅ 完全支持 | 11 个命令 | `AGENTS.md` |
| **Gemini CLI** | ✅ 完全支持 | 11 个命令 | `GEMINI.md` |
| **Codex CLI** | ✅ 完全支持 | 11 个提示 | `AGENTS.md` |
| **GitHub Copilot** | ✅ 完全支持 | 11 个提示 | `AGENTS.md` |
| **Qwen Code** | ✅ 完全支持 | 11 个命令 | `QWEN.md` |
| **Windsurf IDE** | ✅ 完全支持 | 11 个工作流 | `AGENTS.md` |
| 其他（Factory AI Droid） | 📅 规划中 | - | - |

## 📋 命令

### 规格驱动开发工作流（Specs 方法论）
```bash
/kiro:spec-init <description>             # 初始化功能规格
/kiro:spec-requirements <feature_name>    # 产生需求
/kiro:spec-design <feature_name>          # 建立技术设计
/kiro:spec-tasks <feature_name>           # 分解为实现任务
/kiro:spec-impl <feature_name> <tasks>    # 以 TDD 执行
/kiro:spec-status <feature_name>          # 检查进度
```

> **规格作为基础**：基于 [Kiro 的规格驱动方法论](https://kiro.dev/docs/specs/) - 规格将随意开发转换为系统工作流，在明确的 AI-人类协作点将想法与实现连接。

> **Kiro IDE 集成**：规格可移植到 [Kiro IDE](https://kiro.dev) - 提供增强的实现保护栏和团队协作功能。

### 质量验证（可选 - 棕地开发）
```bash
# spec-design 之前（分析现有功能 vs 需求）：
/kiro:validate-gap <feature_name>         # 分析现有功能与需求间的差距

# spec-design 之后（验证设计与现有系统）：
/kiro:validate-design <feature_name>      # 审查设计与现有架构的兼容性
```

> **棕地开发可选**：`validate-gap` 分析现有 vs 所需功能；`validate-design` 检查集成兼容性。两者都是现有系统的可选质量关卡。

### 项目记忆与上下文（必要）
```bash
/kiro:steering                            # 建立/更新项目记忆与上下文
/kiro:steering-custom                     # 新增专门领域知识
```

> **关键基础命令**：指导建立持久项目记忆 - AI 在所有会话中使用的上下文、规则和架构。**现有项目先执行**以大幅提升规格质量。

## 🎨 自定义

可编辑 `{{KIRO_DIR}}/settings/templates/` 中的模板以符合工作流。保留核心结构（需求编号、复选框、标题）并新增团队的上下文—AI 会自动适应。

**常见自定义**:
- **PRD 风格需求** - 包含业务上下文与成功指标
- **前端/后端设计** - 针对 React 组件或 API 规格优化
- **批准关卡** - 用于安全、架构或合规审查
- **JIRA/Linear 就绪任务** - 含估算、优先级、标签
- **领域指导** - API 标准、测试惯例、编码指南

📖 **[自定义指南](https://github.com/gotalab/cc-sdd/blob/main/docs/guides/customization-guide.md)** — 7 个实用范例与可复制代码片段


## ⚙️ 设定

```bash
# 语言与平台
npx cc-sdd@latest --lang zh                # macOS / Linux / Windows（自动检测）
npx cc-sdd@latest --lang zh --os mac       # 保留的可选覆盖

# 安全操作
npx cc-sdd@latest --dry-run --backup

# 自定义目录
npx cc-sdd@latest --kiro-dir docs
```

## 📁 项目结构

安装后，项目将新增：

```
project/
├── .claude/commands/kiro/    # 11 个斜杠命令
├── .codex/prompts/           # 11 个提示命令（Codex CLI）
├── .github/prompts/          # 11 个提示命令（GitHub Copilot）
├── .windsurf/workflows/      # 11 个工作流文件（Windsurf IDE）
├── .kiro/settings/           # 共享规则与模板（以 {{KIRO_DIR}} 展开）
├── .kiro/specs/             # 功能规格文件
├── .kiro/steering/          # AI 指导规则
└── CLAUDE.md (Claude Code)    # 项目设定
```

> 提醒：实际只会建立所选代理需要的目录，上方树状图仅示范整个超集合。

## 📚 文档与支持

- 命令参考: [English](../../docs/guides/command-reference.md) | [日本語](../../docs/guides/ja/command-reference.md)
- 自定义指南: [English](../../docs/guides/customization-guide.md) | [日本語](../../docs/guides/ja/customization-guide.md)
- 规格驱动开发指南: [English](../../docs/guides/spec-driven.md) | [日本語](../../docs/guides/ja/spec-driven.md)
- Claude 子代理指南: [English](../../docs/guides/claude-subagents.md) | [日本語](../../docs/guides/ja/claude-subagents.md)
- 迁移指南: [English](../../docs/guides/migration-guide.md) | [日本語](../../docs/guides/ja/migration-guide.md)
- **[问题与支持](https://github.com/gotalab/cc-sdd/issues)** - 问题报告与提问
- **[Kiro IDE](https://kiro.dev)**

---

**稳定版 v2.0.0** - 生产环境就绪。[报告问题](https://github.com/gotalab/cc-sdd/issues) | MIT License

### 平台支持
- 支持 OS：macOS / Linux / Windows（默认自动检测）。
- 三大平台共用命令模板；`--os` 参数保留给兼容性需求，可视情况指定。

> **提醒:** 即使指定 `--os` 仍会成功执行，但所有平台现在会收到相同的命令模板。
