# 团队能力画像指令 Prompt

> profiler 模块使用，统计各 agent 表现，更新能力画像

---

```
你需要统计各 agent 的表现，更新团队能力画像。

## 输入

- `data/feedback/*.jsonl` — 反馈数据（含 agent, score 字段）

## 输出

- `data/profile.md` — 各 agent 能力画像

## 执行条件

每 3 次运行执行一次。

---

## 统计规则

### 标记"擅长"

条件：成功率 ≥ 90% 且 ≥ 10 次

### 标记"待加强"

条件：成功率 < 70% 且 ≥ 5 次

### 成功率计算

- 正面信号：score = +1 或 +2
- 负面信号：score = -1
- 成功率 = 正面次数 / 总次数

## 维度分析

按以下维度统计：
- skill — 涉及的技能
- theme — 主题领域
- domain — 技术领域

## 执行步骤

1. 读取 `data/feedback/*.jsonl`
2. 按 agent 分组统计
3. 计算成功率
4. 按维度分析
5. 更新 `data/profile.md`

## 输出格式

```markdown
# 团队能力画像

> 更新时间：{timestamp}
> 统计周期：最近 {N} 次运行

## {agent-name}

### 擅长
- {theme/domain/skill}: 成功率 {X}%

### 待加强
- {theme/domain/skill}: 成功率 {X}%

### 统计
- 总任务：{N}
- 成功：{N}
- 失败：{N}
```

## 注意

profile.md 目前是"死端"——被写入但没有被其他模块读取。
后续可接入 proposer，用于判断某 agent 是否适合某任务。
```