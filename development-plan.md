# Harness Skills 开发计划

> 基于用户故事评估和自动化不足分析，制定的开发计划

---

## 一、开发目标

### 核心目标

将 harness skills 从"过度自动化"调整为"智能协作"：
- **机械性任务** → 自动执行
- **决策性任务** → 与用户交流
- **创造性任务** → AI 建议 + 用户确认

### 具体目标

| 目标 | 衡量标准 | 时间节点 |
|------|----------|----------|
| 需求澄清对话 | 90% 用户在生成前完成需求澄清 | v3.4 |
| 确认环节覆盖 | 80% 关键决策点有确认 | v3.4 |
| 迭代优化支持 | 支持 5 种调整命令 | v3.5 |
| 用户满意度 | 用户反馈响应时间 < 30 秒 | v3.4 |

---

## 二、开发计划总览

```
2026-06
├── Week 1: P0 - 需求澄清对话 (v3.4.0)
├── Week 2: P0 - 确认环节 (v3.4.1)
├── Week 3: P1 - 迭代优化 (v3.5.0)
└── Week 4: P2 - 历史追溯 (v3.5.1)

2026-07
├── Week 1-2: 协作功能 (v3.6.0)
└── Week 3-4: 报告功能 (v3.6.1)
```

---

## 三、P0 优化项（高优先级）

### 3.1 需求澄清对话

**问题**：当前 skills 过于自动化，不主动询问用户需求

**解决方案**：在 harness-architect 中增加需求澄清对话模块

#### 任务分解

| 任务 ID | 任务描述 | 负责 Skill | 预计工时 | 依赖 |
|---------|----------|-----------|----------|------|
| P0-1.1 | 设计需求澄清问题库 | harness-architect | 4h | - |
| P0-1.2 | 实现澄清对话流程 | harness-architect | 8h | P0-1.1 |
| P0-1.3 | 添加问题智能跳过 | harness-architect | 4h | P0-1.2 |
| P0-1.4 | 集成到 architect 路由 | harness-architect | 4h | P0-1.2 |

#### 需求澄清问题库设计

```yaml
# harness-architect/clarification-questions.yaml

project_context:
  - id: project_type
    question: "这是新项目还是已有项目？"
    options:
      - label: "新项目"
        impact: "使用 Greenfield 模式，生成标准规范"
      - label: "已有项目"
        impact: "使用 Brownfield 模式，扫描后生成定制规范"
      - label: "重构项目"
        impact: "使用 Guided Discovery 模式，逐步引导"
    skip_if: "用户明确指定了项目路径或技术栈"

  - id: team_size
    question: "团队规模多大？"
    options:
      - label: "个人项目"
        impact: "生成简化版规范，减少协作相关检查"
      - label: "小团队 (<5人)"
        impact: "生成标准规范，包含基础协作检查"
      - label: "中团队 (5-20人)"
        impact: "生成完整规范，包含代码审查和协作流程"
      - label: "大团队 (20+人)"
        impact: "生成严格规范，包含完整的质量门禁"
    default: "小团队 (<5人)"

quality_requirements:
  - id: quality_level
    question: "对代码质量的要求？"
    options:
      - label: "严格（大厂标准）"
        impact: "测试覆盖率 > 80%，所有代码必须有文档"
        gates:
          - test_coverage: ">= 80%"
          - documentation: "required"
          - code_review: "required"
      - label: "标准"
        impact: "测试覆盖率 > 60%，核心代码有文档"
        gates:
          - test_coverage: ">= 60%"
          - documentation: "core only"
          - code_review: "recommended"
      - label: "宽松（快速迭代）"
        impact: "测试覆盖率 > 40%，文档可选"
        gates:
          - test_coverage: ">= 40%"
          - documentation: "optional"
          - code_review: "optional"
    default: "标准"

ci_integration:
  - id: ci_platform
    question: "使用什么 CI 平台？"
    options:
      - label: "GitHub Actions"
        impact: "生成 GitHub Actions workflow"
        output: ".github/workflows/harness.yml"
      - label: "GitLab CI"
        impact: "生成 GitLab CI configuration"
        output: ".gitlab-ci.yml"
      - label: "Jenkins"
        impact: "生成 Jenkins pipeline script"
        output: "Jenkinsfile"
      - label: "其他/无"
        impact: "只生成脚本和 Hook，不生成 CI 配置"
        output: "none"
    default: "GitHub Actions"

  - id: ci_trigger
    question: "在什么情况下触发检查？"
    options:
      - label: "每次提交 (PR/Merge)"
        impact: "在 PR 和 Merge 时触发"
      - label: "定时检查 (每天)"
        impact: "每天定时触发"
      - label: "手动触发"
        impact: "只生成手动运行的脚本"
    default: "每次提交 (PR/Merge)"
    skip_if: "ci_platform == '其他/无'"

compliance:
  - id: security_requirements
    question: "有安全合规要求吗？"
    options:
      - label: "无特殊要求"
        impact: "不添加安全扫描检查"
      - label: "基础安全"
        impact: "添加依赖漏洞扫描"
        checks:
          - "dependency-vulnerability"
      - label: "GDPR 合规"
        impact: "添加数据保护相关检查"
        checks:
          - "dependency-vulnerability"
          - "sensitive-data"
          - "privacy-check"
      - label: "等保合规"
        impact: "添加完整的安全检查"
        checks:
          - "dependency-vulnerability"
          - "sensitive-data"
          - "access-control"
          - "audit-log"
    default: "无特殊要求"
```

