# profiler

> 版本：1.2.0
> 状态：active

## 职责
更新团队能力画像（每 3 次运行执行一次）。

## 详细执行指南

**执行时读取**：`prompts/profiler.md`

包含：
- 成功率计算方法
- "擅长"/"待加强"判断规则
- 维度分析（skill/theme/domain）
- 输出格式规范

## 输入
- `data/feedback/*.jsonl` — 反馈数据

## 输出
- `data/profile.md` — 各 agent 能力画像

## 依赖
- feedback-collector — 需要 feedback 数据

## 注意

profile.md 目前是"死端"——被写入但没有被其他模块读取。
后续可接入 proposer，用于判断某 agent 是否适合某任务。