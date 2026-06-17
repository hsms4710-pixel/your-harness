# Your Harness

定制化 Harness Engineering 系统生成器

[![Version](https://img.shields.io/badge/version-v3.7.4-blue)](https://github.com/hsms4710-pixel/your-harness)

## 核心价值

为项目量身定制 Harness Engineering 系统，生成可执行的定制化工具，而非通用推荐文档。

**与通用工具的区别：**
- **Spec Kit / OpenSpec / Superpowers**：输出推荐文档 + 通用 Skill
- **Your Harness**：输出可执行脚本 + 自动触发 Hook + 定制 Skill + 完整工作流

## 快速开始

### 方式一：自然语言（推荐）

```
"帮我给这个项目生成工程规范"
"检查一下代码规范"
"我有个老项目，存量代码不达标怎么办"
```

### 方式二：命令行

```
/harness-full    # 完整流程
/harness-status  # 查看状态
/harness-help    # 查看帮助
```

详见 [用户指南](./HARNESS-USER-GUIDE.md)

## 架构 (v3.7.4)

```
用户输入
    ↓
harness-architect (v3.7.4) → 智能路由
    ↓
┌─────────────────────────────────────────┐
│  harness-archaeology (v3.5.1)           │
│  → 识别项目特征，输出项目模式            │
│  → 扫描进度反馈，历史记录保存            │
└─────────────────────────────────────────┘
    ↓
┌─────────────────────────────────────────┐
│  harness-designer (v3.5.1)              │
│  → 生成定制脚本/Hook/Skill/工作流       │
│  → 迭代优化命令，历史记录保存            │
└─────────────────────────────────────────┘
    ↓
┌─────────────────────────────────────────┐
│  harness-validator (v3.7.2)             │
│  → 5 层质量验证                         │
│  → 报告生成，质量评分，趋势分析          │
└─────────────────────────────────────────┘
```

## 核心组件

| 组件 | 版本 | 职责 |
|------|------|------|
| harness-architect | v3.7.4 | 总调度，智能路由，命令处理，错误处理 |
| harness-archaeology | v3.5.1 | 代码扫描 + 规范推断 + 进度反馈 + 历史记录 |
| harness-designer | v3.5.1 | 生成定制脚本/Hook/Skill/工作流，迭代优化 |
| harness-validator | v3.7.2 | 5 层质量验证，报告生成，质量评分 |
| harness-onboarding | v3.7.0 | Legacy 项目渐进接入，进度反馈 |
| harness-registry | v3.6.0 | 多项目管理，团队协作 |

## 支持的语言和框架

| 语言 | 框架/工具 |
|------|----------|
| **Python** | FastAPI, Flask, Django, PDM, Poetry, pip |
| **Go** | Gin, Echo, Fiber, golangci-lint v2 |
| **TypeScript** | Next.js, React, Nest.js, pnpm, yarn, npm, Turborepo |

## 主要功能

### 状态查询
- `/harness-status` - 查看当前状态
- `/harness-history` - 查看决策历史
- `/harness-why <item>` - 解释配置原因

### 迭代优化
- `/harness-adjust <key> <value>` - 调整配置
- `/harness-add <type>` - 添加检查项
- `/harness-remove <item>` - 移除检查项
- `/harness-update [scope]` - 更新版本

### 报告功能
- `/harness-report` - 生成报告（HTML/PDF）
- 质量评分、趋势分析、多项目对比

### 快捷别名
- `/hs` = `/harness-status`
- `/ha` = `/harness-adjust`
- `/hh` = `/harness-history`
- `/hr` = `/harness-report`
- `/hf` = `/harness-full`
- `/h` = `/harness-help`

## 输出结构

```
.harness/
├── config.yaml          # 主配置
├── context.yaml         # 上下文（Skills 间共享）
├── decision-history.yaml # 决策历史
├── constitution.md      # 项目特征摘要
├── scripts/             # 定制脚本
├── hooks/               # 自动化 Hook
├── skills/              # 定制 Skill
├── gates/               # 质量门禁
└── workflows/           # 定制工作流
```

## 文档

- [用户指南](./HARNESS-USER-GUIDE.md) - 完整使用说明
- [开发计划](./development-plan.md) - 版本开发历史
- [命令速查表](./INDEX.md) - 快速参考

## 许可证

MIT
