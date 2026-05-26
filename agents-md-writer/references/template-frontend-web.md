# Frontend Web Template

Sections to add on top of `template-base.md` for browser-targeted frontend projects (React / Vue / Svelte / Angular, Vite / Next / Nuxt / Remix).

---

## Core philosophy additions

Add these buckets to `## 🎯 核心开发理念`:

- **声明式 UI 与组件化**
  - 基于状态渲染，禁止直接操作 DOM（除非显式注释说明）
  - 组件 Props 接口必须显式声明类型
  - 受控组件优先，明确数据流向
  - 条件渲染使用早期返回或三元，避免 3 层以上嵌套

- **状态与副作用边界**
  - 业务逻辑抽离为纯函数或自定义 Hook
  - 副作用集中在 `useEffect` / 服务层 / store，禁止散落
  - 全局状态最小化，能 prop 传递的不进 store

---

## Red-line additions

Add under `### 最高优先级原则（红线）`:

```markdown
#### 类型与数据规范（强制）
- 与后端接口类型保持一致；接口请求/响应必须显式定义，放在 `types.ts`
- 禁止 `any`，必要时使用 `unknown` + 类型守卫
- 后端按 `string` 返回的大整数 ID（订单号、工单号等）必须全链路按 `string` 传递；
  禁止 `Number(id)` / `parseInt(id)` / `+id` — JS number 精度 53 位会丢精度
- 后端时间字段统一使用 `{{path/to/dateUtils}}` 中的 `formatDateTime` / `formatDate` 渲染；
  禁止页面内直接渲染时间戳或自实现格式化函数
```

If the team doesn't have central date utils, recommend they create one and call it out as a follow-up.

---

## Section: 组件开发规范

```markdown
### 组件目录结构

src/pages/{PageName}/
├── index.tsx              # 页面入口
├── index.module.{less|css|scss}
├── types.ts               # 页面级类型定义
├── enum.ts                # 页面级枚举（如有）
├── components/            # 页面级私有组件
├── hooks/                 # 页面级 hooks
├── data.ts                # 页面级静态数据（如有）
└── assets/                # 页面级静态资源

src/components/   # 跨页面复用的公共组件
src/hooks/        # 跨页面复用的公共 hooks
src/utils/        # 公共工具函数
src/service/      # 接口请求层（注意：不是 services）
```

(Adapt paths to whatever the project actually uses. If it's `services/` not `service/`, say so and note "本项目使用 `services/`，与某些参考模板不同".)

```markdown
### Hooks 分类与存放位置

| Hooks 类型 | 存放位置 | 示例 |
|-----------|---------|------|
| 页面级 Hooks | `src/pages/{Page}/hooks/` | `useAuthorizationList.ts` |
| 公共 Hooks | `src/hooks/` | `useAuth.ts` |

**命名规范**：Hook 文件和函数均以 `use` 开头，camelCase。
```

```markdown
### 业务域聚合规范

强相关页面（列表 / 详情 / 编辑 / 结果）共属同一业务域时，聚合到同一业务文件夹：

src/pages/{DomainName}/
├── list/
├── detail/
├── components/      # 跨子页共享组件
├── hooks/
├── types.ts
└── enum.ts

**判断准则**：是否共享 `types.ts` / `data.ts` / `assets/` / 子组件。
是 → 按业务域聚合；否 → 按单页 `src/pages/{Page}/index.tsx` 即可。
```

```markdown
### 枚举值规范
- 接口枚举值（状态码、类型字段等）统一抽离到对应业务域的 `enum.ts`
- 禁止页面 / 组件中散落硬编码枚举
- 后端枚举值 → 前端展示文案的映射也放在 `enum.ts` 或同业务域纯函数
```

---

## Section: 样式规范

Pick the approach that matches the project:

### Option A — CSS Module（现代项目推荐）

```markdown
### 样式文件组织（CSS Module）

每个组件 / 页面拥有独立样式作用域，避免全局污染。

src/pages/{Page}/
├── index.tsx
├── index.module.{less|css|scss}
└── components/
    └── {Comp}/
        ├── index.tsx
        └── index.module.{less|css|scss}

#### 使用方式
import styles from './index.module.less';
<div className={styles.pageRoot}>...</div>

#### 命名约定
- 类名使用 camelCase（CSS Module 默认）
- 根容器统一命名为 `{pageName}Root` / `{componentName}Root`
- 禁止使用 `!important` 覆盖样式（除非注释说明为何无法通过结构解决）
```

### Option B — Tailwind / Atomic CSS

```markdown
### 样式规范（Tailwind）
- 优先使用 Tailwind 原子类，不写自定义 CSS
- 复杂样式组合提取为组件，禁止抽 `@apply` 复合类
- 主题颜色 / 间距通过 `tailwind.config.{ts|js}` 中的 token 管理，禁止硬编码
- 响应式断点统一使用 Tailwind 提供的 `sm/md/lg/xl/2xl`
```

### Option C — 历史项目混合

```markdown
### 样式规范（新旧混合）
- 新代码统一使用 CSS Module（`*.module.{less|css}`）
- 历史代码保留全局 less，逐步迁移
- 跨文件的全局变量集中在 `src/styles/variables.{less|css}`
```

---

## Section: 路由与代码分割

```markdown
### 路由
- 使用 {{react-router-dom v7 / next/router / 等}}
- 路由声明集中在 `{{src/routes/index.tsx 或 app/}}`，禁止页面内动态注册
- 大体积页面使用 `React.lazy` + `Suspense` 按需加载
- 路由参数类型必须定义，禁止 `useParams<any>`
```

---

## Section: 性能与体验

Add to `### 代码审查关注点 → 性能层面`:

- 是否有不必要的重渲染？合理使用 `useMemo` / `useCallback` / `React.memo`
- 大列表是否使用虚拟滚动？
- 图片是否懒加载？是否使用合适的尺寸 / 格式（WebP / AVIF）？
- 路由是否按需加载？首屏 bundle size 是否在预算内？

Add `### 体验层面`:
- 加载状态（Skeleton / Spinner）是否友好？
- 错误边界是否覆盖关键页面，避免白屏？
- 空状态是否有引导文案 / CTA？
- 表单失焦校验、提交防抖、按钮 loading 是否处理？
