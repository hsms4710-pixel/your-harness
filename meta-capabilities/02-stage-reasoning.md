---
capability: stage-reasoning
name: 开发阶段推理
version: 1.0.0
description: |
  教 AI 如何根据项目特征，推理出需要哪些开发阶段。
  
  核心思想：不是针对特定项目的规则，而是通用的推理规则。
  任何项目都可以通过这些规则推导出适合它的开发阶段。
---

# 元能力 2: 开发阶段推理

## 核心目标

根据项目特征提取结果，推理出该项目需要哪些开发阶段、每个阶段需要关注什么。

## 推理规则

### 规则 1: 外部集成规则 (External Integration)

```yaml
rule_id: R001
name: external-integration
trigger:
  condition: "项目涉及调用外部系统/API"
  signals:
    - "有 HTTP 客户端调用外部服务"
    - "有 API Key、Token 配置"
    - "有第三方 SDK 依赖"
    - "有 webhook 或 callback 处理"
    
reasoning:
  logic: |
    外部集成意味着：
    1. 需要理解外部 API 的行为和限制
    2. 需要处理 API 变更的兼容性
    3. 需要测试与外部系统的交互
    4. 需要考虑外部服务的可用性
    
  therefore: |
    必须有专门的阶段来处理这些关注点
    
stages_required:
  - id: API_ANALYSIS
    name: "API 分析"
    focus: |
      - 理解外部 API 的功能和限制
      - 识别 API 的版本和兼容性策略
      - 评估 API 的稳定性
      
  - id: INTERFACE_DESIGN
    name: "接口设计"
    focus: |
      - 设计内部与外部 API 的交互层
      - 定义错误处理策略
      - 设计降级和重试机制
      
  - id: COMPATIBILITY_TEST
    name: "兼容性测试"
    focus: |
      - 测试不同 API 版本的兼容性
      - 测试错误场景处理
      - 验证限流和超时处理
      
  - id: INTEGRATION_TEST
    name: "集成测试"
    focus: |
      - 端到端测试外部集成
      - 模拟外部服务故障
      - 验证数据一致性
```

### 规则 2: 数据处理规则 (Data Processing)

```yaml
rule_id: R002
name: data-processing
trigger:
  condition: "项目涉及数据存储/处理"
  signals:
    - "有数据库连接代码"
    - "有 ORM 或数据库迁移"
    - "有数据模型定义"
    - "有数据导入/导出功能"
    - "有缓存实现"
    
reasoning:
  logic: |
    数据处理意味着：
    1. 需要设计合适的数据模型
    2. 需要考虑数据迁移和版本兼容
    3. 需要测试数据一致性和完整性
    4. 需要考虑数据安全和隐私
    
  therefore: |
    必须有专门的阶段来处理数据相关关注点
    
stages_required:
  - id: DATA_MODEL_DESIGN
    name: "数据模型设计"
    focus: |
      - 定义实体和关系
      - 设计索引策略
      - 考虑查询模式
      
  - id: DATABASE_DESIGN
    name: "数据库设计"
    focus: |
      - 表结构设计
      - 迁移脚本
      - 初始化数据
      
  - id: DATA_TEST
    name: "数据测试"
    focus: |
      - 测试 CRUD 操作
      - 测试数据一致性
      - 测试迁移脚本
      
  - id: DATA_SECURITY
    name: "数据安全"
    focus: |
      - 敏感数据加密
      - 访问控制
      - 审计日志
```

### 规则 3: 用户交互规则 (User Interaction)

```yaml
rule_id: R003
name: user-interaction
trigger:
  condition: "项目涉及用户界面"
  signals:
    - "有前端代码 (HTML/CSS/JS/React/Vue)"
    - "有 UI 组件库依赖"
    - "有设计稿或原型"
    - "有用户输入处理"
    - "有状态管理代码"
    
reasoning:
  logic: |
    用户交互意味着：
    1. 需要将需求转化为 UI 设计
    2. 需要开发可复用的组件
    3. 需要测试用户体验
    4. 需要考虑响应式和可访问性
    
  therefore: |
    必须有专门的阶段来处理 UI/UX 关注点
    
stages_required:
  - id: REQUIREMENT_ANALYSIS
    name: "需求分析"
    focus: |
      - 理解用户需求
      - 识别功能优先级
      - 定义验收标准
      
  - id: UI_DESIGN
    name: "UI 设计"
    focus: |
      - 设计界面布局
      - 定义组件结构
      - 考虑响应式设计
      
  - id: COMPONENT_DEVELOPMENT
    name: "组件开发"
    focus: |
      - 开发可复用组件
      - 实现状态管理
      - 处理用户输入
      
  - id: INTERACTION_TEST
    name: "交互测试"
    focus: |
      - 测试用户流程
      - 测试表单验证
      - 测试错误提示
```

