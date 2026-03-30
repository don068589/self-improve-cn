# Self-Improve: Agent 自我进化系统

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Node.js >= 18](https://img.shields.io/badge/Node.js->=18-green.svg)](https://nodejs.org/)
[![OpenClaw Compatible](https://img.shields.io/badge/OpenClaw-Compatible-blue.svg)](https://openclaw.ai)

> 一个可插拔、模块化的 AI Agent 自我进化框架，让 Agent 从经验中学习并持续提升执行质量。

## 概述

Self-Improve 是一个**生产级的自我进化系统**，专为 AI Agent 设计。它自动扫描 Agent 记忆日志，提取可复用的经验规则，并通过安全的审批流程提出系统改进建议。

### 解决的问题

AI Agent 会犯错。它们会重复犯错。它们会忘记学到的教训。Self-Improve 通过以下方式解决这些问题：

- **从失败中学习** - 分析错误模式和纠正信号
- **提炼可复用规则** - 将原始经验转化为可执行智慧
- **记忆管理** - 三层记忆（HOT/WARM/COLD）实现高效检索
- **安全进化** - 所有系统变更都需要明确审批

### 为什么选这个系统？

与需要复杂配置的记忆框架不同，这是**零依赖**的方法：
- **无数据库** — 纯 Markdown + YAML 存储
- **隐私优先** — 所有数据留在本地
- **模块化** — 只用你需要的
- **安全进化** — 系统改动需要明确批准
## 平台兼容性

| 平台 | 支持 | 说明 |
|------|------|------|
| **OpenClaw** | ✅ 原生 | 为 OpenClaw Agent 团队设计 |
| **Claude Code** | ✅ 兼容 | 适配 Claude 记忆系统 |
| **Cursor** | ✅ 兼容 | 可适配 Cursor 规则 |
| **通用 LLM** | ✅ 兼容 | 适用于任何有记忆日志的 Agent |

### 系统要求

- **Node.js** >= 18.0.0
- **操作系统**: Windows, macOS, Linux
- **存储**: ~50MB 数据空间
- **可选**: Cron/任务调度器实现自动运行

## 架构

\\\
┌─────────────────────────────────────────────────────────────┐
│                    Self-Improve 系统                         │
├─────────────────────────────────────────────────────────────┤
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐   │
│  │ 反思器   │→ │ 分析器   │→ │ 分类器   │→ │ 提案器   │   │
│  │ Reflector│  │ Profiler │  │ Classifier│  │ Proposer │   │
│  └──────────┘  └──────────┘  └──────────┘  └──────────┘   │
│       ↓             ↓             ↓             ↓          │
│  ┌──────────────────────────────────────────────────────┐  │
│  │              记忆层 (HOT/WARM/COLD)                  │  │
│  └──────────────────────────────────────────────────────┘  │
│       ↓                                                     │
│  ┌──────────────────────────────────────────────────────┐  │
│  │  输出: 系统更新 | 知识库 | 博客                      │  │
│  └──────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
\\\

## 核心模块

| 模块 | 功能 | 输入 | 输出 |
|------|------|------|------|
| **feedback-collector** | 扫描记忆日志 | Agent 会话 | 原始信号 |
| **reflector** | 分析模式 | 原始信号 | 模式洞察 |
| **profiler** | 构建 Agent 档案 | 洞察 | 档案更新 |
| **distill-classifier** | 分类学习内容 | 洞察 | 按主题分类的规则 |
| **memory-layer** | 管理记忆层级 | 规则 | HOT/WARM/COLD 存储 |
| **proposer** | 生成改进提案 | 规则 | 提案 |
| **notify** | 发送通知 | 提案 | 警报 |

## 记忆层级

\\\
┌────────────────────────────────────────────────────────┐
│  HOT (data/hot.md)                                     │
│  • ≤100 行，每次会话加载                               │
│  • 高频使用的规则                                       │
│  • 直接影响 Agent 行为                                  │
├────────────────────────────────────────────────────────┤
│  WARM (data/themes/)                                   │
│  • 每文件 ≤200 行，按需加载                            │
│  • 按主题组织（编程、写作等）                           │
│  • 需要相关上下文时查询                                 │
├────────────────────────────────────────────────────────┤
│  COLD (data/archive/)                                  │
│  • 无大小限制，仅显式查询                               │
│  • 历史规则、审计追踪                                   │
│  • 可搜索的归档                                         │
└────────────────────────────────────────────────────────┘
\\\

## 安装

### 快速开始

\\\ash
# 克隆仓库
git clone https://github.com/don068589/self-improve-cn.git
cd self-improve-cn

# 配置
cp user-config.yaml my-config.yaml
# 编辑 my-config.yaml，填写你的路径

# 安装
node scripts/setup.mjs --config my-config.yaml

# 验证
node scripts/status.mjs
\\\

### 配置说明

\\\yaml
# my-config.yaml
storage:
  root: ""/path/to/self-improve""        # 安装目录
  knowledge_root: ""/path/to/learned""   # 学习成果输出路径
  workspace_root: ""/path/to/.openclaw"" # Agent 工作空间

owner:
  name: ""你的名字""
  timezone: ""Asia/Shanghai""

agent:
  main_agent: ""main""  # 主 Agent ID

cron:
  enabled: true
  interval_days: 3      # 每 3 天运行一次
\\\

## 使用方法

### 自动模式（推荐）

1. 安装后会在 \proposals/PENDING.md\ 生成 Cron 任务提案
2. 在 Agent 配置中批准该 Cron 任务
3. 系统每 3 天自动运行

### 手动模式

\\\ash
# 运行完整改进周期
node scripts/run-all.mjs

# 运行特定模块
node scripts/improve.mjs --module reflector

# 检查状态
node scripts/status.mjs

# 生成报告
node scripts/report.mjs
\\\

### 集成到 OpenClaw

\\\javascript
// 在 OpenClaw 配置中
{
  ""skills"": [""self-improve""],
  ""cron"": {
    ""self-improve"": {
      ""schedule"": ""0 10 */3 * *"",
      ""enabled"": true
    }
  }
}
\\\

## 输出类型

| 类型 | 位置 | 需要审批 |
|------|------|----------|
| HOT 记忆更新 | \data/hot.md\ | 否（自动） |
| WARM 主题规则 | \data/themes/\ | 否（自动） |
| 系统文件变更 | \proposals/PENDING.md\ | **是**（手动） |
| 知识库条目 | \knowledge/\ | 否（自动） |
| 博客草稿 | \drafts/\ | 否（自动） |

## 安全与隐私

### 永不存储
- 密码、API Key、Token
- 财务/医疗信息
- 第三方个人数据
- 位置模式

### 透明度
- \"你记住了什么？\" → 可完整导出
- 每条规则都有来源和时间戳
- \"忘记 X\" → 从所有记忆层删除

## 技术栈

- **运行时**: Node.js 18+
- **语言**: JavaScript (ESM)
- **数据格式**: Markdown, YAML, JSONL
- **无外部依赖** - 仅使用 Node.js 内置模块

## 贡献

参见 [CONTRIBUTING.md](CONTRIBUTING.md)。

## 许可证

MIT License - 可自由使用、修改和分发。

## 致谢

为 OpenClaw Agent 团队构建。灵感来自于 AI Agent 需要真正随时间进化，而不仅仅是积累聊天历史。