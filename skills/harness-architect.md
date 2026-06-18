---
name: harness-architect
description: |
  Harness Architect — 总调度器。
  
  核心职责：
  1. 接收用户请求，判断意图
  2. 调用 harness-archaeology 识别项目特征
  3. 基于特征调用 harness-designer 生成开发流程体系
  4. 支持迭代调整和历史追溯
  
  🎯 触发方式（自然语言即可）：
  - "帮我生成 Harness" / "给这个项目加规范"
  - "检查一下代码规范" / "验证 Harness 质量"
  - "我有个老项目，帮我加规范"
  - "我有多个项目，怎么统一管理"
  
  🎯 核心命令：
  - `/harness-full` - 触发完整流程（archaeology → designer）
  - `/harness-status` - 查看当前状态
  - `/harness-adjust` - 调整配置
  
  v4.0.0: 重大重构，支持生成完整的开发流程体系（Workflow + Templates）
version: 4.0.0
author: agent_created
tags: [harness, architect, orchestrator, workflow, templates, smart-routing]
---

# Harness Architect — 总调度器

Harness 工程的总调度器。协调 harness-archaeology 和 harness-designer，为项目生成完整的开发流程体系。

## 核心定位（v4.0 重构）

```
旧定位（v3.x）：生成 lint 规则、CI 配置、检查脚本
新定位（v4.0）：生成完整的开发流程体系，类似 SmartMate 的 dev-workflow + template

工作流程：
  用户请求
    ↓
  harness-architect（本 Skill）
    ↓
  harness-archaeology（识别项目特征）
    ↓ 输出：intent + surfaces + risk + depth
  harness-designer（生成开发流程体系）
    ↓ 输出：Workflow + Templates + References + Scripts
  完成
```

## 触发条件

- 用户请求生成 Harness / 开发规范
- 用户请求分析项目并生成流程
- 用户使用 `/harness-full` 命令

## 核心流程

### Step 1: 接收用户请求

```
用户可能的请求：
- "帮我给这个项目生成开发流程"
- "分析一下这个项目，生成规范"
- "/harness-full"
```

### Step 2: 调用 harness-archaeology

调用 harness-archaeology 识别项目特征：

```yaml
# harness-archaeology 输出
project_features:
  project_name: smartmate
  
  # 技术栈
  languages: [Go, Python, TypeScript]
  frameworks: [tRPC, FastAPI, React]
  
  # 特殊流程检测
  special_flows:
    - name: protocol-chain
      detected: true
      stages: 6
      
  # 分类结果
  classification:
    intent: new-capability
    surfaces: [api-contract, data-model, protocol-chain]
    risk: high
    
  # 生成深度决策
  depth_decision:
    depth: full
    generate: [workflow, templates, references, scripts, constitution]
```

### Step 3: 调用 harness-designer

将识别结果传递给 harness-designer：

```yaml
# 传递给 harness-designer 的信息
input:
  project_name: smartmate
  depth: full
  intent: new-capability
  surfaces: [api-contract, data-model, protocol-chain]
  risk: high
  special_flows:
    - name: protocol-chain
      stages: 6
      
# harness-designer 输出
output:
  .harness/
  ├── SKILL.md                    # 主流程 Skill
  ├── templates/                  # 开发模板
  │   ├── TEMPLATE-protocol-change.md
  │   └── ...
  ├── references/                 # 流程参考文档
  ├── scripts/                    # 流程检查脚本
  └── constitution-profiles.json  # 宪法模型
```

### Step 4: 展示生成结果

```
✅ Harness 生成完成！

生成深度: full
生成产物:
  • SKILL.md（主流程 Skill）
  • templates/TEMPLATE-protocol-change.md
  • templates/TEMPLATE-db-change.md
  • references/gate-protocol.md
  • references/clarification-loop.md
  • scripts/workflow-preflight.py
  • scripts/check-gates.py
  • scripts/gate-status.py
  • constitution-profiles.json

生成的流程体系包含：
  • 7 个强制闸门（G0-G6）
  • 2 个领域特定模板（protocol-change, db-change）
  • 完整的流程检查脚本

使用方式：
  1. 阅读 SKILL.md 了解主流程
  2. 在开发时按流程执行
  3. 在闸门处等待人工确认
```

---

## 快捷命令

### /harness-full

触发完整的 archaeology → designer 流程：

```bash
/harness-full
```

### /harness-status

查看当前 Harness 状态：

```bash
/harness-status
```

输出：

```
📊 Harness 状态
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

项目: smartmate
生成深度: full
生成时间: 2026-06-18 10:00:00

📋 生成产物
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Workflow: .harness/SKILL.md
Templates: 2 个
  • TEMPLATE-protocol-change.md
  • TEMPLATE-db-change.md

References: 3 个
  • gate-protocol.md
  • clarification-loop.md
  • task-execution-protocol.md

Scripts: 4 个
  • workflow-preflight.py
  • check-gates.py
  • gate-status.py
  • check-artifacts.py

Gates: 7 个
  • G0_CLARIFICATION
  • G1_PRD
  • G2_TASKS
  • G3_SLICE
  • G4_VERIFY
  • G5_REVIEW
  • G6_SHIP

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### /harness-adjust

调整生成的配置：

```bash
/harness-adjust <配置项> <新值>
```

示例：

```bash
/harness-adjust 闸门数量 5
/harness-adjust 风险等级 medium
```

### /harness-add-template

添加新的 Template：

```bash
/harness-add-template <template-name>
```

### /harness-remove-template

移除 Template：

```bash
/harness-remove-template <template-name>
```

---

## 生成深度说明

### Full（完整）

```
适用场景：
- 有特殊的多阶段变更流程
- 有严格的闸门要求
- 有领域特定的 SOP

生成产物：
├── SKILL.md                    # 主流程 Skill
├── templates/                  # 开发模板
├── references/                 # 流程参考文档
├── scripts/                    # 流程检查脚本
└── constitution-profiles.json  # 宪法模型
```

### Medium（中等）

```
适用场景：
- 有闸门要求但无特殊流程
- 中等复杂度的项目

生成产物：
├── SKILL.md                    # 主流程 Skill
├── references/                 # 流程参考文档
├── scripts/                    # 流程检查脚本
└── constitution-profiles.json  # 宪法模型
```

### Light（轻量）

```
适用场景：
- 流程简单
- 通用 Skill 足够

生成产物：
├── SKILL.md                    # 轻量 Workflow
└── scripts/
    └── gate-check.py           # 基础闸门检查
```

---

## 错误码

| 错误码 | 说明 | 修复建议 |
|--------|------|----------|
| H001 | 项目路径不存在 | 检查项目路径是否正确 |
| H002 | 无法识别项目语言 | 项目可能为空或配置文件缺失 |
| H003 | 生成深度不支持 | 检查 harness-designer 版本 |
| H004 | 模板生成失败 | 检查模板依赖是否完整 |
| H005 | 闸门定义冲突 | 检查闸门配置是否正确 |

---

## 版本历史

- v4.0.0 (2026-06-18): 重大重构
  - 支持生成完整的开发流程体系
  - 引入 intent + surfaces + risk 分类模型
  - 支持三种生成深度（light/medium/full）
  - 协调 harness-archaeology 和 harness-designer
- v3.7.4 (2026-06-17): 添加错误码体系
- v3.7.3 (2026-06-17): 添加快捷别名
- v3.7.1 (2026-06-17): 添加统一上下文共享
- v3.5.1 (2026-06-16): 添加历史追溯功能
- v3.1.0 (2026-06-16): 增加智能意图识别
- v3.0.0 (2026-06-15): 初始版本
