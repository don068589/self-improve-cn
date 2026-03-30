# memory-layer

> 版本：3.2.0
> 状态：active

## 职责
管理分层记忆（HOT/WARM/COLD），维护 hot.md，处理升降级。

## 详细执行指南

**执行时读取**：`prompts/memory-layer.md`

包含：
- 升级到 HOT 的规则（occurrences ≥ 3）
- 降级规则（last_seen > 30 天）
- 归档规则（last_seen > 90 天）
- hot.md 格式规范

## 输入
- `data/themes/{主题}/*.md` — distill-classifier 写入的分类内容

## 输出
- `data/hot.md` — HOT 层，occurrences ≥ 3 的规则
- `data/archive/` — COLD 层

## 依赖
- distill-classifier — 需要 themes/ 数据