#### 验收标准

- [ ] 用户发起请求后，自动开始需求澄清对话
- [ ] 每个问题提供清晰的选项和影响说明
- [ ] 用户可以跳过不想回答的问题（使用默认值）
- [ ] 澄清完成后，生成个性化配置摘要供确认

---

### 3.2 确认环节

**问题**：推断结果不与用户确认，可能基于错误假设

**解决方案**：在关键决策点增加确认环节

#### 任务分解

| 任务 ID | 任务描述 | 负责 Skill | 预计工时 | 依赖 |
|---------|----------|-----------|----------|------|
| P0-2.1 | 设计确认点模板 | harness-archaeology | 4h | - |
| P0-2.2 | 实现推断结果确认 | harness-archaeology | 6h | P0-2.1 |
| P0-2.3 | 实现选项提供机制 | harness-designer | 6h | - |
| P0-2.4 | 实现确认后生成 | harness-designer | 4h | P0-2.2, P0-2.3 |

#### 确认点设计

```yaml
# harness-archaeology/confirmation-points.yaml

inference_confirmation:
  - id: project_features
    trigger: "扫描完成后"
    message_template: |
      我识别到以下项目特征：
      - 语言: {language}
      - 框架: {framework}
      - 包管理器: {package_manager}
      - 测试框架: {test_framework}
      - CI 平台: {ci_platform}
      
      是否正确？有需要补充或修正的吗？
    wait_for_user: true
    allow_modification: true

  - id: legacy_code
    trigger: "发现存量代码问题时"
    message_template: |
      扫描发现 {count} 个文件不符合规范：
      - 代码风格问题: {style_count} 个
      - 类型错误: {type_count} 个
      - 测试缺失: {test_count} 个
      
      您希望如何处理？
    options:
      - label: "立即全部修复"
        impact: "生成修复脚本，立即修复所有问题"
      - label: "渐进式收紧（3个月）"
        impact: "生成收紧计划，分阶段修复"
      - label: "暂不处理，只对新代码强制"
        impact: "只对新提交的代码进行检查"
    wait_for_user: true

  - id: special_conventions
    trigger: "发现可能的特殊约定时"
    message_template: |
      我注意到项目中可能有以下特殊约定：
      {conventions}
      
      这些是否需要在规范中体现？
    options:
      - label: "是，全部纳入"
      - label: "部分纳入"
      - label: "不需要"
    wait_for_user: true

  - id: priority_confirmation
    trigger: "准备生成时"
    message_template: |
      基于您的需求和项目特征，我建议生成以下内容：
      
      **高优先级**:
      {high_priority_items}
      
      **中优先级**:
      {medium_priority_items}
      
      **低优先级**:
      {low_priority_items}
      
      您希望调整优先级吗？
    options:
      - label: "按建议生成"
      - label: "调整优先级"
      - label: "只生成高优先级"
    wait_for_user: true
```

#### 验收标准

- [ ] 扫描完成后，展示推断结果并等待确认
- [ ] 发现存量代码问题时，提供处理选项
- [ ] 生成前确认优先级
- [ ] 用户可以选择确认或修改

---

## 四、P1 优化项（中优先级）

### 4.1 迭代优化

**问题**：生成后不支持优化，不能适应需求变化

**解决方案**：添加调整命令支持

#### 任务分解

