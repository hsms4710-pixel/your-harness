---
capability: flow-generation
name: 流程结构生成
version: 1.0.0
description: |
  教 AI 如何把推理结果转化为具体的流程定义。
  
  核心思想：所有项目都遵循相同的流程骨架，AI 的工作是填充每个阶段的具体内容。
---

# 元能力 3: 流程结构生成

## 核心目标

将开发阶段推理的结果转化为完整的流程定义（SKILL.md + templates + scripts）。

## 流程骨架（所有项目通用）

```yaml
flow_skeleton:
  # 阶段 0: 需求理解
  stage_0:
    id: REQUIREMENT_UNDERSTANDING
    name: "需求理解"
    goal: "理解要做什么"
    output: "需求文档"
    gate: "需求确认"
    universal_steps:
      - "收集需求信息"
      - "澄清模糊点"
      - "确认理解一致"
    
  # 阶段 1: 任务分解
  stage_1:
    id: TASK_DECOMPOSITION
    name: "任务分解"
    goal: "拆解为可执行任务"
    output: "任务列表"
    gate: "任务确认"
    universal_steps:
      - "拆解大任务为小任务"
      - "识别任务依赖"
      - "估算任务工作量"
    
  # 阶段 2: 设计
  stage_2:
    id: DESIGN
    name: "设计"
    goal: "确定怎么做"
    output: "设计文档"
    gate: "设计评审"
    universal_steps:
      - "确定技术方案"
      - "设计接口契约"
      - "识别风险点"
    
  # 阶段 3: 实现
  stage_3:
    id: IMPLEMENTATION
    name: "实现"
    goal: "写出代码"
    output: "代码 + 测试"
    gate: "代码审查"
    universal_steps:
      - "按任务逐个实现"
      - "编写测试"
      - "代码审查"
    
  # 阶段 4: 验证
  stage_4:
    id: VERIFICATION
    name: "验证"
    goal: "确保质量"
    output: "测试报告"
    gate: "质量检查"
    universal_steps:
      - "运行测试"
      - "检查覆盖率"
      - "验证验收标准"
    
  # 阶段 5: 上线
  stage_5:
    id: DEPLOYMENT
    name: "上线"
    goal: "安全发布"
    output: "部署配置"
    gate: "上线审批"
    universal_steps:
      - "准备部署配置"
      - "执行部署"
      - "验证上线结果"
```

## AI 的填充工作

### 1. 填充每个阶段的具体内容

```yaml
content_fill:
  # 基于项目特征，填充阶段的具体内容
  for each stage in skeleton:
    # 从推理结果中获取该阶段的关注点
    focus_points = get_stage_focus(stage.id, reasoning_result)
    
    # 生成该阶段的具体步骤
    specific_steps = []
    for focus in focus_points:
      step = generate_step(focus, project_features)
      specific_steps.append(step)
    
    # 生成该阶段的检查项
    checks = generate_checks(stage.id, project_features)
    
    # 生成该阶段的输出模板
    output_template = select_template(stage.id, project_features)
```

### 2. 生成项目特定的检查项

```yaml
check_generation:
  # 基于项目特征生成检查项
  rules:
    - if: "has_ai_integration"
      add_checks:
        - "API 成本已估算"
        - "Prompt 已测试"
        - "输出已验证"
        
    - if: "has_security_surface"
      add_checks:
        - "安全设计已完成"
        - "安全审查已通过"
        - "无高危漏洞"
        
    - if: "has_data_model_changes"
      add_checks:
        - "数据迁移已测试"
        - "回滚方案已准备"
        - "索引已优化"
        
    - if: "has_external_api_calls"
      add_checks:
        - "API 限流已处理"
        - "错误重试已实现"
        - "降级方案已准备"
```

### 3. 引用合适的 Skill

```yaml
skill_reference:
  # 根据项目特征，引用合适的 Skill
  mapping:
    - feature: "ai-llm-project"
      reference_skills:
        - "prompt-engineering-best-practices"
        - "llm-cost-optimization"
        - "ai-safety-guidelines"
        
    - feature: "security-sensitive"
      reference_skills:
        - "security-checklist"
        - "authentication-patterns"
        - "encryption-guidelines"
        
    - feature: "microservice-architecture"
      reference_skills:
        - "service-boundary-design"
        - "distributed-system-patterns"
        - "service-communication-standards"
        
    - feature: "data-processing"
      reference_skills:
        - "database-design-patterns"
        - "query-optimization-guide"
        - "data-migration-checklist"
```

## 流程定义生成

### 生成 SKILL.md 结构

