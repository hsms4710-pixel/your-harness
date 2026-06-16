# Changelog

All notable changes to the Your Harness Skills project.

## [2.2.0] - 2026-06-15

### Added

#### harness-architect
- 两阶段流程（archaeology → designer）
- 明确 onboarding 与 archaeology 的分工
- 新增示例 5（已有代码项目）和示例 6（存量代码不合规）
- 更新路由逻辑和调用方式

#### harness-designer
- 新增模式：From Inferred（基于 Archaeology 推断结果）
- AI/LLM 项目特殊处理（api-key-management, async-patterns 等）
- 四种模式：Greenfield, Guided Discovery, Brownfield, From Inferred

#### harness-validator
- 自动化检查脚本指引（Python/Node.js/Go）
- 与 SmartMate 验证协议集成
- 提供 L3 Dry-run 验证的自动化实现模板

#### harness-onboarding
- 核心场景明确化：项目已有 Harness，存量代码不合规
- 增加前置条件：要求项目已有 .agents/ 目录
- 明确与 harness-archaeology 的分工
- 增加判断流程图

#### harness-registry
- 工蜂/GitHub 集成（仓库自动发现、Webhook 同步、CI 集成）
- 自动同步机制
- 同步脚本模板（Python）

### Changed

- 所有 Skill 版本升级到 v2.2.0
- 更新文档和示例

## [2.1.0] - 2026-06-15

### Added

- 初始版本发布
- 6 个核心 Skills
- 基础架构和路由逻辑