| 任务 ID | 任务描述 | 负责 Skill | 预计工时 | 依赖 |
|---------|----------|-----------|----------|------|
| P1-1.1 | 设计调整命令语法 | harness-designer | 4h | - |
| P1-1.2 | 实现 /harness-adjust | harness-designer | 6h | P1-1.1 |
| P1-1.3 | 实现 /harness-add | harness-designer | 6h | P1-1.1 |
| P1-1.4 | 实现 /harness-remove | harness-designer | 4h | P1-1.1 |
| P1-1.5 | 实现 /harness-update | harness-designer | 4h | P1-1.1 |
| P1-1.6 | 实现 /harness-status | harness-architect | 4h | - |

#### 调整命令设计

```yaml
# harness-designer/adjust-commands.yaml

commands:
  - command: "/harness-adjust"
    description: "调整生成的配置"
    syntax: "/harness-adjust <配置项> <新值>"
    examples:
      - "/harness-adjust 测试覆盖率 70%"
      - "/harness-adjust 代码审查 必须"
      - "/harness-adjust 文档要求 可选"
    implementation:
      - parse_command
      - find_config_item
      - validate_new_value
      - update_config
      - regenerate_output

  - command: "/harness-add"
    description: "添加新的检查项"
    syntax: "/harness-add <检查类型> [配置]"
    examples:
      - "/harness-add 安全扫描"
      - "/harness-add API版本检查 --strict"
      - "/harness-add 依赖漏洞检查"
    available_checks:
      - name: "安全扫描"
        description: "添加安全相关检查"
        checks:
          - "sensitive-data"
          - "dependency-vulnerability"
          - "access-control"
      - name: "性能检查"
        description: "添加性能相关检查"
        checks:
          - "bundle-size"
          - "query-optimization"
      - name: "API版本检查"
        description: "添加 API 版本兼容性检查"
        checks:
          - "api-compatibility"
          - "api-deprecation"
    implementation:
      - parse_command
      - select_check_type
      - configure_checks
      - add_to_config
      - regenerate_output

  - command: "/harness-remove"
    description: "移除不需要的检查项"
    syntax: "/harness-remove <检查项>"
    examples:
      - "/harness-remove 类型检查"
      - "/harness-remove 代码覆盖率"
    implementation:
      - parse_command
      - find_check_item
      - confirm_removal
      - remove_from_config
      - regenerate_output

  - command: "/harness-update"
    description: "更新到最新版本"
    syntax: "/harness-update [scope]"
    examples:
      - "/harness-update"  # 更新所有
      - "/harness-update scripts"  # 只更新脚本
      - "/harness-update hooks"  # 只更新 Hook
    implementation:
      - check_current_version
      - fetch_latest_templates
      - merge_with_customizations
      - update_config
      - regenerate_output

  - command: "/harness-status"
    description: "查看当前 Harness 状态"
    syntax: "/harness-status"
    examples:
      - "/harness-status"
    output:
      - "项目: {project_name}"
      - "语言: {language}"
      - "框架: {framework}"
      - "生成时间: {timestamp}"
      - "版本: {version}"
      - "检查项数量: {count}"
      - "上次运行: {last_run}"
      - "状态: {status}"
```

#### 验收标准

- [ ] 支持 5 种调整命令
- [ ] 命令执行后自动更新配置
- [ ] 更新后自动重新生成输出
- [ ] /harness-status 显示完整状态信息

---

### 4.2 进度反馈

**问题**：大型项目扫描时用户不知道进度

**解决方案**：添加实时进度反馈

#### 任务分解

| 任务 ID | 任务描述 | 负责 Skill | 预计工时 | 依赖 |
|---------|----------|-----------|----------|------|
| P1-2.1 | 设计进度反馈格式 | harness-archaeology | 2h | - |
| P1-2.2 | 实现扫描进度反馈 | harness-archaeology | 4h | P1-2.1 |
| P1-2.3 | 实现生成进度反馈 | harness-designer | 4h | P1-2.1 |

#### 进度反馈设计

