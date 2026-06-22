---
capability: knowledge-composition
name: 知识组合
version: 1.0.0
description: |
  教 AI 如何组合已有知识，处理混合场景。
  
  核心思想：复杂项目可以分解为多个简单特征，每个特征有对应的知识，AI 的工作是智能组合。
---

# 元能力 4: 知识组合

## 核心目标

当项目包含多种特征时，智能组合已有知识，生成适合该项目的流程。

## 组合策略

### 策略 1: 特征分解 (Feature Decomposition)

```yaml
strategy_id: S001
name: feature-decomposition
description: |
  将复杂项目分解为多个简单特征，每个特征独立处理。
  
  适用场景：项目包含多种不同类型的特征
  
example:
  project: "AI Agent 项目"
  decomposition:
    - feature: "LLM 集成"
      knowledge: "TEMPLATE-llm-integration"
      
    - feature: "API 设计"
      knowledge: "TEMPLATE-api-design"
      
    - feature: "数据存储"
      knowledge: "TEMPLATE-data-storage"
      
  result: 组合三个特征的知识
  
process:
  # Step 1: 识别项目包含哪些特征
  identify_features:
    input: project_features
    output: feature_list
    
  # Step 2: 为每个特征查找对应的知识
  find_knowledge:
    for feature in feature_list:
      knowledge = search_knowledge_base(feature)
      
  # Step 3: 组合各特征的知识
  compose_knowledge:
    base = universal_flow_skeleton
    for knowledge in found_knowledges:
      merge(base, knowledge)
      
  # Step 4: 解决冲突
  resolve_conflicts:
    for conflict in conflicts:
      apply_resolution_strategy(conflict)
```

### 策略 2: 知识检索 (Knowledge Retrieval)

```yaml
strategy_id: S002
name: knowledge-retrieval
description: |
  从知识库中检索相关经验，参考成功案例。
  
  适用场景：知识库中有类似项目的成功案例
  
example:
  project: "电商推荐系统"
  retrieval:
    similar_projects:
      - "用户画像系统" (80% 相似)
      - "商品搜索系统" (70% 相似)
      
    best_practices:
      - "特征工程流程"
      - "模型评估标准"
      
  result: 参考相似项目的流程
  
process:
  # Step 1: 计算项目与知识库中项目的相似度
  calculate_similarity:
    for known_project in knowledge_base:
      similarity = compute_similarity(project_features, known_project.features)
      
  # Step 2: 检索高相似度项目的流程
  retrieve_flows:
    threshold: 0.7
    for project in high_similarity_projects:
      flow = get_project_flow(project)
      
  # Step 3: 提取可复用的模式
  extract_patterns:
    patterns = find_reusable_patterns(retrieved_flows)
    
  # Step 4: 应用到当前项目
  apply_patterns:
    for pattern in patterns:
      adapt(pattern, current_project_features)
```

### 策略 3: 模式匹配 (Pattern Matching)

```yaml
strategy_id: S003
name: pattern-matching
description: |
  识别项目匹配的已知模式，优先使用已知模式的流程。
  
  适用场景：项目高度匹配某个已知模式
  
example:
  project: "React 前端应用"
  matching:
    patterns:
      - pattern: "frontend-only"
        confidence: 0.95
        
      - pattern: "react-app"
        confidence: 0.90
        
    result: 使用 frontend-only 模式的标准流程
  
process:
  # Step 1: 加载项目模式库
  load_patterns:
    source: "knowledge/project-patterns.yaml"
    patterns: ["frontend-only", "react-app", "ai-llm-project", ...]
    
  # Step 2: 计算匹配度
  compute_match:
    for pattern in patterns:
      match_score = 0
      for feature in pattern.required_features:
        if feature in project_features:
          match_score += 1
      pattern.match_score = match_score / len(pattern.required_features)
      
  # Step 3: 选择最佳匹配
  select_best:
    best_pattern = max(patterns, key=lambda p: p.match_score)
    if best_pattern.match_score >= 0.8:
      use_pattern_flow(best_pattern)
      
  # Step 4: 补充不匹配的部分
  supplement_gaps:
    unmatched_features = project_features - best_pattern.features
    for feature in unmatched_features:
      add_feature_handling(feature)
```

### 策略 4: 创新生成 (Innovative Generation)

