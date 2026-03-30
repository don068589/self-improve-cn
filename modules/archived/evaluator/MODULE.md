# evaluator

> 版本：1.0.0
> 状态：archived（功能已合并）

## 职责
评估任务质量，统计 agent 表现。

## 输入
- `data/feedback/*.jsonl` — feedback-collector 产出的反馈数据

## 输出
- `data/feedback/*.jsonl` — 追加评估分数

## 执行规则
1. 读取 feedback 文件
2. 统计各 agent 的正面/负面比例
3. 计算成功率
4. 追加评估结果到 feedback 文件

## 依赖
- feedback-collector — 需要 feedback 数据