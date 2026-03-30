# notify

> 版本：1.0.0
> 状态：active

## 职责

通知用户有待审批的建议。

## 详细执行指南

**执行时读取**：`prompts/notify.md`

包含：
- 通知格式规范
- 渠道配置

## 输入

- `proposals/PENDING.md` — 待审批建议列表
- `config.yaml` — 通知配置

## 输出

- 通知消息（通过配置的渠道发送给用户）

## 执行规则

1. 检查 PENDING.md 中是否有待审批建议
2. 如有，按配置的渠道通知用户
3. 通知内容包括建议标题和链接

## 依赖

- proposer — 产出 PENDING.md