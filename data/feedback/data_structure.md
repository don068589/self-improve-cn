# feedback/ 目录结构

> 结构化反馈记录（JSONL 格式）

## 文件

| 文件 | 说明 |
|------|------|
| YYYY-MM-DD.jsonl | 当日反馈记录 |

（安装后自动生成）

## JSONL 格式

```json
{
  "ts": "ISO时间",
  "agent": "agent名",
  "type": "explicit/insight/innovation/business/synthesis",
  "theme": "主题",
  "domain": "领域",
  "project": "项目",
  "skill": "技能",
  "score": "-1/0/+1/+2",
  "hint": "内容摘要"
}
```

---

> 由 feedback-collector 模块自动维护