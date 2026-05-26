---
name: agents-md-writer
description: Generate a project-specific AGENTS.md (also known as CLAUDE.md / development guidelines) that captures tech stack, conventions, red-line rules, directory layout, and review checklists for an existing codebase. Use this skill whenever the user asks to write, draft, generate, refresh, or maintain an AGENTS.md / CLAUDE.md / 项目开发规范 / AI 协作规范 / 编码规范 document — even if they only say things like "为这个项目写一份给 AI 看的规范", "整理一下我们项目的开发约束", "生成 Cursor / Claude Code 用的项目指引", or "把这些约定写进 AGENTS.md". The skill works for any project type (frontend web, H5/mobile web, backend service, fullstack, CLI tool, library, mobile native), and produces output optimized for AI coding assistants to follow as hard constraints. Trigger it proactively when the user describes team conventions, red-line rules, or pain points and seems to be heading toward documenting them.
---

# AGENTS.md Writer

This skill produces a project-tailored `AGENTS.md` — a document AI coding assistants read first, written as enforceable constraints rather than tutorial prose. The reference example this skill was built from is an H5 React project, but the structure generalizes to backend services, CLIs, libraries, and native apps.

The output you produce should feel like a contract the AI must honor, not a friendly onboarding doc. That stylistic stance shapes every section below.

---

## Workflow

Follow these phases in order. Don't skip the scan — guessing at conventions produces generic docs that the user will reject.

### Phase 1 — Scan the project

Read the actual code before asking questions. The user picked "auto-scan + clarify gaps" — respect that by coming to the interview prepared.

Inspect (in parallel where possible):

- **Manifest files** — `package.json`, `pnpm-workspace.yaml`, `pyproject.toml`, `Cargo.toml`, `go.mod`, `pom.xml`, `build.gradle`, `Gemfile`, `*.csproj`. Read dependencies + scripts + workspaces.
- **Config files** — `tsconfig.json`, `vite.config.*`, `next.config.*`, `webpack.config.*`, `tailwind.config.*`, `.eslintrc*`, `.prettierrc*`, `postcss.config.*`, `babel.config.*`, `jest.config.*`, `vitest.config.*`, `playwright.config.*`, `Dockerfile`, `docker-compose.yml`, `.env.example`.
- **Top-level directory structure** — `Glob` for `src/**`, `app/**`, `pages/**`, `components/**`, `services/**`, `service/**`, `hooks/**`, `utils/**`, `lib/**`, `api/**`, `controllers/**`, `routes/**`, `models/**`, `migrations/**`, `tests/**`, `__tests__/**`, `e2e/**`, `docs/**`. Note: don't dump full trees into context — sample 1-2 representative subfolders.
- **Existing docs** — `README.md`, `CONTRIBUTING.md`, `CLAUDE.md`, `AGENTS.md`, `docs/*.md`. If `AGENTS.md` already exists, treat this as a refresh and preserve the user's intent.
- **Representative source files** — pick 1 page/component and 1 service/api file to confirm the actual code style (CSS Module vs global CSS, naming, type usage, request wrapper).

Output of this phase: an internal mental model of project type, tech stack, layering, naming conventions, and any obvious style choices the user is already enforcing.

### Phase 2 — Classify the project

Pick the closest archetype — this drives which reference templates you load:

| Archetype | Signals |
|---|---|
| `frontend-web` | React/Vue/Svelte/Angular + Vite/Next/Nuxt + browser target |
| `frontend-h5` | `frontend-web` + `postcss-px-to-viewport` / vant / react-vant / `viewport` meta + mobile UA hints |
| `backend-service` | Express/Nest/Fastify/Koa/FastAPI/Django/Flask/Spring/Gin/Echo/Rails/.NET — has controllers or routers, no UI |
| `fullstack` | Next.js app router + API routes, Remix, Nuxt server routes, or monorepo with both |
| `mobile-native` | React Native, Flutter, SwiftUI, Kotlin Android, Expo |
| `cli-or-library` | Single entry binary or published npm/pypi package — no app shell |
| `monorepo` | Workspaces present — apply per-package archetypes |

If the project genuinely spans two archetypes (e.g. fullstack), compose sections from both reference files rather than forcing one label.

After classification, read the matching reference file(s) from `references/`:

- `references/template-base.md` — always read; contains universal sections.
- `references/template-frontend-web.md` — frontend web specifics.
- `references/template-frontend-h5.md` — H5 mobile additions.
- `references/template-backend.md` — service/API layer specifics.
- `references/template-mobile-native.md` — native app specifics.
- `references/template-cli-library.md` — packaged tool specifics.
- `references/writing-style.md` — read this every time; it governs tone and structure.

### Phase 3 — Interview to fill gaps

After scanning you'll know the *what* (tech stack, layout) but not the *why* (team conventions, pain points, red-lines). Ask targeted questions — never blanket-questionnaire the user.

Use `AskUserQuestion` to batch related questions (max 4 at a time, ideally 2-3). Group by theme so the user can answer in one pass.

