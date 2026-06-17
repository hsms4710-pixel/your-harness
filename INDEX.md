# Harness Skills 命令速查表

## 快捷别名

| 别名 | 完整命令 | 功能 |
|------|----------|------|
| `/hs` | `/harness-status` | 查看当前状态 |
| `/ha` | `/harness-adjust` | 调整配置 |
| `/hh` | `/harness-history` | 查看决策历史 |
| `/hr` | `/harness-report` | 生成报告 |
| `/hf` | `/harness-full` | 完整流程 |
| `/h` | `/harness-help` | 显示帮助 |

## 状态查询

```bash
/harness-status              # 查看当前状态
/harness-history             # 查看决策历史
/harness-history --recent    # 只看最近记录
/harness-why <配置项>        # 解释配置原因
/onboarding-status           # 查看 Onboarding 状态
/onboarding-history          # 查看 Onboarding 历史
```

## 迭代优化

```bash
/harness-adjust <key> <value>   # 调整配置
/harness-add <检查类型>         # 添加检查项
/harness-remove <检查项>        # 移除检查项
/harness-update [scope]         # 更新版本
```

## 报告

```bash
/harness-report                  # 生成报告
/harness-report --format html    # HTML 格式
/harness-report --format pdf     # PDF 格式
/harness-report --trend          # 包含趋势分析
```

## 完整流程

```bash
/harness-full                    # 完整流程
/harness-full --skip-clarification  # 跳过需求澄清
/harness-full --skip-validation     # 跳过验证
```

## 帮助

```bash
/harness-help                    # 显示所有命令帮助
/harness-help <command>          # 显示特定命令帮助
/harness-error <code>            # 查询错误码说明
```

## 协作（Registry）

```bash
/registry-share <project>        # 分享项目
/registry-share <project> --to email  # 分享给指定邮箱
/registry-permissions            # 管理权限
/registry-approve <id>           # 审批变更
/registry-approve <id> --comment "LGTM"  # 带评论审批
```

## 错误码

| 错误码 | 说明 |
|--------|------|
| H001 | 项目路径不存在 |
| H002 | 项目未初始化 Harness |
| H003 | 配置文件格式不正确 |
| H004 | 命令参数不正确 |
| H005 | 没有执行操作的权限 |
| H006 | 指定的资源不存在 |
| H007 | Harness 版本不兼容 |
| H008 | Skill 不可用 |
| H009 | 上下文文件损坏 |
| H010 | 操作超时 |

## 自然语言触发示例

```
"帮我给这个项目生成工程规范"
"检查一下代码规范"
"我有个老项目，存量代码不达标怎么办"
"我有多个项目，怎么统一管理"
"新项目，用 Python + FastAPI"
"调整一下最大行长度为 100"
"为什么要有这个 lint 规则"
"生成一份质量报告"
```

## 版本信息

| Skill | 版本 |
|-------|------|
| harness-architect | v3.7.4 |
| harness-archaeology | v3.5.1 |
| harness-designer | v3.5.1 |
| harness-onboarding | v3.7.0 |
| harness-registry | v3.6.0 |
| harness-validator | v3.7.2 |
