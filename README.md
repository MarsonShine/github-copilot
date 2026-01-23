# github-copilot

用于沉淀 **GitHub Copilot** 的使用经验、团队协作约定与高质量参考资料的轻量仓库。

- 收集官方文档与社区精选资源（附简短说明，便于快速判断是否值得阅读）
- 记录可复用的提示词（prompts）/工作流建议（让 Copilot 更贴合你的项目与习惯）

> 当前仓库以资料索引为主；后续如增加示例模板/脚手架，会在 README 中同步结构与用法。

## 目录
- [推荐阅读](#推荐阅读)
- [建议使用路径](#建议使用路径)
- [贡献方式](#贡献方式)
- [许可证](#许可证)

## 推荐阅读

### 官方文档（优先）
- **Customize chat to your workflow**：定制 Copilot Chat 以适配你的工作流（提示词、指令、上下文等）
  - https://code.visualstudio.com/docs/copilot/customization/overview

### 精选集合
- **awesome-copilot**：GitHub 官方维护的 Copilot 资源集合（覆盖 IDE、Chat、Agent、最佳实践等）
  - https://github.com/github/awesome-copilot

## 建议使用路径

1) 明确你要 Copilot 帮你完成的任务类型
- 生成样板代码 / 解释代码 / 重构建议 / 测试生成 / 报错排查 / 文档与注释补全。

2) 统一“高质量输入”
- 尽量提供：目标产出、约束（语言/框架/风格/兼容性）、关键上下文（文件路径、日志、复现步骤、期望行为）。

3) 把高频指令沉淀为可复用模板
- 把你反复在 Chat 里说的话固化成模板（例如：PR 描述模板、Bug 排查清单、重构约束、测试用例生成规则）。
