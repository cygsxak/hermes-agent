<p align="center">
  <img src="assets/banner.png" alt="Hermes Agent" width="100%">
</p>

# Hermes Agent ☤ 中文文档

<p align="center">
  <a href="https://hermes-agent.nousresearch.com/docs/"><img src="https://img.shields.io/badge/文档-hermes--agent.nousresearch.com-FFD700?style=for-the-badge" alt="Documentation"></a>
  <a href="https://discord.gg/NousResearch"><img src="https://img.shields.io/badge/Discord-5865F2?style=for-the-badge&logo=discord&logoColor=white" alt="Discord"></a>
  <a href="https://github.com/NousResearch/hermes-agent/blob/main/LICENSE"><img src="https://img.shields.io/badge/许可证-MIT-green?style=for-the-badge" alt="License: MIT"></a>
  <a href="https://nousresearch.com"><img src="https://img.shields.io/badge/出品-Nous%20Research-blueviolet?style=for-the-badge" alt="Built by Nous Research"></a>
</p>

**Hermes Agent 是由 [Nous Research](https://nousresearch.com) 打造的自我进化型 AI 智能体。** 它是目前唯一内置学习闭环的 Agent 框架——能从经验中创建技能、在使用中持续优化、主动积累知识、检索历史对话，并跨会话构建对你的深度理解。它可以部署在 5 美元/月的 VPS、GPU 集群，或几乎不产生空闲费用的 Serverless 环境。无需绑定本地电脑——你可以随时通过 Telegram 与它交互，同时让它在云端 VM 上处理任务。

支持自由选择任意模型：[Nous Portal](https://portal.nousresearch.com)、[OpenRouter](https://openrouter.ai)（200+ 模型）、[z.ai/GLM](https://z.ai)、[Kimi/Moonshot](https://platform.moonshot.ai)、[MiniMax](https://www.minimax.io)、OpenAI，或你自己的模型端点。使用 `hermes model` 随时切换，无需改代码，不被平台绑定。

---

## 项目亮点

| 特性 | 说明 |
|------|------|
| **真正的终端界面** | 完整 TUI，支持多行编辑、斜杠命令自动补全、对话历史、中断重定向与流式工具输出 |
| **随处可用** | 支持 Telegram、Discord、Slack、WhatsApp、Signal 及 CLI——单一 Gateway 进程统一管理。支持语音便签转写与跨平台对话连续性 |
| **闭环学习系统** | Agent 自动管理记忆并定期巩固。复杂任务后自主创建技能，技能在使用中自我优化。FTS5 会话搜索 + LLM 摘要实现跨会话回忆。[Honcho](https://github.com/plastic-labs/honcho) 辩证用户建模，兼容 [agentskills.io](https://agentskills.io) 开放标准 |
| **定时自动化** | 内置 Cron 调度器，可推送到任意平台。自然语言配置每日报告、夜间备份、每周审计等任务，无人值守运行 |
| **并行委托** | 支持派生独立子 Agent 并行处理任务流。可编写 Python 脚本通过 RPC 调用工具，将多步骤流程压缩为零上下文开销的单次执行 |
| **随处运行，不限本地** | 六种终端后端：本地、Docker、SSH、Daytona、Singularity 和 Modal。Daytona/Modal 提供 Serverless 持久化——空闲时休眠，按需唤醒，几乎零待机成本 |
| **科研就绪** | 批量轨迹生成、Atropos RL 环境、轨迹压缩，助力下一代工具调用模型训练 |

---

## 快速安装

```bash
curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash
```

支持 Linux、macOS、WSL2 及 Android（Termux）。安装脚本会自动处理平台差异。

> **Android / Termux：** 已验证的手动安装路径请参见 [Termux 指南](https://hermes-agent.nousresearch.com/docs/getting-started/termux)。在 Termux 上，Hermes 会安装精简版 `.[termux]` 扩展，因为完整 `.[all]` 扩展目前包含 Android 不兼容的语音依赖。
>
> **Windows：** 暂不支持原生 Windows。请先安装 [WSL2](https://learn.microsoft.com/zh-cn/windows/wsl/install)，再执行上述命令。

安装完成后：

```bash
source ~/.bashrc    # 重新加载 Shell（或：source ~/.zshrc）
hermes              # 开始对话！
```

---

## 基本用法

```bash
hermes              # 交互式 CLI——开始对话
hermes model        # 选择 LLM 提供商和模型
hermes tools        # 配置启用的工具
hermes config set   # 设置单项配置
hermes gateway      # 启动消息 Gateway（Telegram、Discord 等）
hermes setup        # 运行完整配置向导（一次性完成所有配置）
hermes claw migrate # 从 OpenClaw 迁移（如果你使用过 OpenClaw）
hermes update       # 更新到最新版本
hermes doctor       # 诊断问题
```

📖 **[完整文档 →](https://hermes-agent.nousresearch.com/docs/)**

### CLI 与消息平台快速对照

Hermes 有两种入口：通过 `hermes` 启动终端 UI，或运行 Gateway 从 Telegram、Discord、Slack、WhatsApp、Signal 等平台与之对话。进入对话后，许多斜杠命令在两种界面通用。

| 操作 | CLI | 消息平台 |
|------|-----|----------|
| 开始对话 | `hermes` | 运行 `hermes gateway setup` + `hermes gateway start`，然后给 Bot 发消息 |
| 开始新对话 | `/new` 或 `/reset` | `/new` 或 `/reset` |
| 切换模型 | `/model [provider:model]` | `/model [provider:model]` |
| 设置人格 | `/personality [name]` | `/personality [name]` |
| 重试或撤销上一轮 | `/retry`、`/undo` | `/retry`、`/undo` |
| 压缩上下文 / 查看用量 | `/compress`、`/usage`、`/insights [--days N]` | `/compress`、`/usage`、`/insights [days]` |
| 浏览技能 | `/skills` 或 `/<skill-name>` | `/skills` 或 `/<skill-name>` |
| 中断当前任务 | `Ctrl+C` 或发送新消息 | `/stop` 或发送新消息 |
| 平台状态 | `/platforms` | `/status`、`/sethome` |

详细命令请参见 [CLI 指南](https://hermes-agent.nousresearch.com/docs/user-guide/cli) 和 [消息 Gateway 指南](https://hermes-agent.nousresearch.com/docs/user-guide/messaging)。

---

## 主要功能

### 工具与工具集（40+ 工具）

Hermes Agent 内置丰富工具，覆盖终端操作、文件读写、网页搜索与抓取、代码执行、浏览器自动化、MCP 集成等场景。可通过 `hermes tools` 按需启用或禁用工具集。

### 技能系统（Skill System）

技能是 Hermes 的程序性记忆单元。Agent 在完成复杂任务后会自动提炼并保存技能，也支持手动创建。技能存储于 `~/.hermes/skills/`，在使用中持续优化。你还可以通过 [agentskills.io](https://agentskills.io) Skills Hub 安装社区技能。

### 记忆系统（Memory）

- 自动管理持久化记忆，跨会话保留上下文
- FTS5 全文检索历史会话，LLM 摘要辅助跨会话回忆
- [Honcho](https://github.com/plastic-labs/honcho) 辩证用户建模，让 Agent 更了解你

### 消息 Gateway

单一 Gateway 进程同时接入多个消息平台：

- **Telegram** — 支持语音消息转写、内联命令
- **Discord** — Bot 集成
- **Slack** — `/hermes` 斜杠命令
- **WhatsApp** — 通过 WhatsApp Business API
- **Signal** — 私密通信
- **Home Assistant** — 智能家居联动

### 定时任务（Cron）

内置调度器，支持自然语言配置定时任务，结果推送到任意已连接平台：

```
每天早上 8 点发送今日天气和日程摘要
每周日晚上备份工作目录
每小时检查服务器状态
```

### MCP 集成

支持连接任意 MCP（Model Context Protocol）服务器，扩展工具能力边界。参见 [MCP 文档](https://hermes-agent.nousresearch.com/docs/user-guide/features/mcp)。

### 多终端后端

| 后端 | 适用场景 |
|------|----------|
| 本地 | 日常开发，直接访问本机 |
| Docker | 沙箱隔离，一致环境 |
| SSH | 远程服务器 |
| Daytona | Serverless 持久化开发环境 |
| Singularity / Apptainer | HPC 高性能计算集群 |
| Modal | 按需云端执行，几乎零空闲成本 |

---

## 技能开发示例

技能是存储在 `~/.hermes/skills/` 下的 Markdown 文件，描述 Agent 应如何完成某类任务。以下为一个简单示例：

```markdown
# daily-report

每天早上生成日报并发送到 Telegram。

## 步骤

1. 读取今日日历事件（`~/.hermes/calendar.json`）
2. 检查待办事项列表
3. 汇总昨日完成的任务
4. 格式化为简洁的 Markdown 日报
5. 通过 gateway 发送到用户的 Telegram

## 注意事项

- 报告应简明扼要，控制在 20 行以内
- 遇到错误时提供简短说明，而非完整堆栈
```

激活技能：在对话中输入 `/daily-report`，或在 CLI 中直接使用技能名作为斜杠命令。

你也可以通过 Skills Hub 安装社区技能：

```bash
# 在对话中使用 /skills 命令浏览和安装
/skills
```

---

## 从 OpenClaw 迁移

如果你之前使用过 OpenClaw，Hermes 可以自动导入你的设置、记忆、技能和 API 密钥。

**首次安装时：** 配置向导（`hermes setup`）会自动检测 `~/.openclaw` 并在开始配置前提示迁移。

**安装后随时迁移：**

```bash
hermes claw migrate              # 交互式迁移（完整预设）
hermes claw migrate --dry-run    # 预览将迁移的内容
hermes claw migrate --preset user-data   # 仅迁移数据，不含密钥
hermes claw migrate --overwrite  # 覆盖已有冲突文件
```

可迁移内容：SOUL.md 人格文件、记忆（MEMORY.md / USER.md）、用户技能、命令白名单、消息平台配置、API 密钥、TTS 资源、工作区说明（AGENTS.md）。

---

## 文档导航

所有文档位于 **[hermes-agent.nousresearch.com/docs](https://hermes-agent.nousresearch.com/docs/)**：

| 章节 | 内容 |
|------|------|
| [快速开始](https://hermes-agent.nousresearch.com/docs/getting-started/quickstart) | 安装 → 配置 → 2 分钟完成第一次对话 |
| [CLI 使用](https://hermes-agent.nousresearch.com/docs/user-guide/cli) | 命令、快捷键、人格、会话管理 |
| [配置说明](https://hermes-agent.nousresearch.com/docs/user-guide/configuration) | 配置文件、提供商、模型、全部选项 |
| [消息 Gateway](https://hermes-agent.nousresearch.com/docs/user-guide/messaging) | Telegram、Discord、Slack、WhatsApp、Signal |
| [安全](https://hermes-agent.nousresearch.com/docs/user-guide/security) | 命令审批、DM 配对、容器隔离 |
| [工具与工具集](https://hermes-agent.nousresearch.com/docs/user-guide/features/tools) | 40+ 工具、工具集系统、终端后端 |
| [技能系统](https://hermes-agent.nousresearch.com/docs/user-guide/features/skills) | 程序性记忆、Skills Hub、创建技能 |
| [记忆系统](https://hermes-agent.nousresearch.com/docs/user-guide/features/memory) | 持久记忆、用户画像、最佳实践 |
| [MCP 集成](https://hermes-agent.nousresearch.com/docs/user-guide/features/mcp) | 连接任意 MCP 服务器扩展能力 |
| [定时任务](https://hermes-agent.nousresearch.com/docs/user-guide/features/cron) | 定时任务与平台推送 |
| [上下文文件](https://hermes-agent.nousresearch.com/docs/user-guide/features/context-files) | 影响每次对话的项目上下文 |
| [架构说明](https://hermes-agent.nousresearch.com/docs/developer-guide/architecture) | 项目结构、Agent 循环、核心类 |
| [贡献指南](https://hermes-agent.nousresearch.com/docs/developer-guide/contributing) | 开发环境搭建、PR 流程、代码风格 |
| [CLI 命令参考](https://hermes-agent.nousresearch.com/docs/reference/cli-commands) | 全部命令与参数 |
| [环境变量](https://hermes-agent.nousresearch.com/docs/reference/environment-variables) | 完整环境变量参考 |

---

## 参与贡献

欢迎任何形式的贡献！详见 [贡献指南](https://hermes-agent.nousresearch.com/docs/developer-guide/contributing)，了解开发环境搭建、代码风格和 PR 流程。

贡献者快速开始：

```bash
git clone https://github.com/NousResearch/hermes-agent.git
cd hermes-agent
curl -LsSf https://astral.sh/uv/install.sh | sh
uv venv venv --python 3.11
source venv/bin/activate
uv pip install -e ".[all,dev]"
python -m pytest tests/ -q
```

> **RL 训练（可选）：** 如需参与 RL/Tinker-Atropos 集成开发：
> ```bash
> git submodule update --init tinker-atropos
> uv pip install -e "./tinker-atropos"
> ```

你可以通过以下方式参与：

- 🐛 [提交 Issue](https://github.com/NousResearch/hermes-agent/issues)——反馈 Bug 或提出功能需求
- 💡 [参与讨论](https://github.com/NousResearch/hermes-agent/discussions)——分享想法和使用经验
- 🔀 Fork 仓库，创建分支，提交 Pull Request
- 📖 完善文档或添加中文翻译
- 🧩 开发并分享新技能到 [Skills Hub](https://agentskills.io)

---

## 联系方式与支持

| 渠道 | 链接 |
|------|------|
| 💬 Discord 社区 | [discord.gg/NousResearch](https://discord.gg/NousResearch) |
| 📚 Skills Hub | [agentskills.io](https://agentskills.io) |
| 🐛 问题反馈 | [GitHub Issues](https://github.com/NousResearch/hermes-agent/issues) |
| 💡 功能讨论 | [GitHub Discussions](https://github.com/NousResearch/hermes-agent/discussions) |
| 🌐 官方文档 | [hermes-agent.nousresearch.com/docs](https://hermes-agent.nousresearch.com/docs/) |
| 🏢 Nous Research | [nousresearch.com](https://nousresearch.com) |

---

## 许可证

MIT 开源协议——详见 [LICENSE](LICENSE)。

由 [Nous Research](https://nousresearch.com) 出品。
