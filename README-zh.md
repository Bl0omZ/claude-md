# CLAUDE.md 生成器

一个 [Claude Code](https://claude.ai/code) skill，用于生成项目级和用户级 `CLAUDE.md` 文件。

## 功能

生成或审计两个层级的 `CLAUDE.md` 文件：

- **项目级**（≤200 行）— 面向特定代码库的技术栈、编码规则、禁用库和工作风格默认值
- **用户级**（≤150 行）— 面向个人的默认设置、取舍偏好和沟通风格，对所有项目生效

## 遵循原则

生成器遵循四条原则，源自 [Karpathy 的 Claude 工作风格](https://karpathy.ai)：

1. **先想再写** — 显式声明假设，列出选项，拿不准就问
2. **简单优先** — 用最少代码解决问题，不做推测性抽象
3. **精准修改** — 只碰必要的地方，匹配现有风格
4. **目标驱动** — 定义成功标准，循环直到验证通过

用户级生成参考 Karpathy 行为基线（`references/karpathy-claude.md`），根据用户确认的偏好进行适配改写。

## 项目结构

```
.
├── claude-md/              # 英文版本
│   ├── SKILL.md
│   └── references/
│       └── karpathy-claude.md
├── claude-md-zh/           # 中文版本
│   ├── SKILL.md
│   └── references/
│       └── karpathy-claude.md
├── README.md
└── README-zh.md
```

## 使用方式

在 Claude Code 中调用 `claude-md`（英文）或 `claude-md-zh`（中文）来生成或审计 `CLAUDE.md` 文件。