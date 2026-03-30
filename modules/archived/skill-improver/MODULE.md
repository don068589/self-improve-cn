# skill-improver

> 版本：3.0.0
> 状态：archived（功能已合并）

## 职责
从反馈中识别技能改进需求，写入 themes/ 沉淀。

## 输入
- `data/feedback/*.jsonl` — 包含 skill 字段的反馈记录

## 输出
- `data/themes/skill-improvements/{skill名}.md` — 技能改进建议

## 执行规则
1. 从 feedback 中提取 skill 字段
2. 统计每个 skill 的低分记录
3. 低分 ≥3 次 → 写入 themes/skill-improvements/
4. 等待 memory-layer 沉淀到 hot.md
5. 由 proposer 提取固化建议

## 依赖
- feedback-collector — 需要 feedback 数据
- memory-layer — 需要 themes/ 和 hot.md
- proposer — 最终生成 PENDING.md