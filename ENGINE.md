# Self-Improve 引擎

> 定义触发规则、执行流程、层级创建规则

## 触发机制

### 定时触发（每 3 天）

```
Cron: 0 4 */3 * *（每 3 天凌晨 4 点）
模型: bailian/qwen3.5-plus
Agent: {main_agent}
时区: Asia/Shanghai
```

**Cron message：**

见 `prompts/cron-trigger.md`（完整的执行指令）。

安装时 setup.mjs 会将此 prompt 写入 Cron 配置的 message 字段。

**执行流程：**

```
0. 备份              → 复制关键文件到 data/backup/（纯文件操作，不依赖 git）
1. 扫描语料          → feedback-collector 从 memory 日志 + 上轮 reflections.md 提取信号
2. 评估              → 统计正负比例，识别重复模式
2.5 提炼与分类       → distill-classifier 三层次提炼 + 分类沉淀 + value_density 标注
3. 记忆升降          → memory-layer 管理 hot.md（≥3 次升级）
4. 判断产出          → proposer 通盘考虑，产出规则固化/博客/方法论/技能改进
5. 高价值回炉        → 精读原始语料，深度产出（可选）
6. 收尾              → 反思 + 画像 + 通知 + checkpoint
```

---

## 审批规则

**只有固化到系统文件时需要 用户确认。**

### 需要确认的文件

| 文件 | 内容类型 |
|------|---------|
| AGENTS.md | 行为规范、工作流程 |
| TOOLS.md | 工具使用偏好 |
| MEMORY.md | 人际关系、重要偏好 |
| SOUL.md | 人格特质、回复风格 |
| HEARTBEAT.md | 定时任务入口 |
| openclaw.json | 系统配置（Cron 等）|
| SKILL.md | 技能定义（所有技能文件）|

### 自动执行的文件

- `/path/to/self-improve/data/*` - 改进系统自己的数据
- `/path/to/learned/*` - 自动提炼的知识（knowledge-archiver 写入）

---

## 层级创建规则

**agent 可以自动创建新目录/文件，无需提前问。**

### themes/ 目录（可扩展）

现有主题：
- `behavior/` - 行为规范
- `communication/` - 沟通偏好
- `tools/` - 工具使用
- `coding/` - 编码规范
- `search/` - 搜索策略
- `writing/` - 写作风格
- `collaboration/` - 团队协作
- `preferences/` - 个人偏好
- `professional/` - 专业能力
- `personality/` - 人格特质

**发现新主题时自动创建：**
```
classifier 判断 → 不属于现有主题 → 创建 themes/{新主题}/
```

### projects/ 目录（可扩展）

```
发现新项目经验 → 创建 projects/{项目名}/
```

### 知识库目录（knowledge-archiver 写入）

```
错误/踩坑 → /path/to/learned/errors\{主题}.md
经验总结 → /path/to/learned/lessons\{主题}.md
方法论 → /path/to/learned/methodologies\{主题}.md
```

---

## 分类映射

| 主题 | 固化目标文件 |
|------|-------------|
| behavior | AGENTS.md |
| communication | AGENTS.md / SOUL.md |
| tools | TOOLS.md |
| coding | AGENTS.md / TOOLS.md |
| search | AGENTS.md / TOOLS.md |
| writing | AGENTS.md / SOUL.md |
| collaboration | AGENTS.md / MEMORY.md |
| preferences | MEMORY.md / SOUL.md |
| professional | AGENTS.md / TOOLS.md |
| personality | SOUL.md |
| errors | /path/to/learned/errors\ |
| skills | /path/to/self-improve/data\skills\ |

---

## 执行原则

1. **全自动处理** — 从收集到提炼，无需人工干预
2. **只有固化需确认** — 写入 AGENTS.md 等系统文件才需要 用户确认
3. **自己判断格式** — 语言模型自己决定如何组织语言、加入哪个章节
4. **可以新建** — 发现新主题/项目，直接创建目录
5. **索引自动更新** — 写入后更新相关索引文件
6. **容错继续** — 某步骤执行失败时，记录错误后继续后续步骤，最终 status 标记为 "partial"

---

## 模块依赖关系

```
feedback-collector（无依赖）
      ↓
evaluator（依赖 feedback-collector）
      ↓
classifier（无依赖）
      ↓
memory-layer（无依赖）
      ↓
knowledge-archiver（依赖 memory-layer）
methodology-extractor（依赖 memory-layer）
skill-improver（依赖 evaluator）
      ↓
proposer（依赖 memory-layer）
profiler（依赖 evaluator）
reflector（无依赖）
```

