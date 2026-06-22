---
name: harness-designer
description: |
  Harness Designer — 基于智能优化的自适应开发流程生成器。
  
  核心能力（v6.0.0 智能优化）：
  - 元模板：已知模式的高效匹配
  - 元能力：未知场景的智能推理
  - 元素库：可复用的流程元素
  - 智能路由：自动选择最佳模式
  - 组合引擎：智能组合多来源内容
  - 学习系统：从每次使用中学习
  - 知识积累：持续改进知识库
  - 规则优化：自动优化推理规则
  - 个性化适配：基于用户偏好的个性化
  - 预测性建议：主动预测和建议
  
  智能优化公式：
  智能优化 = 混合模式 + 学习系统 + 规则优化 + 个性化 + 预测
  
  核心思想：
  - 简单项目：使用流程骨架
  - 已知模式：使用元模板
  - 混合场景：组合元模板 + 元能力
  - 全新场景：使用元能力推理
  - 持续学习：从每次使用中积累
  - 规则优化：自动改进推理规则
  - 个性化：适应每个用户
  - 预测：主动提供建议
  
  知识库文件：
  - knowledge/reasoning-rules.yaml: 通用推理规则
  - knowledge/flow-skeleton.yaml: 流程骨架
  - knowledge/routing-rules.yaml: 智能路由规则
  - knowledge/combination-engine.yaml: 组合引擎规则
  - knowledge/feedback-schema.yaml: 反馈收集定义
  - knowledge/knowledge-update-rules.yaml: 知识更新规则
  - knowledge/promotion-rules.yaml: 知识提升规则
  - knowledge/rule-optimizer.yaml: 规则优化器（v6.0 新增）
  - knowledge/personalization.yaml: 个性化适配器（v6.0 新增）
  - knowledge/prediction-engine.yaml: 预测性建议引擎（v6.0 新增）
  
  元素库：
  - element-library/element-library.yaml: 元素库定义
  - element-library/templates/: 模板元素
  - element-library/gates/: 闸门元素
  - element-library/scripts/: 脚本元素
  - element-library/references/: 参考元素
  - element-library/checks/: 检查元素
  
  学习系统：
  - meta-capabilities/06-learning-system.md: 学习系统元能力
  - knowledge/learnings/: 学习记录
  - knowledge/feedbacks/: 反馈数据
  - knowledge/best-practices/: 最佳实践
  - knowledge/patterns/: 验证模式
  
  v6.0.0: 实现智能优化
  v5.2.0: 实现知识积累系统
  v5.1.0: 实现混合模式
  v5.0.0: 元能力驱动
  v4.6.0: 知识积累机制
  v4.0.0: 开发流程体系生成器
version: 6.0.0
author: agent_created
tags: [harness, designer, workflow, hybrid-mode, routing, combination, meta-template, meta-capability, element-library, learning-system, knowledge-accumulation, rule-optimization, personalization, prediction]
---

# Harness Designer (v6.0.0 智能优化)

基于智能优化的自适应开发流程生成器。

## 核心定位

```
这是一个基于智能优化的自适应开发流程生成器。

v6.0 智能优化 = 混合模式 + 学习系统 + 规则优化 + 个性化 + 预测

关键转变（v5.2 → v6.0）：
  - v5.2: 知识积累，持续改进
  - v6.0: 智能优化，自适应生成

智能优化的优势：
  - 推理规则自动优化
  - 适应每个用户的偏好
  - 主动提供建议
  - 越用越精准

优化循环：
  生成 → 使用 → 反馈 → 学习 → 优化 → 更好的生成
```

## 触发条件

- 接收 harness-archaeology 的项目特征识别结果
- 基于智能路由选择生成策略
- 基于用户画像进行个性化适配
- 输出定制化的开发流程体系
- 收集反馈并更新知识库
- 应用规则优化和预测建议

---

## 智能优化生成流程

### Step -1: 用户画像加载（v6.0 新增）

