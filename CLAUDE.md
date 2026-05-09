# claude-md

## Overview
生成和维护 CLAUDE.md 文件的 skill。面向 Claude Code 用户。
规范一致性 > 内容完整度 > 更新速度

## Tech Stack
- Markdown，零运行时依赖

## Do NOT introduce unless explicitly requested
- Python / Node.js 脚本 — 本仓库是纯文档，不要引入运行时依赖
- CI/CD 配置 — 无构建、无测试，不需要流水线
- 自动化发布脚本 — 变更频率低，手动发版即可

## Coding Rules
- 每个 skill 独立目录，入口文件统一命名为 `SKILL.md`
- 中文变体目录加 `-zh` 后缀（如 `claude-md-zh/`）
- skill 目录下 references / assets 用 `references/`、`assets/` 子目录
- README.md 是英文版项目说明，README-zh.md 是中文版，内容保持同步
- 新增 skill 时同步更新两个 README 的功能列表

## Working Style
- 显式声明假设；不确定就问，不要默选
- 多种理解时列出选项；有更简单方案主动提出
- 只写解决问题的最小代码量；不做推测性抽象
- 匹配现有代码风格，即使你的写法不同
- 多步骤任务先陈述计划再执行
- 不用"Great question!"/"Happy to help!"等客套话

## Pointers (read on demand)
- 项目文档：`README.md`（英文）、`README-zh.md`（中文）
- Skill 入口：`claude-md/SKILL.md`（英文）、`claude-md-zh/SKILL.md`（中文）