**执行顺序：** 按依赖顺序执行，确保上游模块先完成。

---

## 目标检测

**所有 agent 共享此系统。**

当 self-improve 运行时：
1. 扫描 `/path/to/self-improve/`（系统级，所有 agent 共享）
2. 读取各 agent 的 session 日志（如果可访问）
3. 处理结果写入共享位置，所有 agent 可读

---

## 中间文件位置

| 模块 | 输入 | 输出 |
|------|------|------|
| feedback-collector | 对话日志 | `data/feedback/*.jsonl`<br>`data/corrections.md` |
| evaluator | `data/feedback/` | `data/feedback/*.jsonl`（追加评估结果）|
| classifier | `data/corrections.md` | `data/themes/{主题}/*.md`（含 frontmatter 标签）|
| memory-layer | `data/themes/` | `data/hot.md`<br>`data/themes/`<br>`data/archive/` |
| knowledge-archiver | `data/corrections.md`<br>`data/reflections.md` | `/path/to/learned/errors\*.md`<br>`/path/to/learned/lessons\*.md` |
| methodology-extractor | `data/reflections.md`<br>`data/feedback/` | `/path/to/learned/methodologies\*.md` |
| skill-improver | feedback/ | themes/skill-improvements/（先沉淀，后固化）|
| proposer | `data/hot.md`<br>`data/corrections.md` | `proposals/PENDING.md` |
| profiler | `data/feedback/` | `data/profile.md` |
| reflector | `data/feedback/` | `data/reflections.md` |

---

## 写入规范遵循

**本系统遵循以下规范：**

| 位置 | 规范 |
|------|------|
| `/path/to/self-improve/` | 遵循 shared-manager skill 的写入规范 |
| `/path/to/learned/` | 遵循 knowledge-manager skill 的写入规范 |

**具体要求：**
- 写入后更新索引（`data_structure.md`）
- 新建目录必须有 `data_structure.md`
- 遵循目录分类规则

---

## 模块开关

### 查看开关状态
```bash
node scripts/status.mjs
```

### 一键启用/禁用模块

**方式 A：改 config.yaml**
```yaml
modules:
  classifier:
    enabled: false  # 改为 false 禁用
```

**方式 B：用命令（需要实现）**
```bash
node scripts/module.mjs enable classifier
node scripts/module.mjs disable classifier
```

---

## 新增模块流程

```
1. 在 modules/ 下创建目录：modules/新模块/
2. 写 MODULE.md（按标准格式）
3. 在 config.yaml 的 modules 中注册
4. 如需新数据目录，在 data/ 下创建
5. 更新 changelog.md
```

**标准 MODULE.md 格式：**
```markdown
# 模块名

> 版本：x.y.z
> 状态：active | disabled

## 职责
一句话说明

## 输入
从哪读取数据

## 输出
写到哪

## 接口
如何调用

## 依赖
依赖哪些模块
```

---

## 删除模块流程

```
1. 在 config.yaml 中设置 enabled: false
2. 确认模块无被其他模块依赖
3. 删除 modules/模块名/ 目录
4. 从 config.yaml 中移除注册
5. 如需删除数据，确认后删除 data/下相关目录
6. 更新 changelog.md
```

**注意：** 数据文件不自动删除，需手动确认。

---

## Prompt 体系

所有模块的执行指令集中管理在 `prompts/` 目录：

| Prompt 文件 | 用途 | 使用时机 |
|------------|------|---------|
| `cron-trigger.md` | Cron 触发时的执行入口 | 每 3 天定时触发 |
| `feedback-collector.md` | 扫描对话，提取信号 | 步骤1 |
| `distill-classifier.md` | 提炼三层次规则 + 分类 + 价值密度 | 步骤2 |
| `memory-layer.md` | 分层记忆管理（升降级）| 步骤3 |
| `proposer.md` | 判断产出通道，生成固化建议 | 步骤4 |
| `reflector.md` | 自反思 | 步骤5 |
| `profiler.md` | 团队能力画像（每3次）| 步骤6 |
| `notify.md` | 通知用户 | 步骤7 |
| `execution.md` | 审批后执行写入 | 独立触发 |

**参考文档（已合并到主 prompt 文件，发布包不包含）：**
- 价值判断五问、三层次提炼 → `distill-classifier.md`
- 分类标签体系 → `distill-classifier.md`
- 分流决策树 → `proposer.md`
- 固化执行详细步骤 → `execution.md`
- 方法论提取 → `distill-classifier.md`

**Prompt 质量是系统的核心。** 修改 prompt 前请充分测试。