```yaml
# 引用: knowledge/personalization.yaml

user_profile_loading:
  name: "用户画像加载"
  
  # 获取用户画像
  profile_source:
    - "explicit_preferences"  # 用户显式设置
    - "inferred_preferences"  # 从行为推断
    - "team_preferences"      # 团队偏好（如果有）
    
  # 用户偏好维度
  preference_dimensions:
    - detail_level: "详细程度"
      options: [minimal, standard, detailed, exhaustive]
      
    - guidance_style: "指导风格"
      options: [directive, suggestive, exploratory, adaptive]
      
    - automation_level: "自动化程度"
      options: [manual, semi_auto, auto, full_auto]
      
    - risk_tolerance: "风险容忍度"
      options: [conservative, balanced, aggressive]
      
    - documentation_preference: "文档偏好"
      options: [minimal, essential, comprehensive, exhaustive]
  
  # 输出
  output:
    user_profile:
      explicit_preferences: {}
      inferred_preferences: {}
      team_overrides: {}
      
    adaptation_rules_to_apply: []
```

### Step 0: 智能路由

```yaml
# 引用: knowledge/routing-rules.yaml

routing_decision:
  name: "智能路由决策"
  
  # 路由规则
  rules:
    - rule_id: "RT-001"
      name: "full-template-match"
      condition: "完美匹配元模板"
      route: "template-based"
      confidence: "high"
      
    - rule_id: "RT-002"
      name: "template-combination"
      condition: "可组合多个元模板"
      route: "template-combination"
      confidence: "medium"
      
    - rule_id: "RT-003"
      name: "partial-template-match"
      condition: "部分匹配，有未覆盖特征"
      route: "hybrid"
      confidence: "medium"
      
    - rule_id: "RT-004"
      name: "no-template-match"
      condition: "无匹配模板"
      route: "capability-driven"
      confidence: "medium"
      
    - rule_id: "RT-005"
      name: "simple-project"
      condition: "简单项目，无特殊特征"
      route: "skeleton-only"
      confidence: "high"
  
  # 路由输出
  output:
    selected_route: "template-based | template-combination | hybrid | capability-driven | skeleton-only"
    matched_templates: []
    unmatched_features: []
    confidence: "high | medium | low"
```

### Step 1: 项目特征提取（元能力 1）

```yaml
# 引用: meta-capabilities/01-project-analysis.md

capability: project-analysis

# 从项目代码、配置文件、目录结构中提取结构化的项目特征
dimensions:
  - tech_stack: "技术维度（语言、框架、工具链）"
  - architecture: "架构维度（项目类型、架构模式）"
  - business_context: "业务维度（AI/LLM、安全、性能）"
  - team_context: "团队维度（代码风格、文档、规范）"

# 输出
output:
  identified_features:
    - name: "ai-llm-project"
      confidence: "high"
      signals: ["langchain import", "openai api key"]
      
    - name: "security-sensitive"
      confidence: "medium"
      signals: ["password handling"]
```

### Step 2: 元模板匹配（如果路由选择模板）

```yaml
# 元模板匹配
if routing_result.selected_route in ["template-based", "template-combination", "hybrid"]:
  
  template_matching:
    # 在元模板库中查找匹配的模板
    for template in meta-templates:
      score = compute_similarity(project_features, template.triggers)
      if score >= 0.8:
        add_to_matched_templates(template, score)
        
    # 分解未匹配的特征
    for feature in project_features:
      if not any(t.matched_features.contains(feature) for t in matched_templates):
        add_to_unmatched_features(feature)
```

### Step 3: 开发阶段推理（元能力 2）