```yaml
strategy_id: S004
name: innovative-generation
description: |
  对于完全未知的部分，基于通用原则和最佳实践生成。
  
  适用场景：知识库中没有匹配的知识
  
example:
  project: "全新类型的项目"
  innovation:
    unknown_aspects:
      - "区块链集成"
      - "WebAssembly 编译"
      
    generation:
      - based_on: "外部集成通用原则"
        generate: "区块链集成流程"
        
      - based_on: "性能优化通用原则"
        generate: "WASM 编译流程"
  
process:
  # Step 1: 识别未知的部分
  identify_unknown:
    known = knowledge_base.coverage(project_features)
    unknown = project_features - known
    
  # Step 2: 查找可类比的知识
  find_analogies:
    for unknown in unknowns:
      analogies = find_similar_known_features(unknown)
      
  # Step 3: 基于类比和通用原则生成
  generate_from_principles:
    principles:
      - "外部集成通用原则"
      - "安全设计通用原则"
      - "性能优化通用原则"
      
    for unknown in unknowns:
      flow = generate_flow(unknown, principles, analogies)
      
  # Step 4: 标记为需要验证
  mark_for_validation:
    for flow in generated_flows:
      flow.source = "innovative-generation"
      flow.needs_validation = true
```

## 组合引擎

### 决策流程

```yaml
composition_decision:
  # 优先级：模式匹配 > 特征分解 > 知识检索 > 创新生成
  
priority_1_pattern_matching:
    condition: "存在匹配度 >= 0.8 的已知模式"
    action: "使用模式的标准流程"
    supplement: "补充不匹配的部分"
    
  priority_2_feature_decomposition:
    condition: "项目包含多个可识别的特征"
    action: "分解特征，分别处理"
    combine: "合并各特征的流程"
    
  priority_3_knowledge_retrieval:
    condition: "知识库中有相似项目的成功案例"
    action: "检索相似案例"
    adapt: "适配到当前项目"
    
  priority_4_innovative_generation:
    condition: "以上策略都不适用"
    action: "基于通用原则生成"
    validate: "标记为需要验证"
```

### 冲突解决

```yaml
conflict_resolution:
  # 当不同策略产生冲突时的解决规则
  
rules:
    # 规则 1: 安全优先
    rule_1:
      condition: "一个策略包含安全检查，另一个不包含"
      resolution: "保留安全检查"
      
    # 规则 2: 更具体的优先
    rule_2:
      condition: "两个策略都适用"
      resolution: "选择更具体的策略"
      example: "项目特定流程 > 通用流程"
      
    # 规则 3: 用户确认
    rule_3:
      condition: "冲突无法自动解决"
      resolution: "提交给用户确认"
      
    # 规则 4: 合并
    rule_4:
      condition: "两个策略的流程可以互补"
      resolution: "合并两个流程"
      
  # 冲突检测
  detection:
    - "相同的阶段有不同的步骤"
    - "相同的检查项有不同的标准"
    - "相同的闸门有不同的条件"
```

### 组合算法

```yaml
composition_algorithm:
  # 输入
  input:
    - project_features: "项目特征"
    - knowledge_base: "知识库"
    - reasoning_result: "推理结果"
    
  # 处理
  process:
    # Phase 1: 分析
    1_analyze:
      - 识别项目的所有特征
      - 计算每个特征的知识覆盖度
      - 识别未知特征
      
    # Phase 2: 匹配
    2_match:
      - 在知识库中搜索匹配的模式
      - 计算匹配度
      - 选择最佳匹配
      
    # Phase 3: 组合
    3_compose:
      - 如果有高度匹配的模式，使用模式流程
      - 否则，分解特征分别处理
      - 合并各部分的流程
      
    # Phase 4: 验证
    4_validate:
      - 检测冲突
      - 解决冲突
      - 验证完整性
      
  # 输出
  output:
    - combined_flow: "组合后的流程"
    - knowledge_sources: "知识来源列表"
    - confidence: "组合结果的置信度"
    - needs_validation: "需要用户验证的部分"
```

## 知识库结构

### 项目模式库

```yaml
project_patterns:
  - id: "ai-llm-project"
    name: "AI/LLM 项目"
    required_features:
      - "has_ai_integration"
      - "has_api_key_config"
    optional_features:
      - "has_prompt_engineering"
      - "has_embedding"
    standard_flow:
      - "成本分析"
      - "Prompt 设计"
      - "模型选择"
      - "AI 测试"
      confidence_threshold: 0.8
      
  - id: "microservice-architecture"
    name: "微服务架构"
    required_features:
      - "has_multiple_services"
      - "has_service_discovery"
    optional_features:
      - "has_docker"
      - "has_k8s"
    standard_flow:
      - "服务边界设计"
      - "服务通信设计"
      - "部署顺序"
      confidence_threshold: 0.8
```

### 成功案例库

```yaml
success_cases:
  - id: "case-001"
    project: "电商推荐系统"
    features:
      - "machine-learning"
      - "real-time-processing"
      - "data-intensive"
    flow_used: "ML-training-workflow"
    outcome: "success"
    lessons_learned:
      - "特征工程是关键"
      - "模型评估要全面"
    
  - id: "case-002"
    project: "AI Agent 助手"
    features:
      - "ai-llm-project"
      - "agent-framework"
      - "conversation-management"
    flow_used: "ai-agent-workflow"
    outcome: "success"
    lessons_learned:
      - "Prompt 管理很重要"
      - "成本控制是关键"
```

### 最佳实践库

