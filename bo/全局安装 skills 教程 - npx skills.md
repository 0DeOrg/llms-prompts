# npx skills 完整实用教程

**npx skills** 是 Vercel 推出的开源 Agent Skills 包管理器，相当于 **“npm for AI agents”**。

它允许你一键安装 **Agent Skills**（AI 代理技能），让 Cursor、Claude Code、Gemini CLI、OpenCode、Codex、GitHub Copilot 等 30+ 种 AI 编码工具自动获得专业领域的知识、规则、最佳实践和工具调用能力。

**核心卖点**：一次编写，到处使用（agent-agnostic）。

## 1. Agent Skills 是什么？

- 开放标准（源于 Anthropic Claude Code，后被广泛采纳，详见 [agentskills.io](https://agentskills.io/)）
- 本质：一个或多个 [**SKILL.md**](http://skill.md/) 文件（Markdown + YAML 前置元数据）
- 内容包含：领域特定指令、最佳实践、常见陷阱、模板、工具描述、触发规则等
- 典型场景：React/Next.js 性能优化、Terraform 安全、Stripe 支付流程、网页爬取（Firecrawl）、发邮件（Resend）等
- 安装后，AI 会在相关任务中自动调用这些技能，提升输出质量

## 2. 前置条件

- Node.js v18+（npx 自带，无需额外安装 CLI）
- 已使用支持 Skills 的 AI 工具（Cursor、Claude Code、Gemini CLI 等最常见）
- 无需全局安装 —— 永远用 `npx skills` 运行最新版

## 3. 核心命令速查表

| 命令 | 作用 | 示例命令示例 |
| --- | --- | --- |
| `npx skills find [关键词]` | 交互式搜索技能（最推荐先用） | `npx skills find react performance` |
| `npx skills add <package>` | 安装技能（最常用） | `npx skills add vercel-labs/agent-skills` |
| `npx skills add <url>` | 从 GitHub/URL 安装 | `npx skills add <https://github.com/>...` |
| `npx skills list` 或 `ls` | 列出已安装技能 | — |
| `npx skills check` | 检查更新 | — |
| `npx skills update` | 更新所有技能到最新版 | — |
| `npx skills remove <skill名>` | 卸载技能 | `npx skills remove vercel-react` |
| `npx skills init [名字]` | 创建新技能模板（开发自己的 [SKILL.md](http://skill.md/)） | `npx skills init my-company-nextjs` |

## 4. 一步步上手教程

### 步骤 1：发现技能（Discovery）

- 官网浏览（最直观）：[https://skills.sh/](https://skills.sh/) （排行榜 + 搜索）
- 终端快速搜索（最快）：
→ 会弹出交互选择界面（类似 fzf），选定后直接安装
    
    ```bash
    npx skills find next.js
    npx skills find terraform
    npx skills find stripe
    
    ```
    

### 步骤 2：安装技能（最常用操作）

推荐从热门开始：

```bash
# Vercel 官方 React/Next.js 技能（安装量 Top 1，强烈推荐！）
npx skills add vercel-labs/agent-skills

# 安装特定子技能（如果 repo 有多个）
npx skills add vercel-labs/agent-skills@vercel-react-best-practices

# Neon 数据库最佳实践
npx skills add neondatabase/agent-skills

# HashiCorp Terraform / Vault
npx skills add hashicorp/agent-skills

# Remotion 视频生成
npx skills add remotion-dev/skills

```

安装流程：

1. CLI 自动检测你已安装的 agent（如 Cursor、Claude Code）
2. 选择：全局（所有项目）或当前项目（推荐项目级）
3. 选具体 skill（如果有多个）
4. 自动复制 [SKILL.md](http://skill.md/) 到对应目录（如 `~/.cursor/skills/`、`~/.claude/skills/` 等）

### 步骤 3：验证是否生效

- 打开 Cursor / Claude Code 等工具
- 新建文件或聊天，问：“帮我写一个高性能的 Next.js 组件”
- AI 应该明显更专业：提到 useMemo、避免不必要 re-render、Server Components 优先等（技能已注入）

### 步骤 4：保持更新

每周运行一次：

```bash
npx skills check     # 检查更新
npx skills update    # 一键全量更新

```

### 步骤 5：自己创建技能（进阶）

```bash
npx skills init my-awesome-skill

```

- 生成 [SKILL.md](http://skill.md/) 模板
- 填写 YAML 前置 + 详细指令
- 推到 GitHub
- 别人即可 `npx skills add yourname/your-repo` 安装

## 5. 热门技能推荐（2026 年 2 月 Top 级）

1. **vercel-labs/agent-skills** → React/Next.js/Vercel 全家桶（安装量 Top 1）
2. **vercel-labs/skills find-skills** → 元技能，让 agent 自己帮你找技能
3. **stripe/agent-skills** → Stripe 支付最佳实践
4. **neondatabase/agent-skills** → Postgres + Neon serverless
5. **resend/agent-skills** → 事务邮件 / 发邮件模板
6. **firecrawl/agent-skills** → 网页爬取 & 结构化提取
7. **elevenlabs/agent-skills** → 语音合成最佳实践

## 6. 常见问题 & 注意事项

- **技能没生效？** → 重启 agent / VS Code，检查 `~/.cursor/skills` 等目录是否有文件
- **优先级冲突？** → 项目级 > 全局级 > agent 自带 prompt
- **安全建议** → 只从可信 repo 安装（[skills.sh](http://skills.sh/) 有安装量排行参考）
- **彻底删除** → `npx skills remove xxx` 或手动删 skills 目录
- **支持的 agent 列表**（部分）：Cursor、Claude Code、Gemini CLI、OpenCode、Codex、GitHub Copilot、Windsurf、Trae、Antigravity 等

一句话总结：

**想让 AI 写代码专业度提升 10 倍？从 `npx skills find react` 开始，5 分钟见效！**