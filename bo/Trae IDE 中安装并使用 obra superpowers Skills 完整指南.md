# Trae IDE 中安装并使用 obra/superpowers Skills 完整指南

**文档目标**：从零开始，把 obra/superpowers 的skills 安装到 Trae AI IDE 中，并让它完全生效。如果在Trae中 通过对话安装superpowers ，将会安装在当前项目下。

## 前提条件

- Trae 已安装并登录（免费版或 Pro 版均可）
- 推荐使用 Claude 3.5 / 3.7 Sonnet 或 GPT-4o 模型（superpowers 针对 Claude 优化最好）
- 操作系统：macOS / Windows / Linux 均支持

## 1. 了解 superpowers Skills 是什么

obra/superpowers 是 Jesse Vincent（obra）开发的 agentic 技能框架，包含一套完整的软件开发方法论。

**核心 skills（Markdown 文件）**：

- [brainstorming.md](http://brainstorming.md) → 脑暴 + 精炼需求
- [writing-plans.md](http://writing-plans.md) → 写详细执行计划（文件路径、步骤、验证）
- [test-driven-development.md](http://test-driven-development.md) → 严格 TDD（红-绿-重构）
- [systematic-debugging.md](http://systematic-debugging.md) → 系统性调试
- [executing-plans.md](http://executing-plans.md) → 批量执行计划 + 检查点
- [subagent-driven-development.md](http://subagent-driven-development.md) → 子 agent 协作加速
- [verification-before-completion.md](http://verification-before-completion.md) → 完成前全面验证
- ...（总共 10+ 个，仓库持续更新）

这些 .md 文件本质上是"技能说明书"，Trae agent 会根据上下文自动读取并应用。

## 2. 安装步骤（全局安装，推荐）

Trae 支持两种 skills 位置：

- **项目级**：项目根目录下的 `.trae/skills/`（只在本项目生效）
- **全局级**：用户主目录下的 `~/.trae/skills/`（所有项目通用，推荐新手先用这个）

### 步骤详解（全局安装）

**1. 打开终端**（Trae 内置 Terminal 也可以）

**2. 克隆仓库到临时目录**（随便起名，避免覆盖）

```bash
git clone https://github.com/obra/superpowers.git ~/temp-superpowers
```

**3. 创建 Trae 的全局 skills 目录**（如果不存在会自动创建）

```bash
mkdir -p ~/.trae/skills
```

**4. 只拷贝 skills 文件夹里的 .md 文件**（核心！其他文件夹不要拷贝）

macOS/Linux：

```bash
cp ~/temp-superpowers/skills/*.md ~/.trae/skills/
```

Windows（PowerShell）：

```powershell
Copy-Item -Path ~/temp-superpowers/skills/*.md -Destination ~/.trae/skills/
```

或者用文件资源管理器手动拖拽 `temp-superpowers/skills/` 里的所有 .md 到 `C:\Users\你的用户名\.trae\skills\`

**5. （可选）清理临时目录**

```bash
rm -rf ~/temp-superpowers
```

**6. 重启 Trae**（最保险）

- 完全关闭 Trae 再重新打开
- 或按 `Ctrl/Cmd + Shift + P` → 输入 "Reload Window" 或 "Restart Agent"

### 验证安装成功

- 打开 Trae → 右边栏 Chat / Agent / Builder 模式
- 新建一个 conversation
- 输入测试提示：
  - "列出我当前可用的 superpowers skills"
  - 或 "Use brainstorming skill to help me plan a simple REST API"
- 如果 agent 回复中提到 "Using skill: brainstorming" 或类似，就成功了！

## 3. 如何在 Trae 中使用这些 Skills

Trae 的 agent 会**自动**或**手动**调用 skills。

### 方式一：自动调用（最强大，推荐日常用）

superpowers 自带 "[using-superpowers.md](http://using-superpowers.md)" 元技能，会教 agent "遇到合适任务时必须搜索并使用相关技能"。

**示例任务**（直接描述就好）：

- "帮我从零做一个 Next.js + Supabase 的博客系统"
  → agent 很可能先 brainstorming → writing-plans → TDD 实现
- "这个登录接口有 bug，帮我修"
  → 自动触发 systematic-debugging
- "优化这段代码的性能和可读性"
  → 可能用 refactoring / best practices 相关

你会看到 agent 回复日志如：

"Using skill: test-driven-development from superpowers..."

### 方式二：手动强制调用某个技能

直接在提示里指定：

- "使用 test-driven-development 技能，用 TDD 方式实现一个快速排序函数。"
- "按照 writing-plans 技能，先写详细计划，包括文件结构和每个步骤的验证方法：实现用户注册功能。"
- "用 systematic-debugging 技能分析为什么这个 API 返回 500 错误。"
- "Search available skills from superpowers, pick the best one and apply it to: [你的需求]"

**热门技能快速触发词**：

- Brainstorming → "brainstorm ideas for..."
- TDD → "follow test-driven-development" 或 "use TDD to implement"
- Planning → "write a detailed plan using writing-plans"
- Debugging → "systematic debugging on this error"
- Review → "request code review" 或 "perform receiving-code-review"

## 4. 常见问题 & 排错

**skills 没生效？**

重启 Trae > 新 conversation > 用 Claude 模型测试 > 检查文件是否真的是 .md（不是 .txt 或文件夹嵌套）

**agent 不自动调用？**

显式说 "Use superpowers skills" 或 "Follow superpowers methodology" 作为开头提示。

**路径不对？**

- 全局：`~/.trae/skills/`（macOS/Linux）或 `C:\Users\用户名\.trae\skills\`（Windows）
- 项目级：项目根/.trae/skills/（新建项目文件夹后自动创建）

**想更新 skills？**

进临时目录 `cd ~/temp-superpowers` → `git pull` → 重新拷贝 .md 文件

**Trae 官方 Skills 面板**

设置 → Rules & Skills → Skills 面板，可查看已加载的 skills（部分版本支持导入 .md 或文件夹）

## 5. 进阶玩法建议

- 结合 MCP servers（浏览器、文件系统等工具） → superpowers 效果翻倍
- 在项目根创建 [AGENTS.md](http://AGENTS.md) 文件，写 "Always use superpowers skills when possible" 作为持久规则
- 试试 SOLO 模式（AI 主导）→ 让 agent 全程按 superpowers 走完一个项目
- 后续可自己写新 skill（参考 [writing-skills.md](http://writing-skills.md)）

### 最后测试任务推荐

> 用 superpowers 的完整流程（brainstorming → writing-plans → test-driven-development → verification-before-completion）帮我实现一个命令行版的石头剪刀布游戏，支持多人对战统计胜率。

---

## 参考文档

- [Superpowers 详细用法教程 - 博客园](https://www.cnblogs.com/gyc567/p/19510203)
- [开源Superpowers库：3步解锁AI编程超能力 - CSDN](https://aicoding.csdn.net/696da84b7c1d88441d8de283.html)
