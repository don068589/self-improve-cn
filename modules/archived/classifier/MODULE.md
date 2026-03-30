# classifier

> 版本：3.0.0
> 状态：archived（功能已合并）

## 职责
分类并打标签，写入 themes/，统计 occurrences。

## 输入
- `data/corrections.md` — 纠正记录
- `data/feedback/*.jsonl` — 反馈数据（含多维度字段）

## 输出
- `data/themes/{主题}/*.md` — 按主题存储，含 frontmatter 标签

## 执行规则
1. 读取 corrections.md 和 feedback/
2. 判断主题/领域/项目标签
3. 检查 themes/ 是否已存在相同内容
4. 已存在 → occurrences +1，更新 last_seen
5. 不存在 → 新建文件，occurrences = 1
6. 更新主题索引

## 依赖
- feedback-collector — 需要 corrections.md 和 feedback/