```yaml
skill_md_structure:
  # 头部
  header:
    name: "{project_name}-dev-workflow"
    description: "{project_name} 开发流程编排器"
    version: "1.0.0"
    tags: ["workflow", "harness", "{project_tags}"]
    
  # 分类模型
  classification_model:
    intents:
      - "new-capability"
      - "incremental-change"
      - "bugfix"
      - "refactor"
      
    surfaces:
      - 基于项目特征动态生成
      
    risk_levels:
      - "low"
      - "medium"
      - "high"
      
  # 闸门协议
  gate_protocol:
    required_gates:
      # 基于项目特征动态生成
      - id: "G0_REQUIREMENT"
        stage: "需求理解"
        required_artifacts: ["需求文档"]
        
  # 阶段定义
  stages:
    # 从流程骨架填充
    for stage in filled_stages:
      - id: stage.id
        name: stage.name
        goal: stage.goal
        steps: stage.specific_steps
        checks: stage.checks
        output: stage.output
        gate: stage.gate
        
  # 引用的 Skills
  reference_skills:
    # 从 skill_reference 映射
    
  # 脚本
  scripts:
    # 生成检查脚本
```

### 生成 Template 结构

```yaml
template_structure:
  # 为每个特殊阶段生成 Template
  for each special_stage in triggered_stages:
    filename: "TEMPLATE-{stage_name_lowercase}.md"
    
    structure:
      # 头部
      header:
        purpose: "指导 {stage_name} 阶段的开发"
        required_skill: "executing-plans"
        
      # 变更范围自检清单
      scope_checklist:
        # 基于项目特征动态生成
        
      # Task 定义
      tasks:
        for task in stage.tasks:
          - id: "Task {index}"
            name: task.name
            files: task.files
            steps: task.steps
            
      # 完工检查清单
      completion_checklist:
        # 基于检查项生成
```

### 生成 Reference 文档

```yaml
reference_documents:
  # 闸门协议
  gate_protocol:
    filename: "references/gate-protocol.md"
    content:
      - purpose
      - universal_gate_output_format
      - stop_rules
      
  # 任务执行协议
  task_execution_protocol:
    filename: "references/task-execution-protocol.md"
    content:
      - context_package_requirements
      - execution_checkpoints
      - evidence_requirements
      
  # 需求澄清协议
  clarification_protocol:
    filename: "references/clarification-loop.md"
    content:
      - when_to_clarify
      - clarification_format
      - escalation_path
```

### 生成脚本

```yaml
scripts_generation:
  # 闸门检查脚本
  check_gates:
    filename: "scripts/check-gates.py"
    functionality:
      - "检查闸门状态"
      - "验证必需产物"
      - "输出 PASS/FAIL"
    
  # 产物检查脚本
  check_artifacts:
    filename: "scripts/check-artifacts.py"
    functionality:
      - "检查必需文件是否存在"
      - "检查文件内容完整性"
      - "输出检查报告"
      
  # 流程预检脚本
  workflow_preflight:
    filename: "scripts/workflow-preflight.py"
    functionality:
      - "检查流程配置"
      - "验证阶段顺序"
      - "输出预检报告"
```

## 流程深度选择

```yaml
depth_selection:
  # 根据项目复杂度选择生成深度
  criteria:
    full:
      conditions:
        - "has_multiple_special_stages >= 3"
        - "has_complex_architecture"
        - "has_strict_quality_requirements"
      output:
        - SKILL.md
        - templates/ (所有特殊阶段)
        - references/ (所有参考文档)
        - scripts/ (所有脚本)
        - constitution-profiles.json
        
    medium:
      conditions:
        - "has_multiple_special_stages == 1-2"
        - "has_standard_architecture"
        - "has_normal_quality_requirements"
      output:
        - SKILL.md
        - references/ (核心参考文档)
        - scripts/ (核心脚本)
        
    light:
      conditions:
        - "has_no_special_stages"
        - "has_simple_architecture"
        - "has_relaxed_quality_requirements"
      output:
        - SKILL.md (简化版)
        - scripts/ (基础脚本)
```

## 生成质量要求

```yaml
quality_requirements:
  # 完整性
  completeness:
    - "所有必需阶段都有定义"
    - "所有检查项都有具体内容"
    - "所有脚本都有实现"
    
  # 一致性
  consistency:
    - "SKILL.md 与 Template 一致"
    - "检查项与阶段匹配"
    - "闸门与阶段对应"
    
  # 可执行性
  executability:
    - "每个步骤都是可执行的"
    - "每个检查项都有验证方法"
    - "每个脚本都可以运行"
    
  # 可维护性
  maintainability:
    - "结构清晰"
    - "命名一致"
    - "注释完整"
```

## 与其他元能力的关系

