# cc-sdd 迁移指南

> 📖 **English version here:** [cc-sdd Migration Guide](migration-guide.md)

cc-sdd 1.x (特别是 1.1.5) 和 2.0.0 共享相同的 AI-DLC 理念和命令列表，但**设计产物、模板和引导 (steering) 结构已从头开始重建**。使用本指南在两条清晰的路径中做出选择：要么保持运行 1.1.5 现状，要么接受不连续性并迁移到 2.0.0，在 2.0.0 中模板/规则使自定义变得即时高效。

---

## TL;DR – 选择您的路径

| 目标 | 建议操作 |
| --- | --- |
| 保持旧版 1.x 工作流不变 | 在安装/刷新文件时运行 `npx cc-sdd@1.1.5`。继续编辑特定于 Agent 的提示词文件夹（仅存在原始的 8 个 spec/steering 命令）。 |
| 采用统一模板、研究/设计分离，并在所有 7 个受支持的 Agent 中保持行为一致 | 使用 `npx cc-sdd@latest` (=2.0.0) 重新安装，并仅自定义 `.kiro/settings/templates/*` 和 `.kiro/settings/rules/`（全套 11 个命令，包括 validate-*）。 |

> ⚠️ 不支持在同一个 `.kiro` 树中混合使用 1.x 和 2.x 布局。请为每个仓库/分支选择一条路径。

### 保持不变的内容

- 您已经编写的 `.kiro/specs/<feature>/` 目录仍然是有效的输入；只需在准备好时重新生成新模板即可。
- `.kiro/steering/`（或单个 `steering.md`）可以原样重复使用——内容仍作为项目记忆逐字消耗。
- 11 个 AI-DLC 命令 (`spec-*`, `validate-*`, `steering*`) 和高层级 spec→design→tasks→impl 流程保持一致；只有模板内部已转向即时 (just-in-time) 的 Agent 风格。

---

## 1. 留在 cc-sdd 1.1.5 (备选方案)

1.1.5 不再是 `@latest`，但您可以明确指定版本：

```bash
npx cc-sdd@1.1.5 --claude-code   # 旧版标志名称 (其他 Agent 请使用 --cursor / --gemini 等)
npx cc-sdd@1.1.5 --lang zh       # 旧版 i18n 标志仍然有效
```

- 您可以继续直接编辑 `.claude/commands/*`、`.cursor/prompts/*`、`.codex/prompts/*` 等特定于 Agent 的文件夹。
- 特定于 Agent 的目录布局与 v1 完全相同。
- 此版本不会增加新功能——未来的工作仅针对 `@latest`。
- 验证命令 (`/kiro:validate-gap`, `-design`, `-impl`) 在 1.1.5 中**不**存在。如果您依赖这些质量门禁，请迁移到 v2。

---

## 2. 为什么 2.0.0 值得升级

> AI-DLC 工作流 (spec-init → design → tasks → impl，以及验证门禁) 和 11 个命令入口点未变。改变的是**您在哪里进行自定义，以及生成的文档提供了多少结构。**

- **模板和规则驱动的自定义** – 停止修补命令；只需编辑一次 `.kiro/settings/templates/` 和 `.kiro/settings/rules/`，每个 Agent 都会生效。
- **规格保真度** – `research.md` 捕获调查日志，而 `design.md` 通过摘要表、需求覆盖度 (Req Coverage)、支持性引用 (Supporting References) 以及更轻量的组件/接口块变得对评审者更加友好。
- **引导 = 项目记忆** – 将结构化知识分布在 `.kiro/steering/*.md` 文件中，每个命令都会消耗它。
- **棕地项目护栏** – `/kiro:validate-gap`, `validate-design`, `validate-impl` 加上研究/设计分离，使得缺口分析和现有系统升级更加安全。
- **统一覆盖** – 所有 7 个受支持的 Agent (Claude Code, Claude Subagents, Cursor, Codex CLI, Gemini CLI, GitHub Copilot, Qwen Code, Windsurf) 运行相同的 11 命令工作流，因此混合使用 Agent（例如 Cursor + Claude）无需重写规格。