```yaml
# progress-feedback.yaml

scan_progress:
  format: "[{current}/{total}] {stage}..."
  stages:
    - id: 1
      name: "扫描项目结构"
      description: "分析目录结构和文件分布"
    - id: 2
      name: "识别语言和框架"
      description: "检测编程语言、框架、包管理器"
    - id: 3
      name: "分析配置文件"
      description: "解析项目配置和依赖关系"
    - id: 4
      name: "检测工具链"
      description: "识别 Lint、测试、CI 工具"
    - id: 5
      name: "生成特征报告"
      description: "汇总分析结果"

  examples:
    - "[1/5] 扫描项目结构..."
    - "[2/5] 识别语言和框架..."
    - "[3/5] 分析配置文件..."
    - "[4/5] 检测工具链..."
    - "[5/5] 生成特征报告..."

generation_progress:
  format: "[{current}/{total}] 生成 {item}..."
  stages:
    - id: 1
      name: "Constitution"
      description: "生成项目治理原则"
    - id: 2
      name: "Scripts"
      description: "生成定制脚本"
    - id: 3
      name: "Hooks"
      description: "生成自动化 Hook"
    - id: 4
      name: "Skills"
      description: "生成可调用 Skill"
    - id: 5
      name: "Workflows"
      description: "生成工作流文档"
    - id: 6
      name: "CI Configuration"
      description: "生成 CI 配置"

  examples:
    - "[1/6] 生成 Constitution..."
    - "[2/6] 生成 Scripts..."
    - "[3/6] 生成 Hooks..."
```

#### 验收标准

- [ ] 扫描过程显示实时进度
- [ ] 生成过程显示实时进度
- [ ] 进度信息清晰易懂

---

## 五、P2 优化项（低优先级）

### 5.1 历史追溯

**问题**：不知道之前的决策，无法解释为什么这样配置

**解决方案**：记录决策历史

#### 任务分解

| 任务 ID | 任务描述 | 负责 Skill | 预计工时 | 依赖 |
|---------|----------|-----------|----------|------|
| P2-1.1 | 设计决策记录格式 | harness-architect | 4h | - |
| P2-1.2 | 实现决策记录保存 | harness-architect | 4h | P2-1.1 |
| P2-1.3 | 实现决策历史查询 | harness-architect | 4h | P2-1.2 |

#### 决策记录设计

```yaml
# .harness/decision-history.yaml

version: "1.0"
created_at: "2026-06-16T10:00:00Z"

initial_request:
  user_input: "帮我给这个 FastAPI 项目生成 Harness"
  detected_mode: "Brownfield"
  timestamp: "2026-06-16T10:00:00Z"

clarification_answers:
  - question: "团队规模多大？"
    answer: "小团队 (<5人)"
    impact: "生成标准规范，包含基础协作检查"
    timestamp: "2026-06-16T10:00:15Z"
    
  - question: "对代码质量的要求？"
    answer: "标准"
    impact: "测试覆盖率 > 60%，核心代码有文档"
    timestamp: "2026-06-16T10:00:22Z"
    
  - question: "使用什么 CI 平台？"
    answer: "GitHub Actions"
    impact: "生成 GitHub Actions workflow"
    timestamp: "2026-06-16T10:00:28Z"

scan_results:
  project_features:
    language: "Python"
    framework: "FastAPI"
    package_manager: "PDM"
    test_framework: "pytest"
    ci_platform: "GitHub Actions"
    
  legacy_code:
    total_files: 1121
    non_compliant: 45
    breakdown:
      style_issues: 30
      type_errors: 10
      missing_tests: 5

  user_decision:
    choice: "渐进式收紧（3个月）"
    reason: "不想一次性修复太多，慢慢来"
    timestamp: "2026-06-16T10:01:00Z"

generation_config:
  priority_decisions:
    high:
      - "check-pydantic-sync"
      - "check-router-params"
      reason: "FastAPI 项目核心检查"
    
    medium:
      - "check-tdd"
      - "check-docstrings"
      reason: "用户选择了标准质量要求"
    
    low:
      - "check-performance"
      reason: "非核心功能"

adjustments:
  - id: 1
    command: "/harness-adjust 测试覆盖率 70%"
    previous_value: ">= 60%"
    new_value: ">= 70%"
    timestamp: "2026-06-16T11:00:00Z"
    
  - id: 2
    command: "/harness-add 安全扫描"
    added_items:
      - "sensitive-data"
      - "dependency-vulnerability"
    timestamp: "2026-06-16T11:30:00Z"

summary:
  total_decisions: 10
  user_interactions: 8
  auto_decisions: 2
  adjustments_made: 2
  last_updated: "2026-06-16T11:30:00Z"
```

#### 验收标准

- [ ] 记录所有用户决策
- [ ] 记录所有调整操作
- [ ] 支持查询决策历史
- [ ] 能够解释为什么这样配置

---

### 5.2 协作功能

**问题**：不支持团队协作，多人维护时容易冲突

**解决方案**：添加团队协作支持