```yaml
# 引用: meta-capabilities/02-stage-reasoning.md

capability: stage-reasoning

# 根据项目特征，使用通用推理规则推理需要哪些开发阶段
rules_source: "knowledge/reasoning-rules.yaml"

# 推理规则（通用，不针对特定项目）
rules:
  - rule_id: "R001"
    name: "external-integration"
    trigger: "项目涉及调用外部系统/API"
    stages_required: ["API_ANALYSIS", "INTERFACE_DESIGN", "COMPATIBILITY_TEST", "INTEGRATION_TEST"]
    
  - rule_id: "R002"
    name: "data-processing"
    trigger: "项目涉及数据存储/处理"
    stages_required: ["DATA_MODEL_DESIGN", "DATABASE_DESIGN", "DATA_TEST", "DATA_SECURITY"]

# 推理过程
reasoning_process:
  1_match: "对每个规则，检查项目特征是否满足触发条件"
  2_score: "计算每个规则的匹配度"
  3_select: "选择匹配度高的规则"
  4_generate: "生成需要的阶段列表"

# 输出
output:
  matched_rules:
    - rule_id: "R006"
      name: "ai-llm-integration"
      match_score: 0.9
      
  triggered_stages:
    - stage_id: "COST_ANALYSIS"
      importance: "critical"
```

### Step 4: 知识组合（元能力 4）

```yaml
# 引用: meta-capabilities/04-knowledge-composition.md

capability: knowledge-composition

# 当项目包含多种特征时，智能组合已有知识
composition_strategies:
  - id: "S001"
    name: "feature-decomposition"
    description: "将复杂项目分解为多个简单特征，分别处理后组合"
    
  - id: "S002"
    name: "knowledge-retrieval"
    description: "从知识库中检索相似项目的成功案例"
    
  - id: "S003"
    name: "pattern-matching"
    description: "识别项目匹配的已知模式，使用模式的标准流程"
    
  - id: "S004"
    name: "innovative-generation"
    description: "对于未知部分，基于通用原则生成"

# 知识库引用
knowledge_sources:
  - path: "knowledge/project-patterns.yaml"
    description: "项目模式库"
    
  - path: "element-library/"
    description: "元素库"
    
  - path: "knowledge/best-practices/"
    description: "最佳实践"
```

### Step 5: 组合引擎

```yaml
# 引用: knowledge/combination-engine.yaml

combination_engine:
  name: "组合引擎"
  
  # 组合策略
  strategies:
    - id: "CE-001"
      name: "template-first"
      description: "以模板为基础，用推理结果补充"
      
    - id: "CE-002"
      name: "reasoning-first"
      description: "以推理结果为基础，用模板元素补充"
      
    - id: "CE-003"
      name: "layered-composition"
      description: "分层组合：骨架 → 模板 → 推理 → 元素 → 最佳实践"
  
  # 分层组合
  layers:
    layer_1:
      name: "flow-skeleton"
      source: "knowledge/flow-skeleton.yaml"
      
    layer_2:
      name: "template-content"
      source: "meta-templates/"
      condition: "has_matching_template"
      
    layer_3:
      name: "reasoning-result"
      source: "meta-capabilities/"
      
    layer_4:
      name: "element-supplements"
      source: "element-library/"
      
    layer_5:
      name: "best-practices"
      source: "knowledge/best-practices/"
      condition: "has_applicable_best_practice"
  
  # 冲突解决
  conflict_resolution:
    priority_order:
      1: "security-related"
      2: "best-practices"
      3: "user-preferences"  # v6.0 新增：用户偏好优先级提升
      4: "project-specific"
      5: "reasoning-result"
      6: "template-content"
```

### Step 6: 个性化适配（v6.0 新增）

```yaml
# 引用: knowledge/personalization.yaml

personalization_adaptation:
  name: "个性化适配"
  
  # 应用用户偏好
  apply_preferences:
    # 详细程度适配
    detail_level_adaptation:
      minimal:
        checks: "only_critical"
        explanations: "none"
        examples: "none"
        
      detailed:
        checks: "all"
        explanations: "detailed"
        examples: "comprehensive"
    
    # 指导风格适配
    guidance_style_adaptation:
      directive:
        language: "imperative"
        examples: ["执行 X 操作", "运行 Y 命令"]
        
      suggestive:
        language: "recommendatory"
        examples: ["建议执行 X 操作", "可以考虑运行 Y 命令"]
    
    # 自动化程度适配
    automation_level_adaptation:
      manual:
        checkpoints: "all"
        auto_proceed: false
        
      auto:
        checkpoints: "exceptions_only"
        auto_proceed: true
  
  # 输出
  output:
    personalized_generation:
      adapted_detail_level: "detailed"
      adapted_guidance_style: "suggestive"
      adapted_automation_level: "semi_auto"
      adapted_risk_tolerance: "balanced"
```