### 规则 4: 安全敏感规则 (Security Sensitive)

```yaml
rule_id: R004
name: security-sensitive
trigger:
  condition: "项目涉及认证/授权/敏感数据"
  signals:
    - "有密码处理代码"
    - "有 JWT 或 Token 生成/验证"
    - "有 API Key 存储"
    - "有加密/解密代码"
    - "处理支付信息"
    - "处理个人隐私数据"
    
reasoning:
  logic: |
    安全敏感意味着：
    1. 不能依赖常规测试，需要专门的安全审查
    2. 需要考虑多种攻击向量
    3. 需要遵循安全最佳实践
    4. 可能需要合规性检查
    
  therefore: |
    必须有专门的安全设计和审查阶段
    
stages_required:
  - id: SECURITY_DESIGN
    name: "安全设计"
    focus: |
      - 识别安全风险
      - 设计认证授权方案
      - 定义加密策略
      
  - id: SECURITY_IMPLEMENTATION
    name: "安全实现"
    focus: |
      - 使用安全的密码存储
      - 实现访问控制
      - 添加安全头
      
  - id: SECURITY_REVIEW
    name: "安全审查"
    focus: |
      - 代码安全审计
      - 依赖漏洞扫描
      - 安全配置检查
      
  - id: PENETRATION_TEST
    name: "渗透测试"
    focus: |
      - 测试常见攻击向量
      - 测试授权绕过
      - 测试数据泄露
```

### 规则 5: 性能要求规则 (Performance Requirements)

```yaml
rule_id: R005
name: performance-requirements
trigger:
  condition: "项目涉及高并发/实时性要求"
  signals:
    - "有 async/await 或并发代码"
    - "有性能测试代码"
    - "有缓存实现"
    - "有队列或消息系统"
    - "有流式处理"
    - "有速率限制"
    
reasoning:
  logic: |
    性能要求意味着：
    1. 需要在设计阶段考虑性能
    2. 需要进行性能分析和优化
    3. 需要进行压力测试
    4. 需要监控性能指标
    
  therefore: |
    必须有专门的性能分析和测试阶段
    
stages_required:
  - id: PERFORMANCE_ANALYSIS
    name: "性能分析"
    focus: |
      - 识别性能瓶颈点
      - 分析算法复杂度
      - 评估资源使用
      
  - id: ARCHITECTURE_DESIGN
    name: "架构设计（性能视角）"
    focus: |
      - 设计缓存策略
      - 设计分页方案
      - 设计并发模型
      
  - id: PERFORMANCE_TEST
    name: "性能测试"
    focus: |
      - 基准测试
      - 压力测试
      - 负载测试
      
  - id: PERFORMANCE_OPTIMIZATION
    name: "性能优化"
    focus: |
      - 优化热点代码
      - 优化数据库查询
      - 优化网络通信
```

### 规则 6: AI/LLM 集成规则

```yaml
rule_id: R006
name: ai-llm-integration
trigger:
  condition: "项目涉及 LLM 调用/Prompt 工程"
  signals:
    - "有 OpenAI/Anthropic/LangChain 导入"
    - "有 prompt 模板文件"
    - "有 embedding 或 vector store"
    - "有 API Key 配置"
    
reasoning:
  logic: |
    AI/LLM 集成意味着：
    1. 需要管理 API 成本
    2. 需要处理 LLM 输出的不确定性
    3. 需要考虑 prompt 版本管理
    4. 需要测试不同模型的表现
    
  therefore: |
    必须有专门的 AI 管理阶段
    
stages_required:
  - id: COST_ANALYSIS
    name: "成本分析"
    focus: |
      - 估算 API 调用成本
      - 设计成本控制策略
      - 评估不同模型的成本
      
  - id: PROMPT_DESIGN
    name: "Prompt 设计"
    focus: |
      - 设计 prompt 模板
      - 测试 prompt 效果
      - 版本管理
      
  - id: MODEL_SELECTION
    name: "模型选择"
    focus: |
      - 评估不同模型的能力
      - 测试模型在特定任务上的表现
      - 权衡成本和效果
      
  - id: AI_TESTING
    name: "AI 测试"
    focus: |
      - 测试输出质量
      - 测试边界情况
      - 测试错误处理
      
  - id: SAFETY_GUARDS
    name: "安全防护"
    focus: |
      - Prompt injection 防护
      - 内容过滤
      - 输出验证
```

