# distill-classifier

> 版本：1.0.0
> 状态：active
> 合并自：distill + classifier

## 职责

从信号中提炼规则三层次，分类沉淀，标注价值密度。

## 详细执行指南

**执行时读取**：`prompts/distill-classifier.md`

包含：
- 价值判断五问
- 提炼三层次方法
- 分类标签体系
- 去重和计数规则
- 输出格式规范

## 输入

- `data/feedback/*.jsonl` — feedback-collector 产出的信号

## 输出

- `data/themes/{主题}/{规则}.md` — 已提炼三层次的规则

## 核心能力

### 价值判断五问

1. 这条规则能改变行为吗？
2. 这条规则有具体的触发场景吗？
3. 这条规则能被验证吗？
4. 这条规则和现有规则重复吗？
5. 用户会认同这条规则吗？

### 提炼三层次

1. **表面规则**：触发场景 + 具体动作 + 判断标准
2. **行为模式**：可迁移的决策逻辑
3. **思维原则**：一句话 + 解释

### 分类

- 主题分类（theme）
- 领域分类（domain）
- 项目分类（project）
- 价值密度标注（value_density）

## 执行规则

1. 从 feedback 筛选值得提炼的内容
2. 价值判断五问验证
3. 提炼三层次
4. 去重检查
5. 分类并写入 themes/

## 依赖

- feedback-collector — 需要 feedback 数据