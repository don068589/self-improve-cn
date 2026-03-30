# Self-Improve 系统说明书

> 版本：2.2.0
> 这是团队的自我进化操作系统。任何 agent 读这个文件就知道一切。

---

## 这是什么

一个可插拔的模块化自我改进框架。agent 从错误、纠正、反馈中学习，积累可复用的经验规则，持续提升执行质量。定时（每 3 天）自动运行，只有固化到系统文件时才需要 用户确认。

---

## 核心原则

1. **系统文件修改走审批** — 见 config.yaml 中的 `approval_required` 列表
2. **证据优先** — 不从沉默推断，3 次重复才固化
3. **全团队共享** — 一个 agent 学到的，所有 agent 受益
4. **可无限扩展** — 加模块 = 加目录 + 写 MODULE.md + 注册
5. **定时触发** — 每 3 天自动运行，不在对话中触发
6. **全自动处理** — 收集、分类、提炼全自动
7. **自己判断格式** — 语言模型自己决定如何组织语言、加入哪个章节或新建章节
8. **容错继续** — 某步骤执行失败时，记录错误后继续后续步骤，不中断整个流程

### 运行时原则

9. **逐步递进** — 每个步骤只加载上一步产出，不回头
10. **交班记录** — 每步完成后写 checkpoint，可随时恢复
11. **上下文控制** — 语料提取完释放，不一直占着
12. **高价值回炉** — 发现高价值时标记，后续精读提炼
13. **多产出通道** — 规则固化、技能改进、博客文章、方法论等

---

## 目录结构

```
{安装目录}/
│
├── ENGINE.md              # 动力机制（触发规则、依赖关系、分类映射）
├── SYSTEM.md              # 本文件
├── RUNTIME.md             # 运行时机制（逐步递进、交班记录、恢复流程）
├── config.yaml            # 模块注册表 + 开关 + 审批规则
├── user-config.yaml       # 用户配置（路径、时区等）
├── checkpoint.json        # 交班记录（当前运行状态）
├── changelog.md           # 系统升级日志
│
├── modules/               # 可插拔模块（每个有 MODULE.md）
│   ├── feedback-collector/   # 收集反馈
│   ├── distill-classifier/   # 提炼 + 分类
│   ├── memory-layer/         # 分层记忆管理
│   ├── proposer/             # 固化建议
│   ├── profiler/             # 能力画像
│   ├── reflector/            # 自反思
│   └── notify/               # 通知
│
├── data/                  # 所有模块的共享数据
│   ├── hot.md                # HOT 层：≤100 行活跃规则
│   ├── high-value/           # 高价值项记录
│   ├── corrections.md        # 纠正日志（最近 50 条）
│   ├── reflections.md        # 自反思日志
│   ├── profile.md            # 团队能力画像
│   │
│   ├── feedback/             # 结构化反馈 YYYY-MM-DD.jsonl
│   ├── themes/               # 主题分类
│   │   ├── behavior/         # 行为规范
│   │   ├── communication/    # 沟通偏好
│   │   ├── tools/            # 工具使用
│   │   └── ...               # 其他主题
│   │
│   ├── backup/               # 运行前备份
│   └── archive/              # 冷存储
│
├── proposals/             # 审批队列
│   └── PENDING.md            # 待确认的修改建议
│
├── drafts/                # 博客草稿
│
└── scripts/               # CLI 工具
    ├── setup.mjs             # 一键安装
    ├── run-all.mjs           # 执行全部模块
    ├── module.mjs            # 模块管理
    └── ...
```

---

## 怎么用

### 日常使用（全自动）

每 3 天凌晨 4 点（可配置），Cron 自动触发核心流程：

```
0. 备份               → 复制关键文件到 data/backup/
1. 扫描语料           → feedback-collector 从 memory 日志提取信号
2. 提炼与分类         → distill-classifier 三层次提炼 + 分类
3. 记忆升降           → memory-layer 更新 hot.md
4. 判断产出           → proposer 判断去向
5. 高价值回炉（可选） → 精读原始语料，深度提炼
6. 收尾               → 自反思、团队画像、通知
```

**权威流程定义在 `prompts/cron-trigger.md`。**

### 日常工作中（被动识别）

agent 在正常工作时识别学习信号，记录到 data/ 中：

| 事件 | 动作 |
|------|------|
| 用户纠正了你 | 记录到 corrections.md |
| 用户表扬了你 | 记录正面反馈 |
| 完成重要任务 | 写自反思 |
| 发现可复用规则 | 记录到 corrections.md |
| 规则涉及系统文件 | 写入 PENDING.md |

### 主动查询

| 问法 | 动作 |
|------|------|
| "你学到了什么" | 展示最近 corrections + reflections |
| "看看改进建议" | 展示 proposals/PENDING.md |
| "改进统计" | 运行 scripts/status.mjs |
| "忘掉 X" | 从所有层级删除（确认后）|

