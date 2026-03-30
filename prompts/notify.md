# 通知 Prompt

> Cron session 发现待审批建议后，通知 用户 的消息模板

---

## 输入

- `proposals/PENDING.md` — 待审批建议列表
- `user-config.yaml` — 获取通知配置

## 输出

- 通知消息（通过 message 工具发送）
- `data/notification-log.jsonl` — 通知记录（避免重复通知）

---

## 通知规则

### 有待审批建议时

用 message 工具发送，格式简洁：

**调用方式（从 user-config.yaml 读取配置）：**
```
message(
  action="send", 
  channel="{用户.notification.channel}", 
  to="{用户.notification.to}", 
  message="🧠 Self-Improve：发现 N 条改进建议待审批..."
)
```

**消息内容：**
```
🧠 Self-Improve：发现 {N} 条改进建议待审批。

{逐条简要列出标题}

查看详情：{storage.root}/proposals/PENDING.md
跟任意 agent 说"看看改进建议"即可。
```

**注意：** 不要传递 poll 相关字段，这些字段会触发投票功能而非普通消息。

### 没有待审批建议时

不发消息，不打扰。

### 通知频率

- 同一条建议只通知一次
- 如果 用户 已经被通知过但还没处理，不重复通知
- 检查方法：读取 `data/notification-log.jsonl` 查看已通知的建议

### 通知后记录

追加到 `data/notification-log.jsonl`：
```json
{"ts":"YYYY-MM-DDTHH:MM:SS+TZ","proposal_id":"P-xxx","notified":true}
```

---

## 配置示例

user-config.yaml 中的通知配置：
```yaml
用户:
  notification:
    channel: "telegram"      # 或 "discord", "slack" 等
    to: "your-user-id"       # 你的用户 ID
```

如果未配置通知，跳过通知步骤，只在 run-log.jsonl 中记录。
