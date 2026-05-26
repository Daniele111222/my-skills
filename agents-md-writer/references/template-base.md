# Base Template (all project types)

Universal sections every `AGENTS.md` should include. Adapt phrasing to the project's actual situation — don't paste verbatim.

---

## Section: Header

```markdown
# {{ProjectName}} — AI 开发规范

**技术栈**: {{primary frameworks + versions}}
**项目类型**: {{archetype}}
**目标读者**: AI 编程助手（Claude Code / Cursor / Copilot 等）为主，人类开发者次之

---
```

## Section: 核心开发理念

3-5 capability buckets. Each bucket is a heading + 2-4 bullets. Pick buckets that match the archetype:

**Universal candidates**:
- 声明式 / 模块化思维（frontend, fullstack）
- 防御性编程与可测试性（all）
- 类型安全与契约优先（TypeScript / Rust / Go projects）
- 接口契约与领域建模（backend, fullstack）
- 性能与资源边界（mobile, embedded, perf-sensitive）

**Format**:
```markdown
## 🎯 核心开发理念

### 1. {{Bucket name}}
- **{{Sub-principle}}**：{{one-sentence enforcement clause}}
- **{{Sub-principle}}**：{{one-sentence enforcement clause}}
```

Avoid abstract aspirations ("写优雅的代码"). Each bullet should be checkable.

---

## Section: 项目背景与最高优先级原则（红线）

This is the **most important section**. AI reads it first.

```markdown
## ⚠️ 项目背景与最高优先级原则

### 项目现状
- {{2-4 bullets describing the actual situation: team size, history, iteration speed, runtime environments}}

### 最高优先级原则（红线）

#### 1. 技术栈强制对齐
- **必须**使用项目已配置的技术栈：{{list}}
- **禁止**引入新的 {{category}} 库（如 {{counter-examples}}）
- **禁止**使用未在 {{config file}} 中配置的工具/插件

#### 2. 历史代码处理原则
- {{保持不动 / 增量改进 / 全量重写 — pick policy that matches team}}
- {{whether legacy utilities are reusable or to be replaced}}

#### 3. 类型与数据规范（强制）
- {{language-specific type rules — see archetype templates}}
- {{ID / time / money handling rules — these are nearly universal}}

#### 4. {{Service Layer / API Layer / Data Layer}} 规范（强制）
- {{where this lives, what wrapper to use, where types go}}

#### 5. 文档同步规范（强制）
- 修改公共组件 / 公共方法 / 新建公共文件夹时，需询问是否同步更新 AGENTS.md
- 新规则发现时，AI 应主动提示用户是否纳入 AGENTS.md
```

The 5 categories above are a checklist. If a category genuinely doesn't apply, drop it. Don't add empty placeholders.

---

## Section: 技术栈与项目结构

### 核心技术栈表格

```markdown
| 类别 | 技术选型 | 版本 | 说明 |
|------|---------|------|------|
| 框架 | {{name}} | {{version}} | {{1-clause note}} |
| 语言 | {{name}} | {{version}} | {{1-clause note}} |
| 构建 | {{name}} | {{version}} | {{1-clause note}} |
| {{UI lib / ORM / etc}} | {{name}} | {{version}} | {{1-clause note}} |
| 测试 | {{name}} | {{version}} | {{1-clause note}} |
```

Only list categories the project actually uses. Empty rows ("- 无") are noise.

### 目录结构

```markdown
src/
├── {{folder}}/      # {{purpose}}
├── {{folder}}/      # {{purpose}}
└── {{folder}}/      # {{purpose}}
```

Keep to top 2 levels. Drill into a third level only if it carries a rule (e.g. "页面级 hooks 必须放在 `src/pages/{Page}/hooks/`").

---

## Section: 检查与验证机制

### 代码提交前检查清单

```markdown
- [ ] **类型检查**：{{tsc / mypy / cargo check}} 无错误
- [ ] **代码风格**：通过 {{linter}} 与 {{formatter}}
- [ ] **测试**：{{unit / integration}} 测试通过
- [ ] **构建**：{{build command}} 成功
- [ ] **{{domain-specific check}}**：{{e.g. 接口契约、迁移兼容、移动端预览}}
```

### 代码审查关注点

3 angles, each with 3-4 questions:

**架构层面**
- {{archetype-specific architectural questions}}

**性能层面**
- {{archetype-specific perf questions}}

**{{体验 / 安全 / 可观测性 — pick the most relevant third angle}}**
- {{questions}}

---

## Section: 公共工具与约定

List shared helpers the team wants AI to **reuse rather than reinvent**. One bullet per helper:

```markdown
- **{{功能名}}**：统一使用 `{{path/to/util.ts}}` 中的 `{{functionName}}`；{{ when to use + any constraints}}
```

Only include utils that were observed in the scan or confirmed in the interview. Don't invent.