### 手动触发（可选）

```bash
# 执行全部模块
node scripts/run-all.mjs

# 单独执行某个模块
node scripts/feedback.mjs log
node scripts/status.mjs
```

---

## 模块系统

### 模块标准

每个模块是 `modules/` 下的一个目录，必须包含 `MODULE.md`。

### 核心模块

| 模块 | 职责 | 输出 |
|------|------|------|
| feedback-collector | 扫描对话，提取信号 | data/feedback/*.jsonl |
| distill-classifier | 三层次提炼 + 分类 | data/themes/ |
| memory-layer | 分层记忆管理 | data/hot.md |
| proposer | 判断产出形式 | proposals/PENDING.md, drafts/ |
| reflector | 自反思 | data/reflections.md |
| profiler | 团队能力画像 | data/profile.md |
| notify | 通知用户 | 通过 message 工具 |

### 添加新模块

1. 在 `modules/` 下创建新目录
2. 写 `MODULE.md`
3. 在 `config.yaml` 的 `modules` 中注册
4. 在 `changelog.md` 记录

或用命令：
```bash
node scripts/module.mjs add 新模块名
```

---

## 审批机制

`config.yaml` 中定义了 `approval_required` 列表。列表中的文件，修改前必须：

1. 写入 `proposals/PENDING.md`
2. 告知用户
3. 用户确认后执行
4. 标记已完成

### 需要确认的文件

AGENTS.md、TOOLS.md、MEMORY.md、SOUL.md、HEARTBEAT.md、IDENTITY.md、openclaw.json、SKILL.md

### 自动执行的文件

- `data/*` — 改进系统自己的数据
- `{knowledge_root}/*` — 自动提炼的知识

---

## 数据分层

| 层级 | 位置 | 大小限制 | 行为 |
|------|------|---------|------|
| HOT | data/hot.md | ≤100 行 | Cron 提炼后写入 |
| WARM | data/themes/ | 每文件 ≤200 行 | 按上下文按需加载 |
| COLD | data/archive/ | 无限制 | 仅显式查询时加载 |

升降级规则：
- 应用 3 次/7 天 → 升 HOT
- 30 天未用 → 降 WARM
- 90 天未用 → 归档 COLD
- 永不自动删除

---

## 学习信号

### 触发学习

| 信号 | 置信度 | 动作 |
|------|--------|------|
| 用户明确纠正 | 高 | 立即记录 corrections.md |
| 用户重复说过 | 高 | 标记重复，提升优先级 |
| 用户明确偏好 | 确认 | 直接写入 HOT |
| 同一纠正 3 次 | 确认 | 生成固化建议 |
| 任务失败/报错 | 高 | corrections.md + 根因分析 |
| 用户表扬 | 正面 | 记录成功案例 +1 |

### 不触发学习

- 沉默
- 单次指令
- 假设性讨论
- 第三方偏好
- 群聊模式（除非 用户确认）

---

## 安全边界

### 绝不存储
密码、API Key、Token、财务信息、医疗信息、第三方个人信息、位置模式

### 透明性
- "你记了什么" → 完整导出
- 每条规则标注来源和时间
- "忘掉 X" → 从所有层级删除

---

## 与现有体系的关系

```
memory/YYYY-MM-DD.md      → 事实记录（发生了什么）
MEMORY.md                 → 长期记忆（重要的人、事、决策）
{knowledge_root}/         → 自动提炼的知识
self-improve/             → 执行改进（怎么做得更好）
```

四者互补，不重叠，不替代。

---

## 安装

```bash
# 1. 配置路径
cp user-config.yaml my-config.yaml
# 编辑 my-config.yaml

# 2. 运行安装
node scripts/setup.mjs --config my-config.yaml

# 3. 查看 Cron 建议
cat proposals/PENDING.md

# 4. 将 Cron 任务添加到 OpenClaw 配置
```

---

## 新 agent 接入

**自动化，无需手动配置。**

Cron 执行时自动扫描 `{workspace_root}` 下所有 agent 的 memory 目录。新建 agent 自动被扫描。

### 让新 agent 知道系统存在

在新 agent 的 AGENTS.md 加入：

```markdown
## 自我改进

本团队使用 self-improve 系统持续提升执行质量。

### 日常工作中
- 用户纠正你时 → 记录到 corrections.md
- 完成重要任务后 → 写自反思到 reflections.md
- 发现可复用规则 → 记录到 corrections.md
- 规则涉及系统文件修改 → 写入 proposals/PENDING.md
```

---

## 不是 Skill

self-improve 不是 OpenClaw skill，不放在 `~/.openclaw/skills/`。

原因：
- Skill 是单 agent 用的，self-improve 是全团队共享
- Skill 靠 session 启动时匹配触发，self-improve 靠 Cron 定时
- self-improve 是独立安装的框架
