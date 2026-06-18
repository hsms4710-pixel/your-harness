# Changelog

All notable changes to the Your Harness Skills project.

## [4.0.0] - 2026-06-18

### Major Refactoring

**从"配置生成器"转型为"开发流程体系生成器"**

#### 核心变更

- **harness-designer**：完全重写
  - 新增 Workflow 生成能力（SKILL.md）
  - 新增 Template 按需生成能力
  - 新增闸门机制（7 个强制闸门）
  - 新增宪法模型（constitution-profiles.json）
  - 引入 intent + surfaces + risk 分类模型
  - 支持三种生成深度（light/medium/full）

- **harness-archaeology**：重大更新
  - 新增 intent + surfaces + risk 分类模型
  - 新增生成深度决策逻辑
  - 新增特殊流程检测（protocol-chain 等）
  - 输出格式更新，支持 harness-designer 决策

- **harness-architect**：重构
  - 作为总调度器协调 archaeology 和 designer
  - 支持生成完整的开发流程体系

#### 分类模型

```yaml
# Intents（开发意图）
- new-capability
- incremental-change
- bugfix
- incident-hotfix
- architecture-decision
- consistency-check

# Surfaces（影响面，可组合）
- api-contract
- data-model
- frontend-ui
- backend-logic
- protocol-chain
- security-sensitive
- performance-sensitive

# Risk（风险等级）
- low
- medium
- high
- incident

# Depth（生成深度）
- light: 只生成 SKILL.md + 基础脚本
- medium: 生成 SKILL.md + references + scripts
- full: 生成完整的 Workflow + Templates + References + Scripts
```

#### 输出产物

```
.harness/
├── SKILL.md                    # 主流程 Skill
├── templates/                  # 开发模板（按需）
├── references/                 # 流程参考文档
├── scripts/                    # 流程检查脚本
└── constitution-profiles.json  # 宪法模型
```

### Breaking Changes

- 移除了 v3.x 的"生成 lint 规则"定位
- 移除了 v3.x 的"生成 CI 配置"作为主要输出
- 新的定位是"生成开发流程体系"

---

## [3.7.4] - 2026-06-17

### Added

- harness-architect: 错误码体系、详细错误说明、修复建议、错误日志
- harness-architect: 快捷别名、帮助系统、命令历史、命令自动补全

## [3.7.3] - 2026-06-17

### Added

- harness-architect: 快捷别名（/hs, /ha, /hh, /hr, /hf, /h）
- harness-architect: 帮助系统（/harness-help, /harness-help <command>）
- harness-architect: 错误码查询（/harness-error <code>）

## [3.7.2] - 2026-06-17

### Added

- harness-validator: 多项目对比报告
- harness-validator: 自定义报告模板
- harness-validator: 报告自动发送（邮件/Slack/企业微信）
- harness-validator: 报告模板库（6个模板）

## [3.7.1] - 2026-06-17

### Added

- harness-architect: 统一上下文共享
- harness-architect: /harness-full 命令
- harness-architect: Skills 间状态同步

## [3.7.0] - 2026-06-17

### Added

- harness-onboarding: 进度反馈
- harness-onboarding: 决策历史记录
- harness-onboarding: /onboarding-status, /onboarding-history 命令

## [3.6.1] - 2026-06-16

### Added

- harness-validator: /harness-report 命令
- harness-validator: HTML/PDF 格式输出
- harness-validator: 质量评分、趋势分析、定期报告

## [3.6.0] - 2026-06-16

### Added

- harness-registry: 团队协作功能
- harness-registry: 权限控制（owner/admin/member/viewer）
- harness-registry: 评论和审批功能
- harness-registry: /harness-share 命令

## [3.5.1] - 2026-06-16

### Added

- harness-architect: /harness-history 命令
- harness-architect: /harness-why 命令
- harness-archaeology: 历史记录保存
- harness-designer: 历史记录保存

## [3.5.0] - 2026-06-16

### Added

- harness-designer: /harness-status 命令
- harness-designer: /harness-adjust 命令
- harness-designer: /harness-add 命令
- harness-designer: /harness-remove 命令
- harness-designer: /harness-update 命令
- harness-archaeology: 扫描进度反馈
- harness-designer: 生成进度反馈

## [3.4.0] - 2026-06-16

### Added

- harness-architect: 需求澄清对话
- harness-architect: 6 问题库（项目类型、团队规模、质量要求、CI 平台、CI 触发、安全要求）
- harness-archaeology: 确认环节
- harness-archaeology: 项目特征确认
- harness-archaeology: 存量代码处理选项
- harness-archaeology: 优先级确认

## [3.3.0] - 2026-06-16

### Added

- harness-archaeology: 融合 SDD 工具最佳实践
- harness-designer: Constitution 生成
- harness-designer: 质量门禁
- harness-designer: 任务拆分
- harness-designer: TDD 支持

## [3.2.0] - 2026-06-16

### Added

- harness-archaeology: PDM 检测
- harness-archaeology: golangci-lint v2 检测
- harness-archaeology: Turborepo 检测

## [3.1.0] - 2026-06-16

### Added

- harness-architect: 智能意图识别
- harness-architect: CI/CD 集成
- harness-architect: 快速开始
- harness-architect: 可视化展示

## [3.0.0] - 2026-06-15

### Added

- 重构为定制化系统输出
- 生成脚本/Hook/Skill/工作流
- 支持 Python/Go/TypeScript

## [2.2.0] - 2026-06-15

### Added

- harness-architect: 两阶段流程
- harness-designer: From Inferred 模式
- harness-designer: AI/LLM 项目特殊处理
- harness-validator: 自动化检查脚本指引
- harness-onboarding: 核心场景明确化
- harness-registry: 工蜂/GitHub 集成

## [2.1.0] - 2026-06-15

### Added

- 初始版本发布
- 6 个核心 Skills
- 基础架构和路由逻辑
