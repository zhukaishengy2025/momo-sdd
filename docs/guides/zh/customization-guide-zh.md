# 自定义指南 (Customization Guide)

> 📖 **繁體中文版:** [自定義指南](ja/customization-guide.md) | 📖 **日本語版:** [カスタマイゼーションガイド](ja/customization-guide.md)

本指南介绍了如何编辑 cc-sdd 的模板和规则，以使其适应您团队特定的工作流程。

## 简介

cc-sdd 提供了两个自定义点：

- **templates/** - 定义 AI 生成文档的 **结构和格式**
- **rules/** - 定义 AI 的 **判断标准和生成原则**

两者都位于 `{{KIRO_DIR}}/settings/` 目录下，并在整个项目中共享。

---

## 两种自定义方法

### 📄 templates/ - 自定义输出格式

**位置**: `{{KIRO_DIR}}/settings/templates/specs/`

**作用**: 定义 AI 生成的 **文档结构**。在模板中添加的章节和字段将由 AI 自动填充。

**可编辑文件**:
- `requirements.md` - 需求文档结构
- `design.md` - 设计文档结构
- `tasks.md` - 任务拆解结构

**自定义示例**:
- 添加 PRD 风格的章节（产品概述、成功指标等）
- 添加审批检查列表
- 添加 JIRA 字段

---

### 📋 rules/ - 自定义 AI 判断标准

**位置**: `{{KIRO_DIR}}/settings/rules/`

**作用**: 定义 AI 的 **生成规则和原则**。编辑规则会改变 AI 的判断标准和生成风格。

**可编辑文件**:
- `ears-format.md` - EARS 格式需求描述规则
- `design-principles.md` - 设计原则和文档标准
- `tasks-generation.md` - 任务拆解粒度和结构规则
- `tasks-parallel-analysis.md` - 确定并行可执行性的标准
- 其他 (`design-discovery-*.md`, `gap-analysis.md` 等)

**自定义示例**:
- 调整任务粒度（1-3 小时 → 4-8 小时等）
- 添加设计原则（安全性、性能要求等）
- 需求优先级判定标准

---

## 🚨 必须维护的结构

cc-sdd 命令通过 AI 代理读取和理解文档。以下元素 **必须保留**：

| 文件 | 必须元素 | 原因 |
|------|-------------------|--------|
| **requirements.md** | 编号标准 (`1.`, `2.`, `3.`...) | AI 识别标准的编号和结构 |
| | 与模板的一致性 | AI 从模板中学习结构 |
| **design.md** | **文件存在** | 命令需要读取此文件 |
| **tasks.md** | `- [ ] N.` 复选框格式 | 任务执行引擎识别此格式 |
| | `_Requirements: X, Y_` 引用 | 需求可追溯性 |
| | 层级结构 (1, 1.1, 1.2...) | 依赖分析 |

**重要提示**: `requirements.md` 中的标题可以自由更改。AI 会学习模板中定义的结构模式，并使用相同的模式进行生成。

### ✅ requirements.md 的灵活性（重要）

`requirements.md` 的自定义非常灵活：

#### 1. 标题名称自定义

**标题名称可以自由更改**。AI 从模板中学习结构：

- ✅ **英文**: `### Requirement 1:` / `#### Acceptance Criteria`
- ✅ **本地化**: `### 需求 1:` / `#### 验收标准`（支持任何语言）
- ✅ **自定义**: `### REQ-1:` / `#### 验证标准`

**关键点**:
- 保持编号模式（`N:` 其中 N 是数字）
- 保持层级结构（`###` 和 `####`）
- 保持模板与生成文件之间的一致性

#### 2. 验收标准描述格式

**推荐使用 EARS 格式，但不是强制性的**:

- ✅ **推荐 EARS 格式**: `WHEN [事件] THEN [系统] SHALL [动作]` - AI 生成的默认格式
- ✅ **其他格式也是可以接受的**:
  - 简单格式: `系统响应 XX`
  - BDD 格式: `GIVEN [上下文] WHEN [事件] THEN [结果]`
  - 自定义格式: 您团队自己的模板
- ✅ **编号很重要**: 只要保持 `1.`, `2.`, `3.` 的格式，内容是自由的

**EARS 格式的优势**:
- 高可测试性（明确的条件和预期结果）
- 易于 AI 理解（提高设计/任务生成的准确性）
- 行业标准（易于评审者阅读）

**只有结构是强制性的**: AI 学习结构模式，但不解析特定的字符串。

---

## 自定义流程（3 个步骤）

### 步骤 1: 检查默认模板

```bash
# 检查模板位置
ls -la {{KIRO_DIR}}/settings/templates/specs/
ls -la {{KIRO_DIR}}/settings/rules/
```

### 步骤 2: 在维护结构的同时进行添加/编辑

- **templates/**: 添加章节和字段
- **rules/**: 添加原则和标准

### 步骤 3: 通过测试执行进行验证

```bash
# 使用新规格进行测试
/kiro:spec-init 测试自定义功能
/kiro:spec-requirements test-customization
/kiro:spec-design test-customization
/kiro:spec-tasks test-customization

# 检查生成的文件
cat {{KIRO_DIR}}/specs/test-customization/requirements.md
cat {{KIRO_DIR}}/specs/test-customization/design.md
cat {{KIRO_DIR}}/specs/test-customization/tasks.md
```

---

## 实践场景

我们提供了 3 个针对团队特定需求量身定制的代表性自定义场景。每个场景都包括完整的可复制粘贴代码和测试方法。

### 场景 1: PRD 风格的需求

#### 📋 自定义目标
- **templates**: `{{KIRO_DIR}}/settings/templates/specs/requirements.md`
- **rules**: `{{KIRO_DIR}}/settings/rules/ears-format.md`（可选）

#### 🎯 使用案例
- 产品/业务团队作为利益相关者参与
- 业务背景、优先级和成功指标在需求评审中是强制性的
- 有许多非工程师评审员

---

### 场景 2: 以后端/API 为中心的设计文档

#### 📋 自定义目标
- **templates**: `{{KIRO_DIR}}/settings/templates/specs/design.md`
- **rules**: `{{KIRO_DIR}}/settings/rules/design-principles.md`（可选）

#### 🎯 使用案例
- REST/GraphQL API 开发
- 微服务架构
- 数据库设计和 Schema 定义非常重要

---

## 最佳实践

### ✅ 推荐做法

- **渐进式自定义**: 一次更改并测试一个文件
- **维护必要结构**: 保留编号模式和层级结构
- **版本控制**: 使用 git 管理 `{{KIRO_DIR}}/settings/`
- **强规则**: 使用 "MUST"（必须）+ 3 个以上具体示例

### ❌ 不推荐做法

- 删除必要结构（编号、复选框）
- 模糊的规则（使用 "should", "consider" 而非 "MUST"）
- 模板超过 1000 行
- 未经测试就提交

---

## 下一步

### 1. 确定自定义优先级

**推荐顺序**:
1. **requirements.md** - 需求定义是万物之基
2. **design.md** - 评审频率最高的设计文档
3. **tasks.md** - 实现阶段使用最多
4. **steering/** - 领域知识的积累

### 2. 试点运行

```bash
# 1. 尝试一个小功能
/kiro:spec-init 用于测试自定义模板的小功能
/kiro:spec-requirements test-feature
/kiro:spec-design test-feature
/kiro:spec-tasks test-feature

# 2. 团队评审
# - 检查输出质量
# - 识别缺失信息
# - 调整模板

# 3. 开始正式运行
# - 通知整个团队
# - 更新入职材料
```

---

## 相关文档

- [规格驱动开发工作流](spec-driven.md)
- [命令参考](command-reference.md)
- [迁移指南](migration-guide.md)
