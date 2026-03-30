# feedback-collector

> 版本：1.1.0
> 状态：active

## 职责
扫描 agent 对话日志，提取纠正、表扬、洞察、创新等信号。

## 详细执行指南

**执行时读取**：`prompts/feedback-collector.md`

包含：
- 语义理解式提取方法
- 信号类型识别规则
- 多维度标签判断
- summary/detail 分层格式
- 闭环输入（reflections.md）

## 输入
- `~/.openclaw/agents/*/memory/YYYY-MM-DD.md` — 最近 3 天的对话日志
- `data/reflections.md` — 上轮反思（闭环输入）

## 输出
- `data/feedback/YYYY-MM-DD.jsonl` — 结构化反馈记录

## 依赖
无（基础模块）