# 贡献指南

感谢你对 Self-Improve 的关注！

## 如何贡献

### 报告问题

- 使用 [GitHub Issues](https://github.com/openclaw/self-improve/issues) 报告 bug
- 提供复现步骤
- 说明你的 OpenClaw 和 Node.js 版本

### 提交更改

1. Fork 本仓库
2. 创建特性分支 (`git checkout -b feature/your-feature`)
3. 进行修改
4. 充分测试
5. 提交 Pull Request

### 代码风格

- 使用 ES 模块（`.mjs` 扩展名）
- 遵循现有代码模式
- 为复杂逻辑添加注释

### 模块开发

添加或修改模块时：

1. 阅读 `modules/TEMPLATE.md` 了解格式要求
2. 在模块目录创建 `MODULE.md`
3. 在 `prompts/` 添加对应的执行指令
4. 在 `config.yaml` 注册
5. 更新 `changelog.md`

### Prompt 开发

Prompt 是本系统的核心。编写时注意：

- 明确具体
- 包含示例
- 清晰定义输入/输出
- 测试多种场景

## 有问题？

提交 Issue 或加入 [OpenClaw 社区](https://discord.com/invite/clawd)。
