# My Skills Repository

这是一个集中管理 AI Agent Skills 的仓库，方便团队成员下载和使用。

## 目录结构

```
my-skills/
├── find-skills/              # 技能发现与安装
├── frontend-design/          # 前端界面设计
├── mcp-builder/              # MCP服务器开发
├── skill-creator/            # 技能创建与优化
├── rule-trigger-system/       # 动态规则触发系统
└── inspiration-extractor/    # 深度洞察提取
```

## Skills 列表

### find-skills
帮助用户发现和安装 agent skills。当你询问"如何做X"、"找一个能做X的skill"或表达想要扩展功能时触发。

### frontend-design
创建独特、生产级的前端界面，具有高设计质量。用于构建网页组件、落地页、仪表盘、React组件、HTML/CSS布局或任何Web UI美化。

### mcp-builder
用于创建高质量的MCP (Model Context Protocol) 服务器，使LLM能够通过设计良好的工具与外部服务交互。支持Python (FastMCP) 和 Node/TypeScript (MCP SDK)。

### skill-creator
从零创建新技能、修改和优化现有技能、测量技能性能。用于技能开发、测试评估、性能基准分析等场景。

### rule-trigger-system
基于MCP Resource的动态规则加载系统。根据用户意图、文件类型和关键词自动匹配并加载必要的规则文件，避免不必要的上下文损耗。

### inspiration-extractor
使用"元认知视角内容解读方法" (Protocol v2) 从内容中提取深度洞察和灵感。适用于跨学科视角分析。

## 安装方式

将对应 skill 目录复制到 `~/.config/opencode/skills/` 目录下即可。

```bash
# 示例：安装 find-skills
cp -r find-skills ~/.config/opencode/skills/
```

## 开发

本仓库使用 [skill-creator](https://github.com/your-repo/skill-creator) 技能进行管理和优化。
