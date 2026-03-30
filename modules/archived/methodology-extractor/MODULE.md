# methodology-extractor

> 版本：1.0.0
> 状态：archived（功能已合并）

## 职责
从反思和高分记录中提炼方法论。

## 输入
- `data/reflections.md` — 所有 agent 的自反思
- `data/feedback/*.jsonl` — 高分记录（score: +1）

## 输出
- `/path/to/learned/methodologies\{主题}.md` — 方法论文档

## 执行规则
1. 通读 reflections.md 和高分 feedback
2. 发现反复出现的成功模式
3. 提炼为可复用的方法论
4. 写入 methodologies/
5. 更新索引

## 依赖
- feedback-collector — 需要 reflections.md 和 feedback/
- knowledge-archiver — 共享输出目录