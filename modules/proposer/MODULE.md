# proposer

> 版本：1.2.0
> 状态：active

## 职责
通盘考虑，判断产出通道，生成固化建议或深度产出。

## 详细执行指南

**执行时读取**：`prompts/proposer.md`

包含：
- 价值优先判断方法
- 一条内容多角度利用
- 元能力引导
- 产出通道分流规则
- 高价值标记标准
- PENDING.md 格式规范

## 输入
- `data/themes/` — 主要来源
- `data/hot.md` — 规则固化专项
- `data/errors/` — 回炉检查
- `data/lessons/` — 回炉检查

## 输出
- `proposals/PENDING.md` — 待审批建议
- `drafts/` — 博客草稿
- `/path/to/learned/` — 方法论等
- `data/errors/` — 错误知识点
- `data/lessons/` — 经验教训
- `data/high-value/` — 高价值标记

## 依赖
- distill-classifier — 需要 themes/
- memory-layer — 需要 hot.md