### Step 7: 流程结构生成（元能力 3）

```yaml
# 引用: meta-capabilities/03-flow-generation.md

capability: flow-generation

# 将组合结果转化为完整的流程定义
skeleton_source: "knowledge/flow-skeleton.yaml"

# 流程骨架（所有项目通用）
flow_skeleton:
  stage_0: "需求理解"      # universal
  stage_1: "任务分解"      # universal
  stage_2: "设计"          # universal
  stage_3: "实现"          # universal
  stage_4: "验证"          # universal
  stage_5: "上线"          # universal

# AI 的填充工作（应用个性化适配后）
fill_process:
  1_insert: "在骨架阶段之间插入项目特定阶段"
  2_customize: "为每个阶段填充具体内容（应用个性化）"
  3_configure: "配置闸门和产物"

# 输出产物
generated_artifacts:
  - path: ".harness/SKILL.md"
    description: "主流程 Skill（已个性化）"
    
  - path: ".harness/templates/"
    description: "开发模板（按需生成）"
    
  - path: ".harness/references/"
    description: "流程参考文档"
    
  - path: ".harness/scripts/"
    description: "流程检查脚本"
```

### Step 8: 预测性建议生成（v6.0 新增）

```yaml
# 引用: knowledge/prediction-engine.yaml

prediction_suggestion_generation:
  name: "预测性建议生成"
  
  # 基于历史和上下文生成预测
  prediction_analysis:
    # 序列预测
    sequence_prediction:
      current_state: "flow_generated"
      predicted_next:
        - action: "user_review"
          confidence: 0.9
          
        - action: "customization"
          confidence: 0.75
    
    # 上下文感知预测
    context_prediction:
      missing_elements:
        - element: "cost_analysis"
          confidence: 0.85
          reason: "AI 项目通常需要成本分析"
          
      risk_indicators:
        - risk: "missing_security_check"
          confidence: 0.6
          suggestion: "建议添加安全检查"
  
  # 生成建议
  suggestions_generated:
    critical:
      - id: "SUG-001"
        type: "risk_warning"
        message: "检测到安全风险：API 处理敏感数据但缺少加密检查"
        priority: "critical"
        auto_apply: true
        
    high:
      - id: "SUG-002"
        type: "element_suggestion"
        message: "AI 项目推荐添加成本估算步骤"
        priority: "high"
        
    medium:
      - id: "SUG-003"
        type: "best_practice"
        message: "建议在部署前进行性能基准测试"
        priority: "medium"
  
  # 输出
  output:
    suggestions: []
    predictions: []
    confidence_scores: []
```

### Step 9: 学习反馈（元能力 5 + 学习系统）

```yaml
# 引用: meta-capabilities/05-learning-feedback.md
# 引用: meta-capabilities/06-learning-system.md

capability: learning-feedback + learning-system

# 生成完成后，收集反馈并更新知识库
learning_process:
  # 收集生成结果
  1_collect_result:
    - "生成的流程定义"
    - "使用的路由策略"
    - "应用的个性化设置"
    - "采纳的预测建议"
    
  # 收集用户反馈
  2_collect_feedback:
    implicit_metrics:
      - "completion_rate"
      - "modification_count"
      - "suggestion_acceptance"
      
    explicit_fields:
      - "satisfaction_score"
      - "comments"
      - "suggestions"
      
  # 更新知识库
  3_update_knowledge_base:
    - "记录个性化效果"
    - "更新预测准确性"
    - "更新规则性能"
    
  # 应用规则优化（v6.0 新增）
  4_apply_rule_optimization:
    # 引用: knowledge/rule-optimizer.yaml
    - "分析规则性能"
    - "生成优化建议"
    - "应用自动优化"
```

---

## 智能优化系统

### 规则优化器

