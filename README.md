# My Skills Repository

这是一个集中管理 AI Agent Skills 的仓库，方便团队成员下载和使用。

## 目录结构

```
my-skills/
├── agent-browser/            # 浏览器自动化
├── find-skills/              # 技能发现与安装
├── frontend-design/          # 前端界面设计
├── inspiration-extractor/    # 深度洞察提取
├── mcp-builder/              # MCP服务器开发
├── rule-trigger-system/       # 动态规则触发系统
├── shadcn-ui/                # shadcn/ui组件集成
├── skill-creator/            # 技能创建与优化
├── ui-ux-pro-max/            # UI/UX设计智能
└── vercel-react-best-practices/  # React/Next.js性能优化
```

## Skills 列表

### agent-browser （有风险的技能）
浏览器自动化CLI工具，用于AI agent与网站交互。通过CDP直接控制Chrome/Chromium，支持页面导航、表单填写、按钮点击、截图、数据提取、Web应用测试等浏览器任务。支持多浏览器引擎、会话管理、认证保存等功能。

### find-skills （适合全局安装）
帮助用户发现和安装 agent skills。当你询问"如何做X"、"找一个能做X的skill"或表达想要扩展功能时触发。

### frontend-design （适合全局安装）
创建独特、生产级的前端界面，具有高设计质量。用于构建网页组件、落地页、仪表盘、React组件、HTML/CSS布局或任何Web UI美化。

### inspiration-extractor
使用"元认知视角内容解读方法" (Protocol v2) 从内容中提取深度洞察和灵感。适用于跨学科视角分析。

### mcp-builder （适合全局安装）
用于创建高质量的MCP (Model Context Protocol) 服务器，使LLM能够通过设计良好的工具与外部服务交互。支持Python (FastMCP) 和 Node/TypeScript (MCP SDK)。

### rule-trigger-system 
#### （个人构建的规则分发器，减少上下文占据的情况下补充规则，建议自定义优化）
基于MCP Resource的动态规则加载系统。根据用户意图、文件类型和关键词自动匹配并加载必要的规则文件，避免不必要的上下文损耗。

### shadcn-ui （适合全局安装）
shadcn/ui 组件集成专家，帮助在 React 应用中发现、安装、定制和优化 shadcn/ui 组件。shadcn/ui 不是传统组件库，而是一组可复用的组件代码，直接复制到项目中。支持 Radix UI 或 Base UI 原语、Tailwind CSS 样式化，提供 50+ 可定制组件和完整的 UI Blocks（认证表单、仪表盘、侧边栏导航等）。用于组件发现、安装、定制、主题化、扩展组件和最佳实践指导。

### skill-creator （适合全局安装）
从零创建新技能、修改和优化现有技能、测量技能性能。用于技能开发、测试评估、性能基准分析等场景。

### ui-ux-pro-max （适合全局安装）
UI/UX设计智能工具，涵盖50+样式、161+配色方案、57+字体搭配、161+产品类型、99+ UX指南和25+图表类型。支持React、Next.js、Vue、Svelte、SwiftUI、React Native、Flutter、Tailwind、shadcn/ui和HTML/CSS等10个技术栈。用于规划、建设、创建、设计、实施、审查、修复、改进、优化和检查UI/UX代码。

### vercel-react-best-practices （适合全局安装）
Vercel工程团队维护的React和Next.js性能优化指南。包含64条规则，涵盖8大类别（消除瀑布流、Bundle优化、服务器性能、客户端数据获取、重渲染优化、渲染性能、JavaScript性能和高级模式），按优先级排序指导自动化重构和代码生成。

## 安装方式
对于习惯使用cli工具，比如opencode，Claude code的同学，建议使用 cc switch 去管理相关的skills。

使用 `npx skills add` 命令安装skills：

```bash
# 安装所有skills
npx skills add https://github.com/nextlevelbuilder/skills

# 安装单个skill
npx skills add https://github.com/nextlevelbuilder/skills --skill ui-ux-pro-max
npx skills add https://github.com/nextlevelbuilder/skills --skill agent-browser
npx skills add https://github.com/nextlevelbuilder/skills --skill vercel-react-best-practices
```

## 开发

本仓库使用 [skill-creator](https://github.com/your-repo/skill-creator) 技能进行管理和优化。
