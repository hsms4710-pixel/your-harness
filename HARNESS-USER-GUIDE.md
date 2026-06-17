# Harness Skills 用户指南

## 目录

- [什么是 Harness Skills](#什么是-harness-skills)
- [快速开始](#快速开始)
- [Skills 概览](#skills-概览)
- [命令参考](#命令参考)
- [常见场景](#常见场景)
- [最佳实践](#最佳实践)
- [常见问题](#常见问题)
- [版本信息](#版本信息)

---

## 什么是 Harness Skills

Harness Skills 是一套 AI 驱动的工程规范生成工具，能够：

1. **自动识别**项目特征（语言、框架、构建系统、依赖管理等）
2. **智能生成**定制化的工程规范系统（脚本、Hook、Skill、工作流）
3. **持续迭代**支持调整、优化、版本升级
4. **团队协作**支持多项目管理、权限控制、分享功能

### 与通用工具的区别

| 特性 | 通用工具 (Spec Kit/OpenSpec) | Harness Skills |
|------|------------------------------|----------------|
| 输出 | 推荐文档 + 通用 Skill | 可执行脚本 + 自动触发 Hook + 定制 Skill + 完整工作流 |
| 适用性 | 通用模板 | 基于项目代码推断，完全定制 |
| 迭代 | 手动修改 | 内置迭代命令，一键调整 |
| 历史 | 无 | 完整决策记录，支持追溯 |

---

## 快速开始

### 方式一：自然语言（推荐）

直接告诉 AI 你想做什么：

```
"帮我给这个项目生成工程规范"
"检查一下代码规范"
"我有个老项目，存量代码不达标怎么办"
"我有多个项目，怎么统一管理"
```

### 方式二：命令行

使用斜杠命令触发特定功能：

```
/harness-full        # 完整流程（生成 + 验证）
/harness-status      # 查看当前状态
/harness-history     # 查看决策历史
/harness-help        # 查看帮助
```

### 首次使用流程

```
1. AI 询问项目信息（类型、团队规模、质量要求等）
   ↓
2. AI 扫描项目，识别技术栈和特征
   ↓
3. AI 展示识别结果，等待你确认
   ↓
4. AI 生成定制化的 Harness 系统
   ↓
5. AI 验证生成结果，输出质量报告
```

---

## Skills 概览

| Skill | 版本 | 功能 | 触发方式 |
|-------|------|------|----------|
| harness-architect | v3.7.4 | 总调度器，智能路由 | 自然语言 |
| harness-archaeology | v3.5.1 | 识别项目特征 | "分析项目" |
| harness-designer | v3.5.1 | 生成定制系统 | "生成规范" |
| harness-validator | v3.7.2 | 验证质量，生成报告 | "检查规范" |
| harness-onboarding | v3.7.0 | 处理存量代码 | "老项目迁移" |
| harness-registry | v3.6.0 | 多项目管理 | "多项目管理" |

---

## 命令参考

### 状态查询命令

| 命令 | 别名 | 功能 | 示例 |
|------|------|------|------|
| /harness-status | /hs | 查看当前状态 | `/harness-status` |
| /harness-history | /hh | 查看决策历史 | `/harness-history --recent` |
| /harness-why | - | 解释配置原因 | `/harness-why lint-rules` |
| /onboarding-status | - | 查看 Onboarding 状态 | `/onboarding-status` |
| /onboarding-history | - | 查看 Onboarding 历史 | `/onboarding-history` |

### 迭代优化命令

| 命令 | 别名 | 功能 | 示例 |
|------|------|------|------|
| /harness-adjust | /ha | 调整配置 | `/harness-adjust max-line-length 120` |
| /harness-add | - | 添加检查项 | `/harness-add security-scan` |
| /harness-remove | - | 移除检查项 | `/harness-remove type-check` |
| /harness-update | - | 更新到最新版本 | `/harness-update scripts` |

### 报告命令

| 命令 | 别名 | 功能 | 示例 |
|------|------|------|------|
| /harness-report | /hr | 生成报告 | `/harness-report --format html --trend` |

### 完整流程命令

| 命令 | 别名 | 功能 | 示例 |
|------|------|------|------|
| /harness-full | /hf | 完整生成流程 | `/harness-full --skip-clarification` |
| /harness-help | /h | 显示帮助 | `/harness-help adjust` |
| /harness-error | - | 查询错误码 | `/harness-error H002` |

### 协作命令

| 命令 | 功能 | 示例 |
|------|------|------|
| /registry-share | 分享项目 | `/registry-share my-project --to team@example.com` |
| /registry-permissions | 管理权限 | `/registry-permissions` |
| /registry-approve | 审批变更 | `/registry-approve 123 --comment "LGTM"` |

### 错误码

| 错误码 | 说明 | 解决方案 |
|--------|------|----------|
| H001 | 项目路径不存在 | 检查项目路径是否正确 |
| H002 | 项目未初始化 Harness | 先运行 /harness-full |
| H003 | 配置文件格式不正确 | 检查 YAML 语法 |
| H004 | 命令参数不正确 | 运行 /harness-help 查看用法 |
| H005 | 没有执行操作的权限 | 联系项目管理员 |
| H006 | 指定的资源不存在 | 检查资源名称是否正确 |
| H007 | Harness 版本不兼容 | 运行 /harness-update |
| H008 | Skill 不可用 | 检查 Skill 是否已安装 |
| H009 | 上下文文件损坏 | 删除 .harness 目录重新生成 |
| H010 | 操作超时 | 检查网络连接或项目大小 |

---

## 常见场景

### 场景 1：新项目，从零开始

**描述**：你有一个新项目，想要建立工程规范。

**操作**：
```
用户: "我有一个新的 Python FastAPI 项目，帮我生成工程规范"
```

**AI 会**：
1. 询问项目详情（团队规模、质量要求、CI 平台等）
2. 扫描项目，识别技术栈
3. 生成定制化的 Harness 系统
4. 输出配置摘要，等待确认

---

### 场景 2：老项目，存量代码不达标

**描述**：项目已有规范，但存量代码不符合要求。

**操作**：
```
用户: "我有个老项目，存量代码不达标怎么办"
```

**AI 会**：
1. 生成现状快照，统计不合规代码
2. 制定渐进收紧计划（4 周适应期）
3. 设置豁免清单
4. 定期收紧检查标准

---

### 场景 3：检查代码规范

**描述**：想检查当前代码是否符合规范。

**操作**：
```
用户: "检查一下代码规范"
```

**AI 会**：
1. 运行 5 层验证
2. 输出质量报告
3. 标记不符合项
4. 提供修复建议

---

### 场景 4：调整配置

**描述**：想修改某个配置项。

**操作**：
```
用户: /harness-adjust max-line-length 100
```

**AI 会**：
1. 更新配置
2. 记录决策历史
3. 确认修改结果

---

### 场景 5：多项目管理

**描述**：管理多个项目的 Harness。

**操作**：
```
用户: "我有多个项目，怎么统一管理"
```

**AI 会**：
1. 注册项目到 Registry
2. 检测项目间 Drift
3. 支持共享资源配置
4. 提供统一管理界面

---

## 最佳实践

### 1. 首次使用

- ✅ 让 AI 询问项目信息，提供准确答案
- ✅ 仔细检查 AI 识别的项目特征
- ✅ 确认生成结果后再使用

### 2. 日常使用

- ✅ 使用 `/harness-status` 定期检查状态
- ✅ 使用 `/harness-report` 生成质量报告
- ✅ 重要修改前使用 `/harness-why` 了解原因

### 3. 团队协作

- ✅ 使用 `/registry-share` 分享项目
- ✅ 设置合适的权限级别
- ✅ 使用评论和审批功能

### 4. 版本管理

- ✅ 定期运行 `/harness-update` 保持最新
- ✅ 使用 `/harness-history` 追溯决策
- ✅ 重要变更前备份 `.harness` 目录

---

## 常见问题

### Q: Harness 生成的文件在哪里？

A: 所有生成的文件都在项目的 `.harness/` 目录下：
```
.harness/
├── config.yaml          # 主配置
├── context.yaml         # 上下文（Skills 间共享）
├── decision-history.yaml # 决策历史
├── scripts/             # 定制脚本
├── hooks/               # Git Hooks
├── skills/              # 定制 Skills
├── gates/               # 质量门禁
└── workflows/           # 工作流
```

### Q: 如何恢复到之前的配置？

A: 使用 `/harness-history` 查看历史，找到要恢复的版本，然后手动修改或使用 `/harness-adjust` 逐项恢复。

### Q: 支持哪些语言和框架？

A: 当前支持：
- **Python**: FastAPI, Flask, Django, PDM, Poetry, pip
- **Go**: Gin, Echo, Fiber, golangci-lint v2
- **TypeScript**: Next.js, React, Nest.js, pnpm, yarn, npm, Turborepo

### Q: 如何在 CI/CD 中使用？

A: Harness 会生成 CI 配置文件，支持：
- GitHub Actions
- GitLab CI
- Jenkins
- 工蜂 CI

### Q: 可以自定义生成的规范吗？

A: 可以。使用 `/harness-adjust` 调整配置，或在生成前告诉 AI 你的特殊需求。

### Q: 团队成员能看到我的决策历史吗？

A: 取决于权限设置。使用 `/registry-permissions` 管理权限。

---

## 版本信息

### 当前版本

| Skill | 版本 | 发布日期 |
|-------|------|----------|
| harness-architect | v3.7.4 | 2026-06-17 |
| harness-archaeology | v3.5.1 | 2026-06-17 |
| harness-designer | v3.5.1 | 2026-06-17 |
| harness-onboarding | v3.7.0 | 2026-06-17 |
| harness-registry | v3.6.0 | 2026-06-17 |
| harness-validator | v3.7.2 | 2026-06-17 |

### 功能版本对照

| 功能 | 版本 |
|------|------|
| 需求澄清对话 | v3.4.0 |
| 确认环节 | v3.4.1 |
| 迭代优化命令 | v3.5.0 |
| 历史追溯 | v3.5.1 |
| 协作功能 | v3.6.0 |
| 报告功能 | v3.6.1 |
| Onboarding 同步 | v3.7.0 |
| 统一上下文共享 | v3.7.1 |
| 报告增强 | v3.7.2 |
| CLI 体验优化 | v3.7.3 |
| 错误处理优化 | v3.7.4 |

### GitHub 仓库

https://github.com/hsms4710-pixel/your-harness

---

## 附录：完整命令速查表

```
# 状态查询
/hs, /harness-status          查看当前状态
/hh, /harness-history         查看决策历史
/harness-why <item>           解释配置原因
/onboarding-status            查看 Onboarding 状态
/onboarding-history           查看 Onboarding 历史

# 迭代优化
/ha, /harness-adjust <key> <value>  调整配置
/harness-add <type>           添加检查项
/harness-remove <item>        移除检查项
/harness-update [scope]       更新版本

# 报告
/hr, /harness-report          生成报告

# 流程
/hf, /harness-full            完整流程
/h, /harness-help             显示帮助
/harness-error <code>         查询错误码

# 协作
/registry-share <project>     分享项目
/registry-permissions         管理权限
/registry-approve <id>        审批变更
```
