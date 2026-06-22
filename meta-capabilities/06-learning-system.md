# 元能力 6：学习系统

**版本**: 1.0.0  
**创建日期**: 2026-06-22  
**状态**: 生产就绪

---

## 概述

学习系统是 Your Harness 的核心能力之一，它让系统能够从每次使用中学习，持续改进生成质量。

### 核心理念

```
传统系统: 输入 → 处理 → 输出（静态）
学习系统: 输入 → 处理 → 输出 → 反馈 → 更新 → 更好的处理
```

### 学习循环

```
┌─────────────────────────────────────────────────────────────┐
│                      学习循环                                │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│   ┌─────────┐    ┌─────────┐    ┌─────────┐    ┌─────────┐  │
│   │  生成   │───▶│  使用   │───▶│  反馈   │───▶│  更新   │  │
│   └─────────┘    └─────────┘    └─────────┘    └─────────┘  │
│        ▲                                       │           │
│        └───────────────────────────────────────┘           │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## 学习阶段

### 阶段 1：生成阶段

```yaml
phase: generation

description: |
  记录生成过程中的关键信息

inputs:
  - project_features: 项目特征
  - routing_decision: 路由决策
  - template_used: 使用的模板（如有）
  - reasoning_results: 推理结果
  - final_output: 最终输出

outputs:
  - generation_record: 生成记录
  - confidence_score: 置信度评分
  - uncertainty_flags: 不确定性标记
```

### 阶段 2：使用阶段

```yaml
phase: usage

description: |
  记录用户如何使用生成的流程

tracking_points:
  - [ ] 用户是否完整阅读了流程
  - [ ] 用户是否按照流程执行
  - [ ] 用户是否跳过了某些步骤
  - [ ] 用户是否修改了流程
  - [ ] 用户是否完成了任务

outputs:
  - usage_metrics: 使用指标
  - modification_record: 修改记录
```

### 阶段 3：反馈阶段

```yaml
phase: feedback

description: |
  收集用户的反馈和评价

feedback_types:
  implicit:  # 隐式反馈
    - 完成率
    - 修改次数
    - 跳过步骤数
    - 时间花费
    
  explicit:  # 显式反馈
    - 满意度评分 (1-5)
    - 具体建议
    - 问题报告
    - 推荐标记

outputs:
  - feedback_record: 反馈记录
  - quality_score: 质量评分
```

### 阶段 4：更新阶段

```yaml
phase: update

description: |
  根据反馈更新知识库

update_targets:
  - reasoning_rules: 推理规则
  - project_patterns: 项目模式
  - template_elements: 模板元素
  - best_practices: 最佳实践

outputs:
  - knowledge_updates: 知识更新
  - promotion_candidates: 待提升的候选
```

---

## 学习数据结构

### LearningRecord（学习记录）

```yaml
learning_record:
  id: LR-2026-06-22-001
  created_at: 2026-06-22T10:30:00Z
  
  # 生成信息
  generation:
    project_id: "my-ai-agent-project"
    project_features:
      type: ai_agent
      tech_stack: [python, langchain, openai]
      complexity: medium
    routing_decision:
      mode: hybrid
      matched_templates: [TPL-001, TPL-006]
      capability_used: true
    confidence_score: 0.85
    
  # 推理信息
  reasoning:
    rules_applied:
      - rule_id: R006
        rule_name: ai_llm_integration
        confidence: 0.95
      - rule_id: R004
        rule_name: security_sensitive
        confidence: 0.70
    stages_identified:
      - api_analysis
      - security_review
      - cost_estimation
    
  # 输出信息
  output:
    skill_generated: true
    template_count: 3
    gate_count: 4
    script_count: 2
    
  # 使用反馈
  feedback:
    collected_at: 2026-06-22T15:00:00Z
    satisfaction_score: 4
    completion_rate: 0.85
    modifications:
      - type: skip_step
        step: "optional_review"
        reason: "不需要这个步骤"
      - type: add_step
        step: "custom_validation"
        description: "添加了自定义验证"
    comments:
      - "安全审查部分很详细，很有帮助"
      - "成本估算可以更详细一些"
    
  # 学习结果
  learning_outcomes:
    successful_patterns:
      - pattern: "AI Agent + 安全审查"
        score: 0.9
    areas_for_improvement:
      - area: "成本估算"
        suggestion: "添加更多成本计算细节"
    
  # 质量评分
  quality_score: 0.82
  
  # 元数据
  metadata:
    session_id: "session-12345"
    user_id: "user-001"
    harness_version: "5.2.0"
