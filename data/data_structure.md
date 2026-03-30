# data/ 目录结构

> Self-improve 系统中间数据

## 目录

| 目录/文件 | 内容 | 有索引 |
|------|------|--------|
| themes/ | 分类沉淀的内容 | ✅ 各主题有索引 |
| feedback/ | 结构化反馈记录 | ✅ 有索引 |
| archive/ | 归档数据 | ✅ 有索引 |
| backup/ | 备份数据（Cron 快照）| — 自动清理 |
| notification-log.jsonl | 通知历史记录 | — 单文件 |
| hot.md | 活跃规则（HOT 层）| — 单文件 |
| corrections.md | 纠正日志 | — 单文件 |
| reflections.md | 自反思日志 | — 单文件 |
| profile.md | Agent 能力画像 | — 单文件 |

## themes/

按主题分类的内容，每条含 frontmatter 标签。

| 主题 | 说明 |
|------|------|
| behavior | 行为规范 |
| communication | 沟通方式 |
| tools | 工具使用 |
| coding | 编码相关 |
| search | 搜索策略 |
| writing | 写作风格 |
| collaboration | 团队协作 |
| preferences | 个人偏好 |
| professional | 专业能力 |
| personality | 人格特质 |

---

> 由 self-improve 系统自动维护