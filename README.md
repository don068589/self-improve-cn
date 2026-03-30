# Self-Improve：Agent 进化系统

> 一个可插拔、模块化的 AI Agent 自我改进框架。

## 这是什么？

Self-Improve 让 AI Agent 从错误、纠正和反馈中学习，积累可复用的经验规则，持续提升执行质量。

**核心特性：**
- 🔄 **自动学习**：定时运行（默认每 3 天）
- 📦 **模块架构**：按需添加/移除模块
- 🧠 **多层记忆**：HOT/WARM/COLD 三层记忆
- ✅ **审批工作流**：系统文件修改需明确审批
- 🔌 **OpenClaw 原生**：为 OpenClaw Agent 团队设计

## 安装

### 方式 1：从 GitHub 克隆（推荐）

```bash
git clone https://github.com/openclaw/self-improve.git
cd self-improve

# 复制并编辑配置
cp user-config.yaml my-config.yaml
# 编辑 my-config.yaml，填写你的路径

# 运行安装
node scripts/setup.mjs --config my-config.yaml
```

### 方式 2：下载 ZIP

从 [GitHub Releases](https://github.com/openclaw/self-improve/releases) 下载，解压后：

```bash
cd self-improve
cp user-config.yaml my-config.yaml
# 编辑 my-config.yaml
node scripts/setup.mjs --config my-config.yaml
```

### 验证安装

```bash
node scripts/status.mjs
```

## 快速开始

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

### 安装后

1. 查看 `proposals/PENDING.md` 中的 Cron 任务建议
2. 在 OpenClaw 配置中批准该 Cron 任务
3. 系统将自动每 3 天运行一次

## 工作原理

```
扫描日志 → 提炼规则 → 分类归档
    ↓
记忆升降（HOT/WARM/COLD）
    ↓
判断产出：
  → 系统文件修改（需审批）
  → 知识库
  → 博客草稿
```

## 记忆层级

| 层级 | 位置 | 限制 | 用途 |
|------|------|------|------|
| HOT | `data/hot.md` | ≤100 行 | 固化前加载 |
| WARM | `data/themes/` | 每文件 ≤200 行 | 按需加载 |
| COLD | `data/archive/` | 无限制 | 显式查询 |

## 目录结构

```
self-improve/
├── SKILL.md              # OpenClaw Skill 定义
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

## 手动运行

```bash
node scripts/run-all.mjs
```

## 安全

### 绝不存储
- 密码、API Key、Token
- 财务/医疗信息
- 第三方个人数据
- 位置模式

### 透明性
- "你记了什么" → 完整导出
- 每条规则有来源和时间戳
- "忘掉 X" → 从所有层级删除

## 许可证

MIT License

## 致谢

为 OpenClaw Agent 团队构建。灵感来源：让 Agent 真正随时间改进的需求。
