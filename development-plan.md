# Harness Skills 版本历史

> Harness Skills 开发版本记录

---

## 版本完成状态

| 版本 | 内容 | 状态 | 完成日期 |
|------|------|------|----------|
| v3.4.0 | 需求澄清对话 | ✅ 已完成 | 2026-06-16 |
| v3.4.1 | 确认环节 | ✅ 已完成 | 2026-06-16 |
| v3.5.0 | 迭代优化命令 + 进度反馈 | ✅ 已完成 | 2026-06-16 |
| v3.5.1 | 历史追溯 | ✅ 已完成 | 2026-06-16 |
| v3.6.0 | 协作功能 | ✅ 已完成 | 2026-06-16 |
| v3.6.1 | 报告功能 | ✅ 已完成 | 2026-06-16 |
| v3.7.0 | Onboarding 同步升级 | ✅ 已完成 | 2026-06-17 |
| v3.7.1 | 统一上下文共享 | ✅ 已完成 | 2026-06-17 |
| v3.7.2 | 报告增强 | ✅ 已完成 | 2026-06-17 |
| v3.7.3 | CLI 体验优化 | ✅ 已完成 | 2026-06-17 |
| v3.7.4 | 错误处理优化 | ✅ 已完成 | 2026-06-17 |

---

## 当前 Skills 版本

| Skill | 版本 | 主要功能 |
|-------|------|----------|
| harness-architect | v3.7.4 | 总调度、需求澄清、命令路由、历史追溯、错误处理 |
| harness-archaeology | v3.5.1 | 项目特征识别、进度反馈、历史记录 |
| harness-designer | v3.5.1 | 定制化系统生成、历史记录保存 |
| harness-onboarding | v3.7.0 | 存量代码处理、进度反馈、决策历史 |
| harness-registry | v3.6.0 | 多项目管理、权限控制、评论审批 |
| harness-validator | v3.7.2 | 5 层验证、报告生成、质量评分 |

---

## 核心功能

### 迭代优化命令
- `/harness-status` - 查看当前状态
- `/harness-adjust <配置项> <新值>` - 调整配置
- `/harness-add <检查类型>` - 添加检查项
- `/harness-remove <检查项>` - 移除检查项
- `/harness-update [scope]` - 更新到最新版本

### 历史追溯命令
- `/harness-history` - 查看决策历史
- `/harness-why <配置项>` - 解释配置原因

### 快捷别名
- `/hs` = `/harness-status`
- `/ha` = `/harness-adjust`
- `/hh` = `/harness-history`
- `/hr` = `/harness-report`
- `/hf` = `/harness-full`
- `/h` = `/harness-help`

### 错误码
- H001: PROJECT_NOT_FOUND
- H002: HARNESS_NOT_INITIALIZED
- H003: CONFIG_FORMAT_ERROR
- H004: COMMAND_SYNTAX_ERROR
- H005: INSUFFICIENT_PERMISSIONS
- H006: RESOURCE_NOT_FOUND
- H007: VERSION_INCOMPATIBLE
- H008: SKILL_NOT_AVAILABLE
- H009: CONTEXT_CORRUPTED
- H010: OPERATION_TIMEOUT

---

## 支持的语言和框架

| 语言 | 框架 | 包管理器 | 测试框架 |
|------|------|----------|----------|
| Python | FastAPI, Flask, Django | pip, poetry, uv, pdm | pytest |
| Go | Gin, Echo, Fiber, chi | go mod, golangci-lint | go test |
| TypeScript | Next.js, Nest.js, Nuxt | npm, yarn, pnpm | jest, vitest |

---

## 仓库地址

https://github.com/hsms4710-pixel/your-harness
