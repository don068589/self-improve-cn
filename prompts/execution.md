# 审批执行 Prompt

> 当 用户 批准 PENDING.md 中的建议时执行

---

```
用户 在对话中批准了改进建议，你需要执行写入。

## 输入

- `proposals/PENDING.md` — 待审批建议（用户 已批准的那条）
- 目标文件（AGENTS.md / TOOLS.md / MEMORY.md / SOUL.md / HEARTBEAT.md 等）

## 输出

- 目标文件 — 已更新
- `proposals/PENDING.md` — 状态更新为"已批准"
- `data/archive/proposals-用户e.md` — 归档记录
- `run-log.jsonl` — 执行日志

---

## 触发条件

用户 说：
- "批准 P-xxx"
- "批准全部"
- "通过 P-xxx"
- "执行 P-xxx"

## 执行步骤

### 1. 定位建议

读取 `/path/to/self-improve/proposals\PENDING.md`，找到对应的建议。

### 2. 读取目标文件

根据建议中的 **目标文件** 读取目标文件。

### 3. 执行修改

按建议内容修改目标文件：
- 如果是"追加内容" → 追加到合适位置
- 如果是"修改内容" → 替换对应部分
- 用目标文件的语言风格写

**写入质量要求：**
1. **融入而非插入** — 写入的内容要像原本就在文件里一样自然
2. **不破坏结构** — 保持目标文件的整体结构完整
3. **不重复** — 如果已有类似规则，合并而非新增
4. **最小改动** — 只改需要改的地方，不动其他内容

如果建议的"建议适用"是特定 agent（非全团队），写入时在章节标题后加 `(xxx专属)` 标记。

### 4. 标记完成

在 PENDING.md 中更新状态：

```markdown
- **状态:** ✅ 已批准（YYYY-MM-DD HH:MM）
- **执行者:** {当前 agent}
```

### 5. 归档

移到 `/path/to/self-improve/data\archive\proposals-用户e.md`（追加）。

### 6. 记录

在 `/path/to/self-improve/run-log.jsonl` 追加：

```json
{"ts":"ISO时间","action":"approval_executed","proposal_id":"P-xxx","target_file":"目标文件","executor":"agent名"}
```

## 拒绝处理

用户 说"拒绝 P-xxx"时：

1. 在 PENDING.md 中标记：
   ```markdown
   - **状态:** ❌ 已拒绝（YYYY-MM-DD HH:MM）
   - **拒绝原因:** {用户 说的原因，如果有}
   ```

2. 移到 `data/archive/proposals-用户e.md`

## 目标文件处理规则

| 目标文件 | 处理方式 |
|---------|---------|
| AGENTS.md | 追加到合适章节，或新建章节 |
| TOOLS.md | 追加到合适章节 |
| SOUL.md | 追加到合适章节 |
| MEMORY.md | 追加到合适章节 |
| HEARTBEAT.md | 追加任务项 |
| openclaw.json | 修改对应配置 |
| SKILL.md | 追加内容到 SKILL.md |

**注意：**
- 先读取目标文件，理解现有结构
- 用目标文件的风格写
- 不破坏现有内容
```