# Self-Improve 运行时机制

> 确保系统在上下文爆炸、宕机、压缩后能恢复并继续工作
> 核心原则：**逐步递进、交班记录、高价值回炉、多产出通道**

---

## 一、逐步递进流程

### 流程设计

```
┌─────────────────────────────────────────────────────────┐
│ 步骤1：扫描语料 → 提取信号 → 写入 feedback              │
├─────────────────────────────────────────────────────────┤
│ 步骤2：提炼与分类 → 三层次提炼 → 写入 themes            │
├─────────────────────────────────────────────────────────┤
│ 步骤3：记忆升降 → 管理 hot.md                           │
├─────────────────────────────────────────────────────────┤
│ 步骤4：判断产出 → 多通道分流                            │
├─────────────────────────────────────────────────────────┤
│ 步骤5：自反思 → 写 reflections.md                       │
├─────────────────────────────────────────────────────────┤
│ 步骤6：团队画像 → 更新 profile.md（每3次）              │
├─────────────────────────────────────────────────────────┤
│ 步骤7：通知 → 通知用户                                  │
└─────────────────────────────────────────────────────────┘
```

### 上下文控制原则

| 原则 | 说明 |
|------|------|
| 只加载上一步产出 | 不回头读原始语料（除非高价值回炉）|
| 索引优先 | 先读 data_structure.md，按需加载 |
| 每步写 checkpoint | 记录进度，可随时恢复 |

---

## 二、交班记录机制

### checkpoint.json 结构

```json
{
  "run_id": "YYYY-MM-DDTHH:MM:SS+TZ",
  "current_step": "classifier",
  "status": "in_progress",
  "completed_steps": [
    {"step": "scan", "status": "success", "output": "data/feedback/YYYY-MM-DD.jsonl", "count": 4},
    {"step": "evaluate", "status": "success", "output": null, "count": 4},
    {"step": "classify", "status": "in_progress", "output": null}
  ],
  "pending_steps": ["memory-layer", "output", "revisit"],
  "high_value_items": [
    {"source": "feedback#3", "reason": "可提炼博客：系统设计理念", "potential": ["blog", "methodology"]}
  ],
  "last_update": "YYYY-MM-DDTHH:MM:SS+TZ"
}
```

### 每步完成后

1. 更新 checkpoint.json
2. 写 run-log.jsonl 进度记录

---

## 三、冷启动恢复流程

### 恢复步骤

```
1. 读 checkpoint.json
   - 有未完成的运行？→ 继续执行 pending_steps
   - 没有？→ 开始新运行

2. 读上一步的 output 文件
   - 不需要从头开始
   - 只加载必要的数据

3. 继续执行
   - 从 current_step 开始
   - 不重复已完成的工作

4. 完成后清理
   - 标记 status: "completed"
   - 更新 config.yaml 的 last_success_ts
```

### 上下文大小估算

| 阶段 | 加载内容 | 大小 |
|------|---------|------|
| 恢复状态 | checkpoint.json | ~1KB |
| 步骤输入 | 上一步产出文件 | ~5-20KB |
| 当前执行 | 处理数据 | 动态 |

**总计：每次恢复只需 ~10-30KB，不会爆炸**

---

## 四、高价值回炉机制

### 识别高价值

**高价值的判断和标记发生在步骤5：判断产出。**

proposer 模块会通盘考虑 hot.md 和 themes/ 中的内容，根据以下标准标记 `high_value`：
- **重复次数高**：在 hot.md 中的规则，出现次数多
- **影响范围广**：适用于多个 agent 或系统层面
- **可提炼性强**：能够提炼为普适的方法论、博客文章
- **价值密度高**：信息量大，经过多轮对话凝练

并将标记为 `high_value` 的项写入 `data/high-value/`。

### 回炉流程

```
步骤5完成后：
  检查 high_value_items
    ↓
  有高价值项？
    ↓
  步骤6：回炉
    - 精读原始语料（这时才回头）
    - 深度提炼
    - 写入对应产出通道
```

**回炉是可选的**，只有发现高价值才执行。

---

## 五、多产出通道

### 产出形式判断

proposer 判断物料应该输出到哪个通道：

| 产出形式 | 目标位置 | 判断依据 |
|---------|---------|---------|
| 规则固化 | PENDING.md | 行为规范、重复出现 ≥3 次 |
| 技能改进 | skill改进建议 | 技能使用问题 |
| 博客文章 | drafts/blog-xxx.md | 有普世价值、可分享 |
| 方法论 | /path/to/learned/methodologies\ | 可复用的思维框架 |
| 知识点 | data/errors/ 或 data/lessons/ | 具体知识点 |
| 商业洞察 | /path/to/learned/business\ | 商业相关 |
| 系统改进 | PENDING.md (system-upgrade) | 系统设计问题 |

### 博客产出流程

```
1. proposer 判断"可成文"
2. 写入 drafts/blog-{主题}.md（草稿）
3. 后续可由 agent 润色
4. 发布到博客平台
```

### 草稿格式

```markdown
# {标题}

> 草稿状态：待润色
> 来源：self-improve 运行 {run_id}
> 产出时间：{时间}

## 核心观点

{提炼的核心观点}

## 正文

{文章主体}

## 待润色项

- [ ] 标题优化
- [ ] 开头吸引力
- [ ] 结尾升华
- [ ] 案例补充
```

---

## 六、数据流总览

```
原始语料（memory 日志）
    ↓ 扫描提取
feedback/*.jsonl
    ↓ 评估
评估结果
    ↓ 分类
themes/{主题}/
    ↓ 升降
hot.md
    ↓
    ↓ 步骤5：通盘考虑（themes/（主要）+ hot.md（规则固化专项）+ errors/ + lessons/）
    ↓
┌────────────┬────────────┬────────────┬────────────┐
↓            ↓            ↓            ↓            ↓
PENDING.md   methods/     drafts/      errors/      lessons/
(规则固化)   (方法论)     (博客)       (错误)       (教训)
                                       ↓            ↓
                                       └────────────┘
                                            ↓
                                    下轮 proposer 回炉检查
                                    发现共性 → 升级为规则/方法论
```

**闭环：errors/ 和 lessons/ 既是产出位置，也是下一轮的输入源。**

---

## 七、文件结构更新

```
/path/to/self-improve/
├── checkpoint.json          # 交班记录（新增）
├── run-log.jsonl            # 进度日志
├── config.yaml              # 配置（含 last_success_ts）
│
├── data/
│   ├── feedback/            # 步骤1产出
│   ├── themes/              # 步骤3产出
│   ├── hot.md               # 步骤4产出
│   ├── errors/              # 错误知识点（中间站，下轮回炉）
│   ├── lessons/             # 经验教训（中间站，下轮回炉）
│   └── high-value/          # 高价值项记录
│
├── proposals/
│   └── PENDING.md           # 规则固化建议
│
└── drafts/                  # 博客草稿
    └── blog-xxx.md

/path/to/learned/                  # 独立的产出目录（不在 self-improve 内部）
├── methodologies/
├── business/
├── innovations/
└── articles/
```

---

## 八、与现有机制的关系

| 现有机制 | 新机制 | 关系 |
|---------|--------|------|
| run-log.jsonl | checkpoint.json | run-log 记录历史，checkpoint 记录当前状态 |
| cron-trigger.md | 逐步递进流程 | 改造 cron-trigger，按步骤执行 |
| feedback-collector | 步骤1 | 不变，但完成后要写 checkpoint |
| proposer | 步骤5 | 扩展产出判断能力 |
| /path/to/learned/ | 产出通道之一 | 方法论、商业洞察、创新 |