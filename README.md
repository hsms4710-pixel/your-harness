# Your Harness

定制化 Harness Engineering 系统生成器

## 核心价值

为项目量身定制 Harness Engineering 系统，生成可执行的定制化工具，而非通用推荐文档。

**与通用工具的区别：**
- **Spec Kit / OpenSpec / Superpowers**：输出推荐文档 + 通用 Skill
- **Your Harness**：输出可执行脚本 + 自动触发 Hook + 定制 Skill + 完整工作流

## 架构 (v3.0)

```
harness-archaeology (v3.0.0)  →  识别项目特征，输出项目模式
              ↓
harness-designer (v3.0.0)      →  根据项目模式生成定制脚本/Hook/Skill/工作流
              ↓
harness-architect (v3.0.0)    →  总调度，协调考古和设计流程
```

## 核心组件

| 组件 | 版本 | 职责 |
|------|------|------|
| harness-architect | v3.0.0 | 总调度，路由到对应专业 Skill |
| harness-archaeology | v3.0.0 | 代码扫描 + 规范推断 + 置信度标注 |
| harness-designer | v3.0.0 | 生成定制脚本/Hook/Skill/工作流 |
| harness-validator | v2.2.0 | 5 层质量验证 |
| harness-onboarding | v2.2.0 | Legacy 项目渐进接入 |
| harness-registry | v2.2.0 | 多项目管理 |

## 支持的语言

- **Python** (含 uv Monorepo)
- **Go**
- **TypeScript/JavaScript** (含 pnpm/npm Monorepo)

## 项目模式识别

| 模式 | 生成脚本 |
|------|----------|
| API-Driven | check-api-sync.py |
| DB-Driven | check-migration.py |
| Microservice | check-service-deps.py |
| AI/LLM | check-api-keys.py |

## 输出结构

```
.harness/
├── constitution.md      # 项目特征摘要
├── scripts/             # 定制脚本
├── hooks/               # 自动化 Hook
├── skills/              # 定制 Skill
└── workflows/           # 定制工作流
```

## 使用方式

在 WorkBuddy 中调用 `harness-architect` Skill，它会根据你的需求路由到对应的专业 Skill。

## 许可证

MIT