```yaml
best_practices:
  - id: "bp-001"
    name: "LLM 成本优化"
    applicable_when:
      - "has_ai_integration"
      - "has_cost_concerns"
    content:
      - "使用 GPT-3.5 处理简单任务"
      - "实现缓存机制"
      - "批量处理请求"
      
  - id: "bp-002"
    name: "安全设计检查"
    applicable_when:
      - "has_security_surface"
    content:
      - "API Key 不硬编码"
      - "实现访问控制"
      - "添加输出过滤"
```

## 与其他元能力的关系

```yaml
input_from:
  - capability: "project-analysis"
    provides: ["identified_features", "uncertainties"]
    
  - capability: "stage-reasoning"
    provides: ["triggered_stages", "matched_rules"]
    
output_to:
  - capability: "flow-generation"
    provides: ["combined_knowledge", "best_practices"]
    
  - capability: "learning-feedback"
    provides: ["knowledge_usage_stats", "new_knowledge_candidates"]
```

## 学习反馈

```yaml
learning_from_composition:
  # 记录每次组合的结果
  record:
    - project_features
    - selected_strategy
    - knowledge_sources
    - user_feedback
    - final_outcome
    
  # 从成功案例中学习
  extract_patterns:
    if outcome == "success" and user_feedback == "satisfied":
      consider_promoting_to_best_practice()
      
  # 从失败案例中学习
  learn_from_failures:
    if outcome == "failure" or user_feedback == "unsatisfied":
      analyze_reasons()
      update_knowledge_base()
```

## 示例

### 输入

```yaml
project_features:
  - name: "ai-llm-project"
    confidence: "high"
    
  - name: "microservice-architecture"
    confidence: "medium"
    
  - name: "security-sensitive"
    confidence: "high"
```

### 处理过程

```yaml
# Phase 1: 分析
analysis:
  features:
    - "ai-llm-project" (high confidence, known)
    - "microservice-architecture" (medium confidence, known)
    - "security-sensitive" (high confidence, known)
  
  unknown_features: []
  
# Phase 2: 匹配
matching:
  patterns_checked:
    - pattern: "ai-llm-project"
      match_score: 0.9
      
    - pattern: "microservice-architecture"
      match_score: 0.75
      
    - pattern: "security-critical-system"
      match_score: 0.85
      
  best_matches:
    - "security-critical-system" (0.85)
    - "ai-llm-project" (0.9)
    - "microservice-architecture" (0.75)
  
# Phase 3: 组合
composition:
  strategy: "feature-decomposition"
  
  components:
    - feature: "ai-llm-project"
      knowledge: "TEMPLATE-llm-integration"
      stages: ["成本分析", "Prompt 设计", "AI 测试"]
      
    - feature: "microservice-architecture"
      knowledge: "TEMPLATE-microservice"
      stages: ["服务边界设计", "服务通信设计"]
      
    - feature: "security-sensitive"
      knowledge: "TEMPLATE-security"
      stages: ["安全设计", "安全审查"]
  
  merged_stages:
    - "需求理解"
    - "成本分析" (from ai-llm)
    - "服务边界设计" (from microservice)
    - "安全设计" (from security)
    - "Prompt 设计" (from ai-llm)
    - "实现"
    - "安全审查" (from security)
    - "服务通信测试" (from microservice)
    - "AI 测试" (from ai-llm)
    - "验证"
    - "上线"
  
# Phase 4: 验证
validation:
  conflicts_found: 0
  gaps_identified: 0
  confidence: "high"
```

### 输出

```yaml
combined_flow:
  strategy_used: "feature-decomposition"
  
  stages:
    - id: "REQUIREMENT"
      name: "需求理解"
      source: "universal"
      
    - id: "COST_ANALYSIS"
      name: "成本分析"
      source: "ai-llm-project"
      
    - id: "SERVICE_BOUNDARY"
      name: "服务边界设计"
      source: "microservice-architecture"
      
    - id: "SECURITY_DESIGN"
      name: "安全设计"
      source: "security-sensitive"
      
    - id: "PROMPT_DESIGN"
      name: "Prompt 设计"
      source: "ai-llm-project"
      
    - id: "IMPLEMENTATION"
      name: "实现"
      source: "universal"
      
    - id: "SECURITY_REVIEW"
      name: "安全审查"
      source: "security-sensitive"
      
    - id: "SERVICE_TEST"
      name: "服务通信测试"
      source: "microservice-architecture"
      
    - id: "AI_TESTING"
      name: "AI 测试"
      source: "ai-llm-project"
      
    - id: "VERIFICATION"
      name: "验证"
      source: "universal"
      
    - id: "DEPLOYMENT"
      name: "上线"
      source: "universal"
      
  knowledge_sources:
    - "TEMPLATE-llm-integration"
    - "TEMPLATE-microservice"
    - "TEMPLATE-security"
    
  confidence: "high"
  needs_validation: false
```
