# 项目记忆

## 项目定位
- 核心价值：为项目量身定制 Harness Engineering 系统
- 差异化：基于项目代码推断，生成可执行的定制化系统
- 与通用工具（Spec Kit/OpenSpec/Superpowers）的本质区别：
  - 通用工具：输出推荐文档 + 通用 Skill
  - 我们：输出可执行脚本 + 自动触发 Hook + 定制 Skill + 完整工作流
- 定制化输出包括：
  1. 定制脚本（Python/Node.js/Go）：如 check-api-sync.py 检查前后端 API 同步
  2. 定制 Hook：如 pre-commit 自动检查 API 同步
  3. 定制 Skill：如 api-sync-check 封装 API 同步检查逻辑
  4. 定制工作流：如 api-change.md API 变更完整流程
- v3.6 架构（2026-06-16 最终版本）：
  - harness-archaeology (v3.5.1)：识别项目特征，输出项目模式，扫描进度反馈，历史记录保存
  - harness-designer (v3.5.1)：根据项目模式生成定制脚本/Hook/Skill/工作流，迭代优化命令，历史记录保存
  - harness-architect (v3.5.1)：总调度，协调考古和设计流程，/harness-status 命令，/harness-history 命令，/harness-why 命令
  - harness-validator (v3.6.1)：5 层验证，报告生成功能，质量评分，趋势分析
  - harness-onboarding (v2.2.0)：渐进收紧
  - harness-registry (v3.6.0)：多项目管理，团队协作，权限控制，评论审批，分享功能

## 迭代优化命令（v3.5.0 新增）

| 命令 | 功能 | 语法 |
|------|------|------|
| /harness-status | 查看当前状态 | /harness-status |
| /harness-adjust | 调整配置 | /harness-adjust <配置项> <新值> |
| /harness-add | 添加检查项 | /harness-add <检查类型> [配置] |
| /harness-remove | 移除检查项 | /harness-remove <检查项> |
| /harness-update | 更新到最新版本 | /harness-update [scope] |

## 历史追溯命令（v3.5.1 新增）

| 命令 | 功能 | 语法 |
|------|------|------|
| /harness-history | 查看决策历史 | /harness-history [--recent] [--decisions] |
| /harness-why | 解释配置原因 | /harness-why <配置项> |

## 协作功能命令（v3.6.0 新增）

| 命令 | 功能 | 语法 |
|------|------|------|
| /registry-share | 分享项目 | /registry-share <project> [--to email] [--link] |
| /registry-permissions | 管理权限 | /registry-permissions |
| /registry-approve | 审批变更 | /registry-approve <id> [--comment text] |

## 报告功能命令（v3.6.1 新增）

| 命令 | 功能 | 语法 |
|------|------|------|
| /harness-report | 生成报告 | /harness-report [--format html|pdf] [--output path] [--trend] |

## 核心理念
- v3.0-3.3: 过度自动化（扫描 → 生成 → 用户被动接受）
- v3.4: 智能协作（需求澄清 → 确认理解 → 智能生成）
- v3.5: 支持迭代（生成 → 查看状态 → 调整优化）
- v3.5.1: 历史追溯（记录决策 → 支持查询 → 解释配置）
- v3.6: 团队协作（权限控制 → 评论审批 → 分享功能）
- v3.6.1: 报告功能（质量评分 → 趋势分析 → 定期报告）

## 版本历史
- v3.6.1 (2026-06-16): 报告生成功能（/harness-report、HTML/PDF 格式、质量评分、趋势分析）
- v3.6.0 (2026-06-16): 协作功能（权限控制、评论审批、分享功能）
- v3.5.1 (2026-06-16): 历史追溯（/harness-history、/harness-why、决策记录）
- v3.5.0 (2026-06-16): 迭代优化命令 + 进度反馈
- v3.4.0 (2026-06-16): 需求澄清对话 + 确认环节
- v3.3.0 (2026-06-16): 融合 SDD 最佳实践
- v3.2.0 (2026-06-16): Constitution/Specs/TDD/7阶段工作流
- v3.1.0 (2026-06-16): 智能意图识别 + CI/CD 集成
- v3.0.0 (2026-06-15): 重构为定制化系统输出

## 开发计划完成状态
- ✅ v3.4.0: 需求澄清对话
- ✅ v3.4.1: 确认环节
- ✅ v3.5.0: 迭代优化命令 + 进度反馈
- ✅ v3.5.1: 历史追溯
- ✅ v3.6.0: 协作功能
- ✅ v3.6.1: 报告功能

## GitHub 仓库
- https://github.com/hsms4710-pixel/your-harness