```yaml
input_from:
  - capability: "project-analysis"
    provides: ["project_features", "tech_stack", "architecture"]
    
  - capability: "stage-reasoning"
    provides: ["triggered_stages", "stage_sequence", "focus_points"]
    
  - capability: "knowledge-composition"
    provides: ["combined_knowledge", "best_practices"]
    
output_to:
  - capability: "learning-feedback"
    provides: ["generated_flow", "quality_metrics"]
```

## 示例

### 输入

```yaml
# 来自 stage-reasoning 的结果
triggered_stages:
  - stage_id: "API_ANALYSIS"
    name: "API 分析"
    importance: "critical"
    focus: ["理解 OpenAI API", "评估成本", "设计调用层"]
    
  - stage_id: "SECURITY_DESIGN"
    name: "安全设计"
    importance: "high"
    focus: ["API Key 管理", "访问控制", "输出过滤"]

# 来自 project-analysis 的结果
project_features:
  languages: ["Python"]
  frameworks: ["LangChain"]
  has_ai_integration: true
  has_security_surface: true
```

### 输出

```markdown
# SKILL.md (简化示例)

---
name: my-ai-app-dev-workflow
description: |
  My AI App 开发流程编排器。
  基于 intent + surfaces + risk 分类，编排开发流程、闸门控制、产物要求。
version: "1.0.0"
tags: ["workflow", "harness", "ai", "langchain"]
---

# My AI App Dev Workflow

## Overview

本 Skill 作为 My AI App 的开发流程编排器。

## Gate Protocol

### Required Gates

| Gate | 阶段 | 必须产物 | 继续条件 |
|------|------|----------|----------|
| G0_REQUIREMENT | 需求理解 | 需求文档 | 用户确认需求 |
| G1_API_ANALYSIS | API 分析 | API 分析文档 | API 行为已理解 |
| G2_SECURITY_DESIGN | 安全设计 | 安全设计文档 | 安全方案已评审 |
| G3_IMPLEMENTATION | 实现 | 代码 + 测试 | 代码审查通过 |
| G4_VERIFICATION | 验证 | 测试报告 | 测试通过 |

## Stages

### Stage 0: 需求理解

**Goal:** 理解要做什么

**Steps:**
1. 收集需求信息
2. 澄清模糊点
3. 确认理解一致

**Output:** 需求文档

**Gate:** G0_REQUIREMENT

### Stage 1: API 分析

**Goal:** 理解 OpenAI API 的行为和限制

**Focus:**
- 理解 OpenAI API 的功能和限制
- 评估 API 调用成本
- 设计内部调用层

**Steps:**
1. 阅读 OpenAI API 文档
2. 分析 API 响应格式
3. 估算调用成本
4. 设计 Python 调用层

**Checks:**
- [ ] API 成本已估算
- [ ] 调用层设计已完成
- [ ] 错误处理策略已定义

**Output:** API 分析文档

**Gate:** G1_API_ANALYSIS

### Stage 2: 安全设计

**Goal:** 设计安全的 API Key 管理方案

**Focus:**
- API Key 管理
- 访问控制
- 输出过滤

**Steps:**
1. 设计 API Key 存储方案
2. 设计访问控制策略
3. 设计输出过滤规则

**Checks:**
- [ ] API Key 不硬编码
- [ ] 访问控制策略已定义
- [ ] 输出过滤规则已定义

**Output:** 安全设计文档

**Gate:** G2_SECURITY_DESIGN

...
```

### 生成的 Template 示例

```markdown
# templates/TEMPLATE-api-analysis.md

# [Feature Name] API 分析 Implementation Plan

**Goal:** 理解 OpenAI API 的行为和限制

## 变更范围自检清单

- [ ] 是否需要调用 OpenAI 的哪些 API？
- [ ] 预估每月调用次数？
- [ ] 是否需要处理流式响应？
- [ ] 是否需要处理 Function Calling？

---

## Task 1: API 调研

**Files:**
- 查看: `docs/openai-api-reference.md`

**Step 1: 阅读官方文档**

Run:
- 访问 https://platform.openai.com/docs
- 阅读 Chat Completions API 文档

Expected: 理解 API 的请求格式、响应格式、限制

---

## Task 2: 成本估算

**Files:**
- 创建: `docs/cost-estimation.md`

**Step 1: 计算成本**

基于预估的调用量，计算：
- GPT-4 成本
- GPT-3.5 成本
- 总月成本

Expected: 输出成本估算文档

---

## 完工检查清单

- [ ] API 功能已理解
- [ ] 成本已估算
- [ ] 调用层设计已完成
- [ ] 错误处理策略已定义
```
