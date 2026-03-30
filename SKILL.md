# SKILL.md - Self-Improvement 自我改进框架

一个可插拔的 AI Agent 自我改进框架。自动从错误、纠正和反馈中学习，持续提升执行质量。

## Description

Self-Improve 让你的 agent 团队持续进化：
- 扫描 agent 记忆日志，识别学习信号
- 提炼可复用的经验规则
- 提议改进系统文件（带审批流程）
- 维护三层记忆系统（HOT/WARM/COLD）

## When to Use

- **自动运行（Cron）**：默认每 3 天运行一次
- **手动触发**：用户说"运行自我改进"、"学习改进"
- **重大事件后**：用户可在重大纠正后请求立即运行

## Installation

```bash
# 克隆到你指定的位置
git clone https://github.com/openclaw/self-improve.git
cd self-improve

# 用你的配置运行安装
node scripts/setup.mjs --config user-config.yaml

# 将生成的 Cron 任务添加到 OpenClaw
# 安装后查看 proposals/PENDING.md
```

## Quick Start

### 1. 配置路径

编辑 `user-config.yaml`：
```yaml
storage:
  root: "/path/to/self-improve"      # 安装路径
  knowledge_root: "/path/to/learned" # 知识输出路径
  workspace_root: "/path/to/.openclaw"  # OpenClaw workspace

owner:
  name: "你的名字"
  timezone: "Asia/Shanghai"

agent:
  main_agent: "main"  # 主 agent ID
```

### 2. 运行安装

```bash
node scripts/setup.mjs --config user-config.yaml
```

安装脚本会：
1. 创建目录结构
2. 初始化数据文件
3. 生成 Cron 任务提议

### 3. 添加 Cron 任务

检查 `proposals/PENDING.md`，将提议的 Cron 任务添加到 OpenClaw 配置。

### 4. 验证

```bash
node scripts/status.mjs
```

## How It Works

```
扫描日志 → 提炼规则 → 分类归档 → 记忆升降 → 提议改进
```

### 记忆层级

| 层级 | 位置 | 限制 | 用途 |
|------|------|------|------|
| HOT | `data/hot.md` | ≤100 行 | 固化前加载 |
| WARM | `data/themes/` | 每文件 ≤200 行 | 按需加载 |
| COLD | `data/archive/` | 无限制 | 显式查询 |

## Manual Run

```bash
# 立即运行（不等待 Cron）
node scripts/run-all.mjs
```

## Directory Structure

```
self-improve/
├── SKILL.md              # 本文件
├── SYSTEM.md             # 系统文档
├── ENGINE.md             # 动力机制
├── RUNTIME.md            # 运行时机制
├── config.yaml           # 模块配置
├── user-config.yaml      # 用户配置模板
├── modules/              # 模块目录
├── prompts/              # 执行指令
├── scripts/              # CLI 工具
└── data/                 # 数据目录
```

## Safety

### 绝不存储
- 密码、API Key、Token
- 财务/医疗信息
- 第三方个人数据
- 位置模式

### 透明性
- "你记了什么" → 完整导出
- 每条规则有来源和时间戳
- "忘掉 X" → 从所有层级删除

## License

MIT License

## Links

- [GitHub](https://github.com/openclaw/self-improve)
- [Documentation](./SYSTEM.md)