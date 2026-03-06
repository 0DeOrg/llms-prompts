# Skill Seekers 安装与使用实战指南

这份指南基于实际操作经验总结，旨在帮助团队成员快速上手 `skill-seekers`，将各类官方文档高效转换为 AI 技能（Skills）。

## 1. 工具简介

**Skill Seekers** 是一个强大的 CLI 工具，能够将文档网站、GitHub 仓库和 PDF 文件转换为结构化的知识库，供 Trae、Claude 等 AI 助手使用。

- **核心功能**：一键抓取文档 -> 自动清洗 -> 生成 AI 可读的 Skill。
- **适用场景**：为 AI 补充最新的框架文档、私有库知识。

## 2. 环境准备与安装

确保你的系统已安装 Python 3.x。

### 安装命令

在终端中执行以下命令进行安装：

```bash
pip install skill-seekers
```

或者使用 `pip3`：

```bash
pip3 install skill-seekers
```

## 3. 交互式配置（推荐）

Skill Seekers 提供了非常直观的交互式配置界面。**建议使用 `sudo` 运行以避免权限问题。**

### 3.1 启动配置向导

在终端执行：

```bash
sudo skill-seekers config
```

你将看到如下操作界面：

```text
╔═══════════════════════════════════════════════════╗
║         Skill Seekers Configuration              ║
╚═══════════════════════════════════════════════════╝

  1. GitHub Token Setup
  2. API Keys (Claude, Gemini, OpenAI)
  3. Rate Limit Settings
  4. Resume Settings
  5. View Current Configuration
  6. Test Connections
  7. Clean Up Old Progress Files
  0. Exit

Select an option [0-7]:
```

### 3.2 配置步骤

1.  **设置 GitHub Token**：输入 `1`，按提示添加你的 GitHub Token（用于抓取仓库）。
2.  **设置 AI API Keys**：输入 `2`，配置 OpenAI / Anthropic / Gemini Key（用于数据清洗和增强）。
3.  **查看配置状态**：输入 `5`，确认配置已生效。
    - _正确状态示例：_
    ```text
    📋 Skill Seekers Configuration
    Config file: /Users/mac/.config/skill-seekers/config.json
    ...
    GitHub Profiles: 1
      • work (default)
    API Keys:
      • Anthropic: ✅ Set (from config)
      • Openai: ✅ Set (from config)
    ```
4.  **测试连接**：输入 `6`，系统会自动检测所有 Key 是否有效。
    - _成功示例：_
    ```text
    Testing GitHub tokens...
      ✅ work: 5000/5000 requests remaining
    Testing API keys...
      ℹ️  Anthropic: Key configured
      ℹ️  OpenAI: Key configured
    ```

## 4. 实战演练：抓取文档与代码库

配置完成后，我们通过两个真实的例子来演示：一个是开源代码库 `yt-dlp`，另一个是开发者文档 `Privy`。

### 4.1 场景一：转换 GitHub 代码库 (yt-dlp)

我们以著名的视频下载工具 `yt-dlp` 为例，将其代码库转换为技能。

**执行抓取命令**：

```bash
# 方式 A：使用配置文件中的 Token（推荐）
skill-seekers github \
  --repo yt-dlp/yt-dlp \
  --name yt-dlp-skill \
  --profile work

# 方式 B：临时指定 Token（解决权限问题）
GITHUB_TOKEN=你的Token skill-seekers github \
  --repo yt-dlp/yt-dlp \
  --name yt-dlp-skill
```

- `--repo`: 目标仓库的 `Owner/Name`。
- `--profile`: 指定使用哪个配置文件的 Token（解决多账号问题）。
- `GITHUB_TOKEN=...`: 如果遇到配置文件权限报错，可用此环境变量强制传参。

### 4.2 场景二：抓取在线开发文档 (Privy)

接下来我们抓取 `Privy` 的官方文档，方便后续集成登录功能。

**执行抓取命令**：

```bash
skill-seekers scrape "https://docs.privy.io/welcome" \
  --name "privy-skill" \
  --description "Privy SDK Documentation" \
  --max-pages 50
```

- `scrape`: 抓取网页模式。
- `--max-pages`: 限制抓取页数，防止抓取过多无关页面。

### 4.3 产物使用与集成

命令执行成功后，会在 `output/` 文件夹生成对应的技能包：

```text
output/
├── yt-dlp-skill/      # yt-dlp 技能包
│   ├── SKILL.md       # 核心技能摘要
│   └── references/    # 详细参考文档（README, Issues, etc.）
└── privy-skill/       # Privy 技能包
    ├── SKILL.md
    └── references/
```

**在 IDE 中使用**：
为了让当前项目能够长期使用这些技能，建议将其移动到项目的 `.agents/skills` 目录，供各类 AI 编程助手（如 Trae, Claude Code, Cursor 等）读取使用：

```bash
mkdir -p .agents/skills
mv output/yt-dlp-skill .agents/skills/yt-dlp
mv output/privy-skill .agents/skills/privy
```

这样，你的 AI 助手就可以通过读取这些路径，随时回答关于 `yt-dlp` 用法或 `Privy` 集成的问题了。

## 5. 常见问题排查 (Troubleshooting)

- **问题 1：`Operation not permitted` / 权限错误**
  - **原因**：普通用户可能无法写入 `~/.config` 目录。
  - **解决**：
    1.  使用 `sudo skill-seekers config` 进行配置。
    2.  或者手动修复目录权限：
        ```bash
        sudo chown -R $(whoami) ~/.config/skill-seekers
        ```

- **问题 2：`unrecognized arguments: --url`**
  - **解决**：新版本中 URL 是位置参数，不需要加 `--url` 标记。直接写 `skill-seekers scrape https://...`。

- **问题 3：抓取内容为空**
  - **解决**：
    - 检查网络连接。
    - 确保 API Key 配置正确（运行 `sudo skill-seekers config` -> 选项 `6` 测试）。
    - 对于复杂网站，可能需要创建 `config.json` 指定 CSS 选择器。

---

_文档生成时间：2026-02-09_
