# My Skills Repository

这是一个集中管理 AI Agent Skills 的仓库，方便团队成员下载和使用。

## 目录结构

```
my-skills/
└── rule-trigger-system/      # 动态规则触发系统
```

## Skills 列表

### ✅ rule-trigger-system (v2.0.0) — 已实现

基于 MCP Resource 的动态规则加载系统。根据用户意图、文件类型和关键词自动匹配并加载必要的规则文件，避免不必要的上下文损耗。

**核心功能：**
- 意图识别 → 触发器匹配 → 资源声明 → 按需加载规则 → 执行任务
- 支持 6 种规则文件：react-patterns、ts-strict、ui-system、testing-strategy、security-compliance、performance-tuning
- 智能优先级机制（Security 100 > React 95 > TypeScript 90 > UI 90 > Testing 85 > Performance 80）
- 多重触发时自动解决规则冲突
- 完善的错误处理与优雅降级

**使用方式：**
当 AI 处理代码、配置或文档任务时自动触发，确保仅加载当前任务所需的最小规则集。

## 安装方式

将对应 skill 目录复制到 `~/.config/opencode/skills/` 目录下即可。

```bash
# 示例：安装 rule-trigger-system
cp -r rule-trigger-system ~/.config/opencode/skills/
```

## 开发

本仓库用于管理已开发的 Skills。
