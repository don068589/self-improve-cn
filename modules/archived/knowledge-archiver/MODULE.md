# knowledge-archiver

> 版本：1.2.0
> 状态：archived（功能已合并）

## 职责
处理无法固化的知识内容，存入外部知识库。

## 输入
- `data/corrections.md` — 错误知识点来源
- `data/reflections.md` — 经验教训来源

## 输出
- `/path/to/learned/errors\{主题}.md` — 错误知识点
- `/path/to/learned/lessons\{主题}.md` — 经验教训
- `/path/to/learned/methodologies\{主题}.md` — 方法论
- `/path/to/learned/business\{主题}.md` — 商业洞察
- `/path/to/learned/innovations\{主题}.md` — 创新点
- `/path/to/learned/articles\{主题}.md` — 长链条文章

## 执行规则
1. 判断内容是否满足固化条件（occurrences ≥ 3）
2. 不满足固化但有保留价值 → 写入 /path/to/learned/
3. 更新索引

## 依赖
- feedback-collector — 需要 corrections.md 和 reflections.md