Topics to cover (only ask what scanning didn't already answer):

1. **Project background** — what does it do, who's the team, what's the rough quality / collaboration situation? (e.g. multi-developer, mixed history, frequent iteration).
2. **Red-line rules** — what must AI *never* do? Common categories:
   - Tech stack lock-in (no new UI libraries, no new state libraries)
   - How to treat legacy code (leave alone? rewrite to new spec? incremental?)
   - Type/data hard rules (e.g. no `any`, big-integer IDs must stay string, time fields must use central formatter)
   - Service/API layer rules (where requests live, which wrapper to use)
   - Public-component sync rules (do edits to shared components require doc updates?)
3. **Directory & naming conventions** — anything non-obvious that scanning missed (e.g. `src/service/` vs `src/services/`, business-domain folder grouping, page-level vs global hooks).
4. **Style / UI conventions** *(frontend only)* — CSS Module vs global, design tokens, theming, responsive strategy.
5. **Shared utilities to call out** — date formatters, text truncation, ID handling, request wrapper, error reporter — anything the team wants AI to reuse instead of reinventing.
6. **Review checklist priorities** — what does the team look at first in code review? (Types, perf, error handling, accessibility, security…)

Skip topics that don't apply. A library project doesn't need NavHeader rules; a backend doesn't need viewport adaptation.

### Phase 4 — Draft the AGENTS.md

Compose the document by selecting and adapting sections from the reference templates. The skeleton below is the recommended order — drop sections that don't apply, don't invent ones that the project doesn't need.

```
# {{Project Name}} — AI 开发规范 / AGENTS.md

**技术栈**: {{tech list}}
**项目类型**: {{archetype}}

## 🎯 核心开发理念
  (3-5 capability buckets tailored to archetype — see writing-style.md)

## ⚠️ 项目背景与最高优先级原则
  ### 项目现状
  ### 最高优先级原则（红线）
    1. 技术栈强制对齐
    2. 历史代码处理原则
    3. 类型与数据规范（强制）
    4. 服务/数据层规范（强制）
    5. 文档同步规范（强制）

## 🛠 技术栈与项目结构
  ### 核心技术栈（表格）
  ### 目录结构（树）

## 📋 {{frontend: 组件与样式规范 | backend: 接口与领域规范 | etc.}}

## ✅ 检查与验证机制
  ### 代码提交前检查清单
  ### 代码审查关注点（架构/性能/体验 或 架构/性能/安全）

## 🔧 公共工具与约定
  (project-specific shared utilities)
```

Follow `references/writing-style.md` strictly while drafting — it covers tone, the imperative voice, when to use 强制/禁止 vs 优先/推荐, and how to format examples.

**Critical writing rules** (also in writing-style.md, but worth repeating):

- Write for AI as the primary reader. Use unambiguous, enforceable phrasings. "禁止 X" / "必须 Y" / "优先 Z" beats "建议考虑 Y".
- Every red-line rule needs a **why** — one short clause explaining the reason. Without it, AI will rules-lawyer edge cases. Example: "禁止使用 `Number(id)` — JS number 精度只有 53 位，大整数 ID 会丢精度".
- Show paths verbatim with the project's actual casing. `src/service/` and `src/services/` are not interchangeable.
- Tables for tech stack and props. Bullet lists for rules. Code blocks for layout examples and minimal usage snippets — never paste large file contents.
- Don't fabricate file paths, util names, or component names. If something wasn't observed in the scan and wasn't confirmed in the interview, leave it out.
- Output language matches the user's working language (default 中文 if the existing docs and conversation are in Chinese).

### Phase 5 — Show and revise

Write the file to the project root as `AGENTS.md` (or whatever name the user picked — `CLAUDE.md`, `docs/ai-guidelines.md`, etc.).

Then summarize in 2-3 lines: which sections you included, which optional sections you skipped and why, and ask if anything needs adjustment. Don't dump the full doc back into chat — the user can read the file.

If the user requests changes, edit in place rather than regenerating from scratch.

---

## Common pitfalls to avoid

- **Generic prose.** "Components should be well-designed" is useless. "组件 Props 接口必须明确声明，禁止用 `any` 占位；行内 props 超过 5 个时拆 `props` 对象" is enforceable.
- **Overreach from a single example.** One H5 reference doesn't mean every project gets NavHeader rules. Cut sections aggressively.
- **Inventing conventions the team doesn't actually follow.** If you didn't observe it and didn't confirm it, don't write it as a rule.
- **Writing onboarding content.** This document is a constraint contract for AI, not a tutorial. Don't explain what React is. Don't explain what TypeScript does.
- **Forgetting the why.** A red-line without a why is a future bug report. Always give one short reason.
- **One giant file.** If the project genuinely needs more than ~600 lines of guidance, split: keep `AGENTS.md` lean and reference `docs/common-components.md`, `docs/api-conventions.md`, etc.

---

## When this skill should NOT trigger

- The user wants a README for end users → that's `README.md`, different audience.
- The user wants API documentation → that's OpenAPI / Swagger / generated docs.
- The user wants commit message conventions only → that's a small `CONTRIBUTING.md` addition, not a full AGENTS.md.
- The user wants Claude Code settings (hooks, permissions) → use the `update-config` skill instead.

If in doubt, ask once before scanning.
