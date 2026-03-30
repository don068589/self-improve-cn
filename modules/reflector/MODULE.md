# reflector

> 版本：1.1.0
> 状态：active

## 职责
自反思，提炼可复用做法，形成闭环学习。

## 详细执行指南

**执行时读取**：`prompts/reflector.md`

包含：
- 反思格式规范
- 可复用做法提炼方法
- 闭环说明（reflections.md → 下轮步骤1）

## 输入
- 本次运行过程

## 输出
- `data/reflections.md` — 自反思日志

## 闭环机制

reflections.md 会在**下轮步骤1**被 feedback-collector 读取，形成闭环学习。

## 依赖
无（基础模块）