```yaml
# 引用: knowledge/rule-optimizer.yaml

rule_optimizer:
  name: "规则优化器"
  
  # 性能追踪
  performance_tracking:
    metrics:
      - trigger_count: "规则触发次数"
      - match_accuracy: "匹配准确率"
      - false_positive_rate: "误触发率"
      - success_rate: "触发后成功完成率"
      - user_satisfaction: "用户满意度"
  
  # 优化策略
  optimization_strategies:
    - id: "OPT_001"
      name: "threshold_adjustment"
      description: "调整规则阈值"
      trigger: "false_positive_rate > 0.2 OR false_negative_rate > 0.1"
      
    - id: "OPT_002"
      name: "condition_optimization"
      description: "优化规则条件"
      trigger: "false_positive_rate > 0.15 AND threshold_adjustment_ineffective"
      
    - id: "OPT_003"
      name: "weight_optimization"
      description: "优化规则权重"
      trigger: "multiple_rules_trigger_and_need_priority"
      
    - id: "OPT_004"
      name: "rule_merge_split"
      description: "规则合并/拆分"
      trigger: "rules_highly_correlated OR rule_too_broad"
  
  # 自动优化流程
  automatic_optimization:
    schedule: "daily"
    process:
      - "收集性能数据"
      - "评估规则健康状态"
      - "识别需要优化的规则"
      - "生成优化提案"
      - "验证提案"
      - "应用或标记审核"
```

### 个性化适配器

```yaml
# 引用: knowledge/personalization.yaml

personalization_adapter:
  name: "个性化适配器"
  
  # 用户画像
  user_profile:
    explicit_preferences: "用户显式设置"
    inferred_preferences: "从行为推断"
    team_overrides: "团队偏好（如果有）"
  
  # 偏好维度
  preference_dimensions:
    - detail_level: "详细程度"
    - guidance_style: "指导风格"
    - automation_level: "自动化程度"
    - risk_tolerance: "风险容忍度"
    - documentation_preference: "文档偏好"
    - coding_style: "代码风格"
  
  # 适配规则
  adaptation_rules:
    - detail_level_adaptation: "调整生成内容的详细度"
    - guidance_style_adaptation: "调整指令的措辞风格"
    - automation_level_adaptation: "调整确认点的设置"
    - risk_tolerance_adaptation: "调整检查的严格度"
```

### 预测性建议引擎

```yaml
# 引用: knowledge/prediction-engine.yaml

prediction_engine:
  name: "预测性建议引擎"
  
  # 预测模型
  prediction_models:
    - sequence_prediction: "基于历史序列预测下一步"
    - context_aware_prediction: "基于当前上下文的智能预测"
    - knowledge_enhanced_prediction: "结合知识库进行增强预测"
  
  # 建议类型
  suggestion_types:
    - action_suggestion: "建议执行的操作"
    - element_suggestion: "建议添加的元素"
    - timing_suggestion: "建议的时机"
    - risk_suggestion: "建议的风险处理"
    - optimization_suggestion: "建议的优化"
  
  # 预测准确性追踪
  accuracy_tracking:
    - prediction_accuracy: "预测正确率"
    - user_acceptance: "用户接受建议的比例"
    - false_positive_rate: "误报率"
    - missed_prediction_rate: "漏报率"
```

---

## 生成流程概览