#### 任务分解

| 任务 ID | 任务描述 | 负责 Skill | 预计工时 | 依赖 |
|---------|----------|-----------|----------|------|
| P2-2.1 | 设计协作权限模型 | harness-registry | 6h | - |
| P2-2.2 | 实现角色权限控制 | harness-registry | 8h | P2-2.1 |
| P2-2.3 | 实现评论和审批 | harness-registry | 8h | P2-2.1 |

---

## 六、测试计划

### 6.1 测试项目

| 项目 | 语言 | 用途 |
|------|------|------|
| FastAPI | Python | Python 项目测试 |
| Gin | Go | Go 项目测试 |
| Next.js | TypeScript | TypeScript 项目测试 |
| Nx | TypeScript | 复杂 Monorepo 测试 |

### 6.2 测试用例

```yaml
test_cases:
  - id: TC-001
    name: "需求澄清对话 - 新项目"
    input: "帮我生成一个新项目的 Harness"
    expected:
      - "开始需求澄清对话"
      - "询问项目类型"
      - "询问团队规模"
      - "询问质量要求"
    
  - id: TC-002
    name: "需求澄清对话 - 已有项目"
    input: "帮我给这个项目生成 Harness"
    context: "FastAPI 项目路径"
    expected:
      - "开始扫描项目"
      - "展示推断结果"
      - "等待用户确认"
      
  - id: TC-003
    name: "调整命令 - 修改配置"
    input: "/harness-adjust 测试覆盖率 70%"
    expected:
      - "解析命令"
      - "更新配置"
      - "重新生成输出"
      - "显示更新结果"

  - id: TC-004
    name: "调整命令 - 添加检查"
    input: "/harness-add 安全扫描"
    expected:
      - "解析命令"
      - "添加安全扫描检查"
      - "重新生成输出"
      - "显示新增内容"
```

---

## 七、风险评估

| 风险 | 影响 | 概率 | 缓解措施 |
|------|------|------|----------|
| 需求澄清问题过多，用户疲劳 | 高 | 中 | 支持跳过，使用默认值 |
| 确认环节过多，流程变慢 | 中 | 中 | 智能判断何时需要确认 |
| 调整命令语法复杂 | 中 | 低 | 提供自然语言支持 |
| 历史记录占用空间 | 低 | 低 | 定期清理，只保留关键决策 |

---

## 八、版本规划

| 版本 | 内容 | 时间 |
|------|------|------|
| v3.4.0 | 需求澄清对话 | 2026-06-XX |
| v3.4.1 | 确认环节 | 2026-06-XX |
| v3.4.2 | 进度反馈 | 2026-06-XX |
| v3.5.0 | 迭代优化命令 | 2026-06-XX |
| v3.5.1 | 历史追溯 | 2026-06-XX |
| v3.6.0 | 协作功能 | 2026-07-XX |
| v3.6.1 | 报告功能 | 2026-07-XX |

---

## 九、成功指标

### 技术指标

| 指标 | 当前 | 目标 | 衡量方式 |
|------|------|------|----------|
| 需求澄清覆盖率 | 0% | 90% | 用户完成澄清对话的比例 |
| 确认环节覆盖率 | 0% | 80% | 关键决策点有确认的比例 |
| 调整命令可用性 | 0 | 5 | 支持的调整命令数量 |
| 历史追溯完整性 | 0% | 100% | 决策记录的完整性 |

### 用户指标

| 指标 | 当前 | 目标 | 衡量方式 |
|------|------|------|----------|
| 用户满意度 | - | > 4.5/5 | 用户反馈评分 |
| 生成准确度 | 94% | 97% | 用户确认推断结果的比例 |
| 调整频率 | 0 | < 3次/项目 | 用户调整生成结果的平均次数 |
| 学习时间 | 30min | < 5min | 新用户上手时间 |

---

## 十、下一步行动

1. **立即开始**：实现 P0-1.1 需求澄清问题库设计
2. **本周完成**：P0 需求澄清对话
3. **下周完成**：P0 确认环节
4. **第三周**：P1 迭代优化命令
5. **持续优化**：根据用户反馈调整

---

## 附录

### A. 相关文档

- `user-story-evaluation.md` - 用户故事评估
- `automation-gaps-analysis.md` - 自动化不足分析
- `harness-advantages-demo.md` - Harness 优势展示

### B. 参考资料

- Spec Kit - Constitution 设计
- Superpowers - 7 阶段工作流
- OpenSpec - Specs 格式