### 规则 7: 微服务架构规则

```yaml
rule_id: R007
name: microservice-architecture
trigger:
  condition: "项目采用微服务架构"
  signals:
    - "有多个服务目录"
    - "有服务发现配置"
    - "有 docker-compose 或 k8s 配置"
    - "有服务间调用代码"
    
reasoning:
  logic: |
    微服务架构意味着：
    1. 需要管理服务间通信
    2. 需要考虑分布式事务
    3. 需要处理服务部署顺序
    4. 需要设计服务边界
    
  therefore: |
    必须有专门的服务管理阶段
    
stages_required:
  - id: SERVICE_BOUNDARY_DESIGN
    name: "服务边界设计"
    focus: |
      - 定义服务职责
      - 设计 API 契约
      - 定义数据边界
      
  - id: SERVICE_COMMUNICATION_DESIGN
    name: "服务通信设计"
    focus: |
      - 选择通信协议
      - 设计消息格式
      - 处理服务发现
      
  - id: DEPLOYMENT_SEQUENCING
    name: "部署顺序"
    focus: |
      - 确定服务依赖
      - 设计部署顺序
      - 处理版本兼容
      
  - id: SERVICE_TESTING
    name: "服务测试"
    focus: |
      - 服务独立测试
      - 集成测试
      - 契约测试
```

### 规则 8: 数据密集型规则

```yaml
rule_id: R008
name: data-intensive
trigger:
  condition: "项目涉及大数据/批处理"
  signals:
    - "有 Spark/Dask 依赖"
    - "有数据管道代码"
    - "有大数据文件"
    - "有批处理任务"
    
reasoning:
  logic: |
    数据密集型意味着：
    1. 需要考虑资源使用
    2. 需要优化数据处理效率
    3. 需要考虑数据存储成本
    4. 需要测试大数据量场景
    
  therefore: |
    必须有专门的数据处理阶段
    
stages_required:
  - id: RESOURCE_PLANNING
    name: "资源规划"
    focus: |
      - 评估计算资源需求
      - 评估存储需求
      - 成本估算
      
  - id: DATA_PIPELINE_DESIGN
    name: "数据管道设计"
    focus: |
      - 设计数据流程
      - 选择处理框架
      - 设计存储方案
      
  - id: EFFICIENCY_OPTIMIZATION
    name: "效率优化"
    focus: |
      - 优化数据读取
      - 优化数据处理
      - 优化内存使用
      
  - id: SCALE_TESTING
    name: "规模测试"
    focus: |
      - 测试大数据量
      - 测试处理时间
      - 测试资源使用
```

## 推理流程

### Step 1: 匹配触发条件

```yaml
matching_process:
  # 对每个规则，检查是否满足触发条件
  for each rule in rules:
    signals_found = []
    for signal in rule.trigger.signals:
      if detect_signal(project_features, signal):
        signals_found.append(signal)
    
    # 计算匹配度
    if len(signals_found) >= rule.trigger.min_signals:
      rule.matched = true
      rule.match_score = len(signals_found) / len(rule.trigger.signals)
```

### Step 2: 计算阶段重要性

```yaml
importance_calculation:
  # 对每个阶段，计算其重要性分数
  for each stage:
    score = 0
    for each rule that requires this stage:
      score += rule.match_score * rule.priority_weight
    
    # 分类
    if score >= 0.8:
      stage.importance = "critical"
    elif score >= 0.5:
      stage.importance = "high"
    elif score >= 0.3:
      stage.importance = "medium"
    else:
      stage.importance = "low"
```

### Step 3: 生成阶段列表

```yaml
stage_list_generation:
  # 按重要性排序
  stages = sort_by_importance(all_stages)
  
  # 选择要包含的阶段
  selected_stages = []
  for stage in stages:
    if stage.importance in ["critical", "high"]:
      selected_stages.append(stage)
      stage.inclusion_reason = "自动选择（高重要性）"
    elif stage.importance == "medium":
      selected_stages.append(stage)
      stage.inclusion_reason = "建议包含（中等重要性）"
      stage.optional = true
    else:
      stage.inclusion_reason = "未选择（低重要性）"
      stage.optional = true
```

### Step 4: 生成推理结果