```
┌─────────────────────────────────────────────────────────────────┐
│                    智能优化生成流程 (v6.0)                       │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌──────────────┐                                               │
│  │ 项目代码     │                                               │
│  │ 配置文件     │                                               │
│  │ 目录结构     │                                               │
│  └──────┬───────┘                                               │
│         │                                                       │
│         ▼                                                       │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │ Step -1: 用户画像加载 (v6.0 新增)                          │  │
│  │ - 加载用户偏好设置                                         │  │
│  │ - 加载团队偏好（如果有）                                    │  │
│  │ - 确定适配规则                                             │  │
│  └────────────────────────┬─────────────────────────────────┘  │
│                           │                                     │
│                           ▼                                     │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │ Step 0: 智能路由                                          │  │
│  │ - 分析项目复杂度                                          │  │
│  │ - 匹配元模板                                              │  │
│  │ - 选择生成策略                                            │  │
│  └────────────────────────┬─────────────────────────────────┘  │
│                           │                                     │
│           ┌───────────────┼───────────────┐                    │
│           │               │               │                    │
│           ▼               ▼               ▼                    │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐            │
│  │ skeleton-only│  │ template-based│ │ capability-driven│            │
│  │ (简单项目)   │  │ (已知模式)   │  │ (全新场景)   │            │
│  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘            │
│         │                │                │                    │
│         └────────────────┼────────────────┘                    │
│                          │                                     │
│                          ▼                                     │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │ Step 1-5: 特征提取 + 模板匹配 + 阶段推理 + 知识组合        │  │
│  │ (根据路由结果选择性执行)                                   │  │
│  └────────────────────────┬─────────────────────────────────┘  │
│                           │                                     │
│                           ▼                                     │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │ Step 6: 个性化适配 (v6.0 新增)                            │  │
│  │ - 应用详细程度偏好                                        │  │
│  │ - 应用指导风格偏好                                        │  │
│  │ - 应用自动化程度偏好                                      │  │
│  │ - 应用风险容忍度偏好                                      │  │
│  └────────────────────────┬─────────────────────────────────┘  │
│                           │                                     │
│                           ▼                                     │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │ Step 5: 组合引擎                                          │  │
│  │ - 分层组合: 骨架 → 模板 → 推理 → 元素 → 最佳实践           │  │
│  │ - 冲突解决 (含用户偏好优先级)                              │  │
│  │ - 验证                                                     │  │
│  └────────────────────────┬─────────────────────────────────┘  │
│                           │                                     │
│                           ▼                                     │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │ Step 7: 流程结构生成                                      │  │
│  │ - 生成 SKILL.md (已个性化)                                │  │
│  │ - 生成 Templates                                          │  │
│  │ - 生成 References                                         │  │
│  │ - 生成 Scripts                                            │  │
│  └────────────────────────┬─────────────────────────────────┘  │
│                           │                                     │
│                           ▼                                     │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │ Step 8: 预测性建议生成 (v6.0 新增)                        │  │
│  │ - 序列预测: 预测下一步                                    │  │
│  │ - 上下文预测: 检测遗漏和风险                              │  │
│  │ - 生成主动建议                                            │  │
│  └────────────────────────┬─────────────────────────────────┘  │
│                           │                                     │
│                           ▼                                     │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │ Step 9: 学习反馈 + 规则优化                               │  │
│  │ - 收集用户反馈                                             │  │
│  │ - 更新知识库                                               │  │
│  │ - 更新规则性能                                             │  │
│  │ - 应用规则优化                                             │  │
│  └──────────────────────────────────────────────────────────┘  │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## 分类维度（保留）

```
项目特征 → 分类决策

维度 1: Intents（开发意图）
  - new-capability      新功能
  - incremental-change  增量变更
  - bugfix              Bug 修复
  - refactor            重构
  - architecture-decision  架构决策

维度 2: Surfaces（影响面，可组合）
  - api-contract        API 契约
  - data-model          数据模型
  - frontend-ui         前端 UI
  - backend-logic       后端逻辑
  - security-sensitive  安全敏感
  - performance-sensitive  性能敏感
  - deployment-config   部署配置

维度 3: Risk（风险等级）
  - low   低风险
  - medium  中风险
  - high   高风险
  - incident  线上事故
```

---

## 输出产物结构

### 完整模式（Full Depth）

```
.harness/
├── SKILL.md                          # 主流程 Skill（已个性化）
├── README.md                         # 使用说明
├── constitution-profiles.json        # 宪法模型（机器可读）
├── constitution-profiles.md          # 宪法模型（人类可读）
├── references/                       # 流程参考文档
│   ├── gate-protocol.md              # 闸门协议
│   ├── task-execution-protocol.md    # 任务执行协议
│   ├── clarification-loop.md         # 需求澄清协议
│   ├── change-control.md             # 变更控制
│   └── ...
├── templates/                        # 开发模板（按需生成）
│   ├── TEMPLATE-feature.md           # 功能开发模板
│   ├── TEMPLATE-bugfix.md            # Bug 修复模板
│   └── ...
└── scripts/                          # 流程检查脚本
    ├── workflow-preflight.py         # 流程预检
    ├── check-gates.py                # 闸门检查
    ├── check-artifacts.py            # 产物检查
    └── ...
