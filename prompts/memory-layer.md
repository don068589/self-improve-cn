# 记忆升降指令 Prompt

> memory-layer 模块使用，管理分层记忆（HOT/WARM/COLD）

---

```
你需要管理规则的分层记忆，维护 hot.md，处理升降级。

## 输入

- `data/themes/{主题}/*.md` — distill-classifier 写入的规则（含 frontmatter: occurrences, last_seen）

## 输出

- `data/hot.md` — HOT 层活跃规则（occurrences ≥ 3）
- `data/archive/` — COLD 层归档内容（last_seen > 90 天）

---

## 核心规则

### 升级到 HOT

条件：occurrences ≥ 3

执行：
1. 从 themes/ 中找到 occurrences ≥ 3 的规则
2. 写入 `data/hot.md`，格式如下：

```markdown
## [R-YYYYMMDD-NNN] {规则标题}

- 来源：themes/{主题}/{文件}.md
- 出现次数：{occurrences}
- 思维原则：{一句话}

{表面规则}
```

### 降级

条件：last_seen 超过 30 天

执行：
1. 从 hot.md 移除
2. 保留在 themes/（WARM 层）

### 归档

条件：last_seen 超过 90 天

执行：
1. 从 themes/ 移动到 `data/archive/themes/`

## hot.md 限制

- 最大 100 行
- 超过时移除最旧的规则（occurrences 最小或 last_seen 最早）

## 执行步骤

1. 扫描 `data/themes/` 下所有 .md 文件
2. 读取 frontmatter：occurrences、last_seen
3. 按 above 规则处理升级/降级/归档
4. 更新 hot.md
5. 写 checkpoint

## 输出

- `data/hot.md` — HOT 层活跃规则
- `data/archive/` — COLD 层归档内容
```