```

### LearningAggregation（学习聚合）

```yaml
learning_aggregation:
  id: LA-2026-06-22-AI-AGENT
  pattern_type: ai_agent
  created_at: 2026-06-22T18:00:00Z
  last_updated: 2026-06-22T18:00:00Z
  
  # 统计数据
  statistics:
    total_records: 15
    average_satisfaction: 4.2
    average_quality_score: 0.85
    completion_rate: 0.88
    
  # 趋势分析
  trends:
    satisfaction_trend: "up"  # up/down/stable
    modification_frequency: 0.3
    common_modifications:
      - "添加成本估算"
      - "增强安全审查"
      
  # 成功模式
  successful_patterns:
    - pattern_id: "AI-AGENT-001"
      description: "AI Agent + API 分析 + 安全审查"
      success_rate: 0.95
      usage_count: 12
      
  # 待改进点
  areas_for_improvement:
    - area: "成本估算详细度"
      priority: high
      suggested_action: "添加详细的成本计算模板"
      
  # 推广候选
  promotion_candidates:
    - candidate_id: "PC-001"
      source_record: "LR-2026-06-22-003"
      target: best_practices
      reason: "高成功率 + 用户推荐"
```

---

## 学习规则

### LR-001: 反馈收集规则

```yaml
rule_id: LR-001
rule_name: feedback_collection
priority: high

trigger:
  event: generation_complete
  condition: always

actions:
  - type: implicit_tracking
    description: "自动收集使用指标"
    metrics:
      - completion_rate
      - modification_count
      - time_spent
      
  - type: explicit_request
    description: "请求用户反馈"
    prompts:
      - "生成的流程是否有帮助？(1-5)"
      - "有哪些地方需要改进？"
      - "是否愿意推荐这个流程给其他人？"
    
  - type: modification_monitoring
    description: "监控用户的修改"
    track:
      - added_elements
      - removed_elements
      - modified_elements
```

### LR-002: 质量评分规则

```yaml
rule_id: LR-002
rule_name: quality_scoring
priority: high

input:
  - satisfaction_score: 1-5
  - completion_rate: 0-1
  - modification_count: 0-n
  - user_recommendation: yes/no
  
formula:
  quality_score = (
    satisfaction_score / 5 * 0.4 +
    completion_rate * 0.3 +
    (1 - modification_count / 5) * 0.2 +
    user_recommendation * 0.1
  ) * 100
  
  # 范围: 0-100

tiers:
  excellent: [90, 100]
  good: [75, 90)
  acceptable: [60, 75)
  needs_improvement: [0, 60)
```

### LR-003: 模式识别规则

```yaml
rule_id: LR-003
rule_name: pattern_recognition
priority: medium

trigger:
  event: feedback_collected
  condition: quality_score >= 75

actions:
  - type: extract_pattern
    description: "从成功案例中提取模式"
    extract:
      - project_features
      - routing_decision
      - successful_stages
      - effective_checks
      
  - type: check_existing_patterns
    description: "检查是否匹配已有模式"
    similarity_threshold: 0.8
    
  - type: create_new_pattern
    description: "创建新模式（如果无匹配）"
    condition: unique_pattern AND high_success_rate
```

### LR-004: 知识更新规则

```yaml
rule_id: LR-004
rule_name: knowledge_update
priority: medium

trigger:
  event: pattern_recognized
  condition: confidence >= 0.8

update_targets:
  - target: reasoning_rules
    condition: "识别出新的推理规则"
    action: add_rule
    
  - target: project_patterns
    condition: "识别出新的项目模式"
    action: add_pattern
    
  - target: template_elements
    condition: "识别出新的模板元素"
    action: add_element
    
  - target: feedbacks
    condition: "always"
    action: add_feedback
```

### LR-005: 推广规则

```yaml
rule_id: LR-005
rule_name: promotion_to_best_practice
priority: low

trigger:
  event: learning_aggregated
  condition: all(
    total_records >= 10,
    average_satisfaction >= 4.5,
    success_rate >= 0.9,
    has_user_recommendation
  )