```

### 中等模式（Medium Depth）

```
.harness/
├── SKILL.md                    # 主流程 Skill
├── constitution-profiles.json  # 宪法模型
├── references/                 # 核心参考文档
│   ├── gate-protocol.md
│   ├── clarification-loop.md
│   └── ...
└── scripts/                    # 核心脚本
    ├── check-gates.py
    └── ...
```

### 轻量模式（Light Depth）

```
.harness/
├── SKILL.md           # 轻量 Workflow
└── scripts/
    └── gate-check.py  # 基础闸门检查
```

---

## 迭代优化命令

### /harness-status

查看当前 Harness 状态：

```bash
/harness-status
```

输出：

```
📊 Harness 状态 (v6.0.0 智能优化)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

项目: my-ai-web-app
生成模式: 混合模式 (hybrid)
生成时间: 2026-06-22 15:30:00

👤 用户画像
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
详细程度: detailed
指导风格: suggestive
自动化程度: semi_auto
风险容忍度: balanced

🔀 路由决策
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- 选择路由: hybrid
- 置信度: medium
- 匹配模板: TEMPLATE-ai-feature.md
- 未匹配特征: ["react-app"]

📋 识别的特征
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- ai-llm-project (high confidence) [有模板]
- react-app (high confidence) [无模板，推理]

🧠 使用的能力
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
1. 用户画像加载 ✓
2. 项目特征提取 ✓
3. 开发阶段推理 ✓ (用于 react-app)
4. 知识组合 ✓
5. 个性化适配 ✓
6. 组合引擎 ✓
7. 流程结构生成 ✓ (已个性化)
8. 预测性建议 ✓
9. 学习反馈 (待用户反馈)

💡 预测性建议
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[高] 建议添加成本估算步骤 (AI 项目推荐)
[中] 建议在部署前进行性能测试
[低] 可以将步骤 3 和 4 合并

📚 知识积累
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- 学习记录: 等待反馈
- 知识库版本: 1.3.0
- 最佳实践: 12 个已加载
- 规则优化: 3 条已应用

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### /harness-predict

查看预测性建议（v6.0 新增）：

```bash
/harness-predict
```

输出：

```
💡 预测性建议
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🎯 序列预测
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
当前状态: 流程已生成
预测下一步: 用户审查 (置信度: 90%)
备选: 自定义修改 (置信度: 75%)

⚠️ 风险预测
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[高] 检测到安全风险
  - 问题: API 处理敏感数据但缺少加密检查
  - 建议: 立即添加加密检查步骤
  - 自动应用: 是

[中] 潜在性能风险
  - 问题: AI API 调用可能超时
  - 建议: 添加超时处理和重试逻辑
  - 自动应用: 否

📦 缺失元素检测
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[高] AI 项目通常需要成本分析
  - 当前流程中缺少此步骤
  - 建议: 添加成本估算模板
  - 自动应用: 是

[中] 建议添加部署前检查
  - 当前流程中缺少此步骤
  - 建议: 添加 pre-deployment checklist
  - 自动应用: 否

🚀 效率建议
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[低] 可以将代码审查和测试合并
  - 预计节省: 约 30 分钟
  - 建议: 在测试环境进行代码审查

[低] 可以跳过可选文档生成
  - 基于您的偏好设置
  - 建议: 跳过 API 文档自动生成

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### /harness-optimize

查看规则优化状态（v6.0 新增）：

```bash
/harness-optimize
```

输出：

```
⚙️ 规则优化状态
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

规则健康度概览
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
总规则数: 10
健康规则: 8 (80%)
需优化规则: 2 (20%)

