# 安装指南

## 前置要求

- **OpenClaw** 已安装并配置
- **Node.js** 18 或更高版本
- 已配置主 agent

## 第一步：下载

### 方式 A：从 GitHub 克隆

```bash
git clone https://github.com/openclaw/self-improve.git
cd self-improve
```

### 方式 B：下载 ZIP

从 [GitHub Releases](https://github.com/openclaw/self-improve/releases) 下载并解压。

## 第二步：配置

1. 复制配置模板：
   ```bash
   cp user-config.yaml my-config.yaml
   ```

2. 编辑 `my-config.yaml`：
   ```yaml
   storage:
     root: "/home/user/self-improve"      # 安装路径
     knowledge_root: "/home/user/learned" # 知识输出路径
     workspace_root: "/home/user/.openclaw"  # OpenClaw workspace

   owner:
     name: "你的名字"
     timezone: "Asia/Shanghai"
   ```

## 第三步：运行安装

```bash
node scripts/setup.mjs --config my-config.yaml
```

安装脚本会：
1. 创建目录结构
2. 初始化数据文件
3. 生成 Cron 任务提议

## 第四步：添加 Cron 任务

安装后检查 `proposals/PENDING.md`：

```bash
cat proposals/PENDING.md
```

你会看到一个提议的 Cron 任务。将其添加到 OpenClaw 配置：

**方式 A：直接编辑 `openclaw.json`**

在 `cron` 部分添加：
```json
{
  "name": "self-improve",
  "schedule": {
    "kind": "cron",
    "expr": "0 4 */3 * *",
    "tz": "Asia/Shanghai"
  },
  "sessionTarget": "isolated",
  "payload": {
    "kind": "agentTurn",
    "message": "运行自我改进...",
    "model": "bailian/qwen3.5-plus"
  }
}
```

**方式 B：使用 OpenClaw CLI**

```bash
openclaw cron add --from proposals/PENDING.md
```

## 第五步：验证

检查系统状态：
```bash
node scripts/status.mjs
```

## 手动运行（可选）

无需等待 Cron，立即运行：
```bash
node scripts/run-all.mjs
```

## 更新

```bash
git pull
node scripts/setup.mjs  # 重新运行以更新结构
```

## 卸载

1. 从 OpenClaw 配置删除 Cron 任务
2. 删除安装目录
3. （可选）归档或删除 `knowledge_root` 数据

## 常见问题

### "Permission denied"
确保你对所有配置路径有写入权限。

### "Module not found"
运行 `node scripts/setup.mjs` 确保所有模块已复制。

### "没有收集到反馈"
系统扫描 agent 记忆日志。确保 agent 正在写入 `memory/*.md`。

## 下一步

- 阅读 [SYSTEM.md](SYSTEM.md) 了解完整文档
- 阅读 [ENGINE.md](ENGINE.md) 了解触发规则
- 在 `modules/` 自定义模块