```yaml
output:
  format: "yaml"
  structure:
    reasoning_timestamp: datetime
    
    matched_rules:
      - rule_id: "R001"
        name: "external-integration"
        match_score: 0.85
        signals_found: ["http_client", "api_key_config", "third_party_sdk"]
        
    triggered_stages:
      - stage_id: "API_ANALYSIS"
        name: "API 分析"
        importance: "critical"
        required_by: ["R001"]
        focus: ["理解外部 API", "评估稳定性"]
        
      - stage_id: "SECURITY_DESIGN"
        name: "安全设计"
        importance: "high"
        required_by: ["R004"]
        focus: ["识别风险", "设计防护"]
        
    stage_sequence:
      # 按依赖关系排序
      - 1: "REQUIREMENT_ANALYSIS"
      - 2: "API_ANALYSIS"
      - 3: "DATA_MODEL_DESIGN"
      - 4: "SECURITY_DESIGN"
      - ...
      
    reasoning_explanation:
      # 人类可读的解释
      summary: "该项目涉及外部 API 集成和安全敏感数据，因此需要 API 分析、安全设计等阶段"
      details: |
        1. 检测到 OpenAI API 集成 → 需要 API 分析和成本管理阶段
        2. 检测到密码处理 → 需要安全设计阶段
        3. 检测到数据库操作 → 需要数据模型设计阶段
```

## 推理质量要求

```yaml
quality_requirements:
  # 可解释性
  explainability:
    - "每个阶段的选择都有明确的规则依据"
    - "规则匹配过程可追溯"
    - "匹配分数有计算依据"
    
  # 一致性
  consistency:
    - "相同输入产生相同输出"
    - "规则应用顺序不影响结果"
    - "边界情况有明确处理"
    
  # 完整性
  completeness:
    - "所有匹配的规则都产生阶段"
    - "所有阶段都有明确的 focus"
    - "阶段间依赖关系完整"
```

## 与其他元能力的关系

```yaml
input_from:
  - capability: "project-analysis"
    provides: ["identified_features", "business_context", "tech_stack"]
    
output_to:
  - capability: "flow-generation"
    provides: ["triggered_stages", "stage_sequence", "focus_points"]
    
  - capability: "knowledge-composition"
    provides: ["matched_rules", "stage_importance"]
```

## 示例

### 输入（项目特征）

```yaml
identified_features:
  - name: "ai-llm-project"
    confidence: "high"
    signals: ["langchain import", "openai api key"]
    
  - name: "security-sensitive"
    confidence: "medium"
    signals: ["password handling", "jwt tokens"]
    
  - name: "data-processing"
    confidence: "high"
    signals: ["database connection", "orm usage"]
```

### 输出（推理结果）

```yaml
matched_rules:
  - rule_id: "R006"
    name: "ai-llm-integration"
    match_score: 0.9
    signals_found: ["langchain_import", "openai_api_key"]
    
  - rule_id: "R004"
    name: "security-sensitive"
    match_score: 0.7
    signals_found: ["password_handling", "jwt_tokens"]
    
  - rule_id: "R002"
    name: "data-processing"
    match_score: 0.85
    signals_found: ["database_connection", "orm_usage"]

tiggered_stages:
  - stage_id: "COST_ANALYSIS"
    name: "成本分析"
    importance: "critical"
    required_by: ["R006"]
    
  - stage_id: "PROMPT_DESIGN"
    name: "Prompt 设计"
    importance: "critical"
    required_by: ["R006"]
    
  - stage_id: "SAFETY_GUARDS"
    name: "安全防护"
    importance: "high"
    required_by: ["R006"]
    
  - stage_id: "SECURITY_DESIGN"
    name: "安全设计"
    importance: "high"
    required_by: ["R004"]
    
  - stage_id: "SECURITY_REVIEW"
    name: "安全审查"
    importance: "high"
    required_by: ["R004"]
    
  - stage_id: "DATA_MODEL_DESIGN"
    name: "数据模型设计"
    importance: "critical"
    required_by: ["R002"]
    
  - stage_id: "DATABASE_DESIGN"
    name: "数据库设计"
    importance: "high"
    required_by: ["R002"]

stage_sequence:
  1: "REQUIREMENT_ANALYSIS"
  2: "COST_ANALYSIS"
  3: "PROMPT_DESIGN"
  4: "DATA_MODEL_DESIGN"
  5: "DATABASE_DESIGN"
  6: "SECURITY_DESIGN"
  7: "AI_TESTING"
  8: "DATA_TEST"
  9: "SAFETY_GUARDS"
  10: "SECURITY_REVIEW"
  11: "INTEGRATION_TEST"
```