actions:
  - type: promote_to_best_practice
    description: "将学习记录提升为最佳实践"
    target: knowledge/best-practices/
    
  - type: generate_template
    description: "生成可复用的模板"
    template_type: custom
```

---

## 学习流程

### 标准学习流程

```yaml
standard_learning_flow:
  name: default
  
  steps:
    - step: 1
      name: 生成完成
      action: trigger_learning
      
    - step: 2
      name: 收集反馈
      action: collect_feedback
      timeout: 7d  # 7 天后自动关闭
      
    - step: 3
      name: 计算质量评分
      action: calculate_quality_score
      
    - step: 4
      name: 保存学习记录
      action: save_learning_record
      path: knowledge/learnings/
      
    - step: 5
      name: 更新聚合统计
      action: update_aggregation
      path: knowledge/learnings/aggregations/
      
    - step: 6
      name: 检查推广条件
      action: check_promotion
      
    - step: 7
      name: 执行更新
      action: update_knowledge_base
      
    - step: 8
      name: 通知用户
      action: notify_user
      message: "感谢您的反馈，已帮助改进系统"
```

### 快速学习流程

```yaml
quick_learning_flow:
  name: minimal
  description: "用于简单项目，减少学习开销"
  
  steps:
    - step: 1
      name: 生成完成
      action: trigger_learning
      
    - step: 2
      name: 收集隐式反馈
      action: collect_implicit_feedback
      
    - step: 3
      name: 保存学习记录
      action: save_learning_record
      
    - step: 4
      name: 更新聚合统计
      action: update_aggregation
```

---

## 与元能力的协作

### 与元能力 1（项目分析）的协作

```yaml
collaboration:
  capability: project-analysis
  
  learning_contribution:
    - "记录新的项目特征识别模式"
    - "优化特征提取的准确性"
    
  knowledge_updates:
    - target: project_patterns
      description: "添加新的项目模式定义"
```

### 与元能力 2（阶段推理）的协作

```yaml
collaboration:
  capability: stage-reasoning
  
  learning_contribution:
    - "记录推理规则的命中率"
    - "识别推理规则的盲点"
    - "优化推理规则的权重"
    
  knowledge_updates:
    - target: reasoning_rules
      description: "添加新的推理规则或修正现有规则"
```

### 与元能力 4（知识组合）的协作

```yaml
collaboration:
  capability: knowledge-composition
  
  learning_contribution:
    - "记录成功的组合模式"
    - "识别组合冲突的解决方案"
    
  knowledge_updates:
    - target: template_elements
      description: "添加新的可复用元素"
    - target: combinations
      description: "记录成功的组合配置"
```

---

## 最佳实践

### 1. 及时收集反馈

```yaml
best_practice: timely_feedback

description: |
  反馈越及时越有价值。用户刚使用完时的记忆最清晰。

recommendations:
  - "在生成完成后立即请求反馈"
  - "设置合理的反馈超时时间（建议 7 天）"
  - "提供简化的反馈选项，降低用户负担"
```

### 2. 区分信号和噪音

```yaml
best_practice: signal_noise_separation

description: |
  不是所有反馈都需要同等对待。区分高质量和低质量反馈。

recommendations:
  - "优先处理高满意度 + 详细建议的反馈"
  - "忽略过于简短或无意义的反馈"
  - "聚合多个相似反馈后再更新知识库"
```

### 3. 渐进式推广

```yaml
best_practice: progressive_promotion

description: |
  不要急于将学习结果提升为最佳实践。确保足够的验证。

recommendations:
  - "至少 10 次成功使用后再考虑推广"
  - "推广前进行人工审核"
  - "保持学习记录的追溯性"
```

### 4. 持续监控效果

```yaml
best_practice: continuous_monitoring

description: |
  推广后的最佳实践也需要持续监控，确保仍然有效。

recommendations:
  - "定期检查最佳实践的使用率"
  - "监控用户对最佳实践的反馈"
  - "过时的最佳实践应及时降级或删除"
```

---

## 版本历史

| 版本 | 日期 | 变更 |
|------|------|------|
| 1.0.0 | 2026-06-22 | 初始版本，定义学习系统元能力 |
