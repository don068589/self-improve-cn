# 待审批的配置修改建议

> 只有固化到系统文件时需要确认，其他全自动

---

（安装后自动生成 Cron 任务建议）

## 格式

```markdown
## YYYY-MM-DD HH:MM
- **编号**: P-XXX
- **类型**: {correction/methodology/innovation/system}
- **内容**: {建议内容}
- **目标文件**: {要修改的文件路径}
```

## 状态说明

- 待审批 → 用户确认后执行
- 已批准 → execution 模块执行写入
- 已拒绝 → 记录但不执行