规则性能统计 (最近 7 天)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
规则 ID     触发次数  成功率   满意度  状态
────────────────────────────────────────────
R001        45        92%     4.3/5   ✓ 健康
R002        32        88%     4.1/5   ✓ 健康
R003        28        75%     3.8/5   ⚠ 需优化
R004        56        85%     4.2/5   ✓ 健康
R005        12        95%     4.5/5   ✓ 健康
R006        67        88%     4.0/5   ✓ 健康
R007        44        62%     3.5/5   ⚠ 需优化
R008        21        88%     4.1/5   ✓ 健康
R009        35        90%     4.4/5   ✓ 健康
R010        18        85%     4.0/5   ✓ 健康

需优化规则详情
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

R003 - data-processing
问题: 误触发率高 (25%)
原因: 阈值过于宽松
建议: 将置信度阈值从 0.6 提高到 0.7
状态: 待应用

R007 - microservice-architecture
问题: 漏匹配率高 (18%)
原因: 条件不够全面
建议: 添加 "使用 Docker Compose" 作为触发条件
状态: 待应用

自动优化
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
上次运行: 2026-06-22 02:00:00
下次运行: 2026-06-23 02:00:00

自动应用的优化: 1 个
待人工审核: 2 个

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### /harness-knowledge

查看知识库状态：

```bash
/harness-knowledge
```

输出：

```
📖 知识库状态
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

知识层级:
  Level 0 - 原始反馈: 156 条
  Level 1 - 学习记录: 89 条
  Level 2 - 验证模式: 15 个
  Level 3 - 模板元素: 24 个
  Level 4 - 最佳实践: 12 个

知识更新:
  上次更新: 2026-06-22 10:00:00
  待处理更新: 3 个
  自动更新: 已启用 (每周)

提升统计:
  本月提升: 5 个
  - 反馈 → 学习记录: 12 个
  - 学习记录 → 模式: 2 个
  - 模式 → 模板元素: 1 个
  - 模板元素 → 最佳实践: 0 个

知识质量:
  平均质量评分: 85/100
  高质量知识: 48 个 (55%)
  待改进知识: 10 个 (11%)
  过时知识: 2 个 (2%)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## 版本历史

- v6.0.0 (2026-06-22): 实现智能优化
  - 新增规则优化器 (knowledge/rule-optimizer.yaml)
  - 新增个性化适配器 (knowledge/personalization.yaml)
  - 新增预测性建议引擎 (knowledge/prediction-engine.yaml)
  - 新增 /harness-predict 命令
  - 新增 /harness-optimize 命令
  - 组合引擎增加用户偏好优先级
  - 新增 Step -1 用户画像加载
  - 新增 Step 6 个性化适配
  - 新增 Step 8 预测性建议生成
  - 更新 Step 9 学习反馈整合规则优化
  
- v5.2.0 (2026-06-22): 实现知识积累系统
  - 新增学习系统元能力
  - 新增反馈收集定义
  - 新增知识更新规则
  - 新增知识提升规则
  - 新增知识结构目录
  - 新增 /harness-knowledge 命令
  
- v5.1.0 (2026-06-22): 实现混合模式
  - 新增智能路由机制
  - 新增组合引擎
  - 新增元素库
  - 支持 5 种路由模式
  - 新增 /harness-route 命令
  
- v5.0.0 (2026-06-22): 元能力驱动
  - 新增 5 个元能力定义
  - 新增推理规则库
  - 新增流程骨架定义
  - 新增学习反馈机制
  
- v4.6.0 (2026-06-22): 知识积累机制
  - 新增 learnings.yaml
  - 补充 AI/微服务/Monorepo 元模板
  
- v4.5.0 (2026-06-18): 基于 GitHub Top 100 项目分析
  - 扩展项目模式覆盖（AI/LLM 项目 30%）
  
- v4.0.0 (2026-06-18): 从"配置生成器"转型为"开发流程体系生成器"
  - 新增 Workflow 生成能力
  - 新增 Template 按需生成能力
  - 新增闸门机制
  - 新增宪法模型
  
- v3.5.1: 历史记录保存功能
- v3.5.0: 迭代优化命令
- v3.2.0: 融合 SDD 最佳实践
- v3.1.0: CI/CD 集成
- v3.0.0: 重构为定制化系统输出