---

## 3. 建议的迁移步骤

1. **备份**
   ```bash
   cp -r .kiro .kiro.backup
   cp -r .claude .claude.backup   # 对 .cursor, .codex 等重复此操作
   ```

2. **全新安装 v2 (重用交互式选择)**
   ```bash
   npx cc-sdd@latest                 # 默认 (Claude Code)
   npx cc-sdd@latest --cursor        # 其他 Agent
   npx cc-sdd@latest --claude-agent  # Subagents 模式
   ```
   - 安装程序现在会针对每个文件组进行提示（覆盖 / 追加 / 保留）。您可以为 steering/specs 选择“追加 (append)”以合并现有文档，或选择“保留 (keep)”以跳过未改动的资源。

3. **重新生成 + 合并模板/规则**
   - 新布局：`.kiro/settings/templates/` (集中式) + `.kiro/settings/rules/`。
   - 将新模板与您之前在 Agent 提示词文件夹中保留的任何自定义逻辑进行比较，并将可重用的部分移至 templates/rules。

4. **迁移自定义规则**
   - 将 Markdown 文件放在 `.kiro/settings/rules/` 下。每个 spec/design/tasks 命令都会读取它们。
   - 您之前硬编码到提示词中的任何内容都应变为规则条目（“DO/DO NOT ...”）。

5. **重建引导文档 (可选)**
   - 将项目记忆拆分为诸如 `project-context.md`、`architecture.md`、`domain-knowledge.md` 等文件。
   - 研究/设计模板会引用此文件夹，因此请将现有笔记迁移至此。

6. **更新自动化**
   - 将所有脚本/文档指向 `npx cc-sdd@latest`；停止使用 `@next`。
   - 将旧的手动命令调用映射到 11 个支持的命令 (`spec-*`, `validate-*`, `steering*`)。

---

## 4. 将旧版编辑映射到 v2

| 旧版触点 | v2 替代方案 | 说明 |
| --- | --- | --- |
| `.claude/commands/spec-design.prompt.md` 等特定于 Agent 的命令文件 | `.kiro/settings/templates/specs/design.md` | 模板现在位于 `.kiro/settings/templates/` 中，并自动生成摘要/支持性引用。 |
| `.claude/commands/<cmd>.prompt`, `.cursor/prompts/*` | `.kiro/settings/rules/*.md` | 将提示词编辑替换为共享的规则陈述，以便每个 Agent 接收相同的指导。 |
| `.kiro/steering/` (单个文件或多个文件) | `.kiro/steering/*.md` 以及更清晰的原则/指南 | 相同的文件夹路径；v2 只是鼓励将内容拆分为专注的项目记忆指南。 |
| 交织在 design.md 中的研究笔记 | `.kiro/specs/<feature>/research.md` + 支持性引用部分 | 设计保持对评审者友好；研究保留原始发现而不使主体显得杂乱。 |

---

## 5. 常见问题 / 故障排除

**我可以在 v2 中重用旧模板吗？** – 技术上可以，但您会失去需求覆盖度和支持性引用，因此生成质量会下降。建议将内容移植到新的模板/规则中。

**我可以在一个仓库中切换 1.1.5 和 2.0.0 吗？** – 只有在按分支隔离 `.kiro` 或自动化交换目录时才可以；两种布局存在冲突。

**编辑模板后，我应该运行哪些命令？** – 至少运行：`/kiro:steering`, `/kiro:spec-init`, `/kiro:spec-design` 以使用新格式重新生成研究/设计/任务。

---

## 6. 核心要点

- **留在 1.1.5**：如果您只需要旧版工作流——请固定版本并照旧运行。
- **迁移到 2.0.0**：如果您想要统一的模板、支持性引用、研究/设计分离以及通过规则实现的最小化维护。
- 未来的功能和修复将针对 v2+，因此升级将解锁完整的规格驱动开发体验。
