# Your Harness

为项目生成完整的开发流程体系（Dev Workflow + Templates），类似 SmartMate 的深度流程规范。

[![Version](https://img.shields.io/badge/version-v6.0.0-blue)](https://github.com/hsms4710-pixel/your-harness)

## 核心价值

**Your Harness 为任何项目生成一套完整的开发流程体系**，包括：
- **Workflow（SKILL.md）**：主流程定义，类似 SmartMate 的 dev-workflow
- **Templates**：领域特定的开发模板（如协议变更、数据库变更）
- **Gates**：强制闸门机制，确保关键节点有人工确认
- **Scripts**：流程检查脚本，确保流程合规

**与通用工具的区别：**
- **Spec Kit / OpenSpec / Superpowers**：输出推荐文档 + 通用 Skill
- **Your Harness**：输出完整的开发流程体系，可直接用于项目开发

## 快速开始

### 方式一：自然语言（推荐）

```
"帮我给这个项目生成开发流程"
"分析一下这个项目，生成规范"
"/harness-full"
```

### 方式二：命令行

```
/harness-full    # 完整流程
/harness-status  # 查看状态
/harness-help    # 查看帮助
```

详见 [用户指南](./HARNESS-USER-GUIDE.md)

## 架构 (当前版本)

```
用户输入
    ↓
harness-architect (v4.0.0) → 总调度
    ↓
harness-archaeology (v4.0.0) → 识别项目特征
    ↓ 输出：intent + surfaces + risk + depth
harness-designer (v6.0.0) → 生成开发流程体系
    ↓ 输出：Workflow + Templates + References + Scripts
完成
```

## 核心组件

| 组件 | 版本 | 职责 |
|------|------|------|
| harness-architect | v4.0.0 | 总调度，协调 archaeology 和 designer |
| harness-archaeology | v4.0.0 | 识别项目特征，判断生成深度（light/medium/full） |
| harness-designer | v6.0.0 | 生成 Workflow + Templates + References + Scripts |
| harness-validator | v3.7.2 | 5 层质量验证，报告生成 |
| harness-onboarding | v3.7.0 | Legacy 项目渐进接入 |
| harness-registry | v3.6.0 | 多项目管理 |

## 分类模型

### Intents（开发意图）

- `new-capability` - 新功能开发
- `incremental-change` - 增量变更
- `bugfix` - Bug 修复
- `incident-hotfix` - 线上事故修复
- `architecture-decision` - 架构决策
- `consistency-check` - 一致性检查

### Surfaces（影响面，可组合）

- `api-contract` - API 契约
- `data-model` - 数据模型
- `frontend-ui` - 前端 UI
- `backend-logic` - 后端逻辑
- `protocol-chain` - 协议链路
- `security-sensitive` - 安全敏感
- `performance-sensitive` - 性能敏感

### Risk（风险等级）

- `low` - 低风险
- `medium` - 中风险
- `high` - 高风险
- `incident` - 线上事故

## 生成深度

### Full（完整）

```
适用：有特殊流程的项目（如协议驱动开发）

生成产物：
├── SKILL.md                    # 主流程 Skill
├── templates/                  # 开发模板
├── references/                 # 流程参考文档
├── scripts/                    # 流程检查脚本
└── constitution-profiles.json  # 宪法模型
```

### Medium（中等）

```
适用：有闸门要求但无特殊流程

生成产物：
├── SKILL.md
├── references/
├── scripts/
└── constitution-profiles.json
```

### Light（轻量）

```
适用：流程简单

生成产物：
├── SKILL.md
└── scripts/gate-check.py
```

## 输出结构

```
.harness/
├── SKILL.md                    # 主流程 Skill（类似 smartmate-dev-workflow）
├── README.md                   # 使用说明
├── constitution-profiles.json  # 宪法模型（机器可读）
├── constitution-profiles.md    # 宪法模型（人类可读）
├── templates/                  # 开发模板（按需生成）
│   ├── TEMPLATE-feature.md
│   ├── TEMPLATE-bugfix.md
│   └── TEMPLATE-db-change.md
├── references/                 # 流程参考文档
│   ├── gate-protocol.md        # 闸门协议
│   ├── clarification-loop.md   # 需求澄清协议
│   └── task-execution-protocol.md
└── scripts/                    # 流程检查脚本
    ├── workflow-preflight.py   # 流程预检
    ├── check-gates.py          # 闸门检查
    ├── gate-status.py          # 闸门状态管理
    └── check-artifacts.py      # 产物检查
```

## 主要命令

| 命令 | 说明 |
|------|------|
| `/harness-full` | 触发完整流程 |
| `/harness-status` | 查看当前状态 |
| `/harness-adjust` | 调整配置 |
| `/harness-add-template` | 添加 Template |
| `/harness-remove-template` | 移除 Template |
| `/harness-help` | 查看帮助 |

## 文档

- [用户指南](./HARNESS-USER-GUIDE.md) - 完整使用说明
- [开发计划](./development-plan.md) - 版本开发历史
- [命令速查表](./INDEX.md) - 快速参考

## 许可证

MIT
