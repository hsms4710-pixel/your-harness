---
name: harness-designer
description: |
  Harness Designer — 根据项目特征动态生成开发流程体系。
  
  核心能力（v4.5.0）：
  - 使用元模板系统，根据项目特征动态生成产物
  - 使用项目模式知识库（project-patterns.yaml），匹配项目特征到通用模式
  - 使用特征-产物映射知识库（feature-to-product-mapping.yaml），细粒度控制生成
  - 使用工具集成知识库（tool-integration/），根据项目特征选择适用的工具
  - 动态填充模板变量，生成项目特定的内容
  - 支持模式组合（一个项目可能匹配多个模式）
  - 生成 Workflow（SKILL.md + references + scripts）
  - 按需生成 Templates（领域特定 SOP）
  - 生成 Constitution（宪法模型）和 Gates（闸门机制）
  
  重要：这是通用的开发流程生成器，不针对特定项目
  所有项目特定的内容都通过变量填充，由 harness-archaeology 在扫描时识别
  
  知识库文件（基于 GitHub Top 100 项目分析）：
  - knowledge/project-patterns.yaml: 通用项目模式定义（AI/LLM、前端、开发工具等）
  - knowledge/feature-to-product-mapping.yaml: 特征到产物的细粒度映射
  - knowledge/tool-integration/mcp-tools.yaml: 通用 MCP 工具集成规则（支持 GitHub/GitLab/Gongfeng）
  - knowledge/tool-integration/scripts.yaml: 脚本生成规则
  - meta-templates/: 通用元模板（包含 AI Agent、LLM 集成、前端等模板）
  - analysis/github-top100-analysis.yaml: GitHub Top 100 项目特征分析
  
  v4.5.0: 基于 GitHub Top 100 项目分析，扩展项目模式覆盖（AI/LLM 项目 30%）
  v4.4.0: 修正通用性问题，移除项目特定硬编码内容
  v4.3.0: 新增工具集成知识库，支持填充具体的 MCP 工具调用和脚本实现
  v4.2.0: 新增特征-产物映射知识库，支持更细粒度的生成控制
  v4.1.0: 新增元模板系统和项目模式知识库，支持动态生成
  v4.0.0: 重大重构，从"配置生成器"转型为"开发流程体系生成器"
version: 4.5.0
author: agent_created
tags: [harness, designer, workflow, templates, gates, constitution, dev-process, dynamic-generation, meta-template, knowledge-base, tool-integration, mcp, general-purpose, github-top100, ai-llm]
---

# Harness Designer

根据项目特征**动态生成**完整的开发流程体系（Dev Workflow + Templates）。

## 核心定位

```
这是一个通用的开发流程生成器，适用于任何项目。

关键原则：
  - 元模板是通用的，不包含任何项目特定内容
  - 项目特定的内容通过变量填充
  - harness-archaeology 负责识别项目特征并填充变量
  - 知识库定义"通用模式"和"通用工具"，不定义特定项目的实现

参考对象（了解深度，不复制内容）：
  - SmartMate 的 smartmate-dev-workflow/SKILL.md
  - SmartMate 的 TEMPLATE-*.md
```

## 触发条件

- 接收 harness-archaeology 的项目特征识别结果
- 基于项目特征匹配项目模式
- 使用元模板动态生成对应的开发流程体系

---

## 动态生成流程（v4.2.0 增强）

### Step 0: 加载知识库（v4.2.0 新增）

```yaml
# 加载两个知识库文件
knowledge_files:
  - path: your-harness/knowledge/project-patterns.yaml
    description: 项目模式定义和匹配规则
    version: 2.0.0
    
  - path: your-harness/knowledge/feature-to-product-mapping.yaml
    description: 特征到产物的细粒度映射
    version: 1.0.0

# 知识库包含的内容
project_patterns:
  patterns:  # 项目模式定义
    - protocol-driven
    - ai-llm-project
    - multi-repo
    - frontend-backend
    - frontend-only
    - microservice
    - data-intensive
    - security-sensitive
  
  combination_rules:  # 模式组合规则
    - patterns: ["protocol-driven", "ai-llm-project"]
      merge_strategy: union
      
  priority_rules:  # 优先级规则
    always_generate:
      - gate-protocol.md
      - check-gates.py
      
  default:  # 默认模式
    description: "适用于无法匹配特定模式的项目"

feature_to_product_mapping:
  tech_stack_mapping:  # 技术栈特征映射
    - python
    - go
    - typescript
    - rust
    - java
    
  framework_mapping:  # 框架特征映射
    - fastapi
    - nextjs
    - gin
    - spring
    
  special_flow_mapping:  # 特殊流程特征映射
    - protocol_chain
    - ai_pipeline
    - cross_repo
    - api_contract
    - microservice
    - data_pipeline
    - security_sensitive
    
  quality_requirement_mapping:  # 质量要求映射
    - high_test_coverage
    - strict_linting
    - security_scanning
    
  team_size_mapping:  # 团队规模映射
    - small
    - medium
    - large
    - has_interns
    
  deployment_mapping:  # 部署环境映射
    - kubernetes
    - docker
    - serverless
    - traditional
```

### Step 2: 多模式匹配（v4.2.0 增强）

```yaml
# 在多个知识库中进行模式匹配
match_process:
  # Step 2.1: 在 project-patterns.yaml 中匹配项目模式
  pattern_matching:
    source: knowledge/project-patterns.yaml
    patterns:
      - name: protocol-driven
        confidence_boost: 1.5
        signals:
          files: ["*.proto", "*.go"]
          directories: ["protocol", "cgi", "dbproxy"]
          
      - name: ai-llm-project
        confidence_boost: 1.3
        signals:
          code_patterns: ["openai", "anthropic", "langchain"]
          directories: ["prompts", "agents"]
          
      - name: multi-repo
        confidence_boost: 1.4
        signals:
          structure: ["多个 .git 目录"]
          
    # 计算每个模式的匹配分数
    scoring:
      exact_match: 1.0
      partial_match: 0.7
      no_match: 0.0
      
      # 应用 confidence_boost
      final_score = match_score * confidence_boost
      
    # 选择得分最高的模式（可以多选）
    selection:
      - threshold: 0.5  # 最低匹配阈值
      - allow_multiple: true  # 允许多个模式
      - max_patterns: 5  # 最多匹配 5 个模式
      
  # Step 2.2: 在 feature-to-product-mapping.yaml 中匹配特征
  feature_matching:
    source: knowledge/feature-to-product-mapping.yaml
    dimensions:
      - tech_stack_mapping  # 技术栈
      - framework_mapping  # 框架
      - special_flow_mapping  # 特殊流程
      - quality_requirement_mapping  # 质量要求
      - team_size_mapping  # 团队规模
      - deployment_mapping  # 部署环境
      
    # 每个维度独立匹配，然后合并结果
    merging:
      strategy: union  # 取并集
      priority: special_flows > tech_stack > framework  # 特殊流程优先
      
  # Step 2.3: 合并匹配结果
  result_merging:
    # 从 project-patterns.yaml 的匹配结果
    from_patterns:
      matched: [protocol-driven, multi-repo, ai-llm-project]
      generates: {...}
      
    # 从 feature-to-product-mapping.yaml 的匹配结果
    from_features:
      tech_stack: [python]
      framework: [fastapi]
      special_flows: [protocol_chain, ai_pipeline, cross_repo]
      generates: {...}
      
    # 合并生成规则
    merged_generates:
      templates: |
        from_patterns.templates + from_features.templates
        # 去重，保持顺序
        
      references: |
        from_patterns.references + from_features.references
        # 应用 priority_rules
        
      scripts: |
        from_patterns.scripts + from_features.scripts
        
      constitution: |
        from_patterns.constitution + from_features.constitution
        # 安全相关规则优先

### Step 2.5: 工具集成匹配（v4.3.0 新增）

```yaml
# 在 tool-integration/ 知识库中匹配工具集成
match_process:
  # Step 2.5.1: 检测项目使用的 MCP 工具
  mcp_detection:
    source: knowledge/tool-integration/mcp-tools.yaml
    tools:
      - name: gongfeng
        detection_rules:
          - "项目根目录有 .git 远程指向 git.woa.com"
          - "项目中引用了 mcp__gongfeng-woa 工具"
        common_use_cases:
          - 获取 MR 变更
          - 获取 MR 详情
          - 创建 MR
          - 获取文件内容
          - 获取提交列表
          
      - name: wecom
        detection_rules:
          - "项目中引用了 mcp__wecom 工具"
          
      - name: lexiang
        detection_rules:
          - "项目中引用了 mcp__lexiang 工具"
          
  # Step 2.5.2: 检测项目使用的脚本类型
  script_detection:
    source: knowledge/tool-integration/scripts.yaml
    scripts:
      - name: check-gates
        selection_rules:
          - if: "project_features.has_gongfeng_mcp == true"
            use: "with_gongfeng"
          - else:
            use: "basic"
            
      - name: check-protocol-mapping
        selection_rules:
          - if: "project_features.has_dbproxy == true"
            use: "with_dbproxy"
          - else:
            use: "basic"

  # Step 2.5.3: 生成工具集成信息
  tool_integration_result:
    mcp_tools:
      - name: gongfeng
        detected: true/false
        use_cases: ["get_merge_request_changes", "create_merge_request", ...]
        template_patterns:
          run_pattern: "mcp_call tool=\"{{tool_name}}\" arguments='{{arguments_json}}'"
          expected_pattern: "返回 {{return_description}}"
          
    scripts:
      - name: check-gates
        implementation: "with_gongfeng" | "basic"
        body_template: "完整的脚本实现"
```

### Step 3: 应用优先级规则（v4.2.0 新增）

```yaml
# 应用 knowledge/project-patterns.yaml 中的 priority_rules
priority_rules:
  # 总是生成的产物
  always_generate:
    - gate-protocol.md
    - check-gates.py
    
  # 永不生成的产物（在特定条件下）
  never_generate_when:
    frontend_only:
      - TEMPLATE-api-sync.md
      - TEMPLATE-backend-feature.md
      
  # 组合时额外生成的产物
  generate_when_combined:
    - patterns: ["protocol-driven", "multi-repo"]
      additional:
        - TEMPLATE-cross-repo-protocol-sync.md
        - check-multi-repo-protocol.py
        
# 应用 knowledge/feature-to-product-mapping.yaml 中的规则
feature_rules:
  # 技术栈特定规则
  tech_stack_rules:
    python:
      constitution:
        - rule: python-type-hints-required
          when: python_version >= "3.10"
          
    go:
      constitution:
        - rule: go-error-must-be-checked
          severity: critical

### Step 3.5: 加载元模板（v4.3.0 增强）

```yaml
# 加载元模板文件
meta_templates:
  - path: meta-templates/templates/TEMPLATE-protocol-change.md.tmpl
    type: template
    version: 2.0.0
    
  - path: meta-templates/templates/TEMPLATE-feature.md.tmpl
    type: template
    version: 2.0.0

# 元模板中的变量定义
variables:
  # 项目基本信息
  - feature_name: "功能名称"
  - project_path: "项目路径"
  
  # 协议链路相关
  - protocol_chain: ["DB", "Proto", "Tag", "下游", "云API"]
  - has_dbproxy: true/false
  - has_cgi: true/false
  
  # 工具集成相关（v4.3.0 新增）
  - has_gongfeng_mcp: true/false
  - gongfeng_project_id: "项目路径"
  - mcp_tool_patterns: "MCP 工具调用模式"
  - script_implementations: "脚本具体实现"
  
# 元模板中的条件填充（v4.3.0 新增）
conditional_fill:
  # 根据 has_gongfeng_mcp 决定是否填充工蜂 MCP 调用
  - if: "has_gongfeng_mcp == true"
    fill:
      - "Run 部分使用 mcp_call 命令"
      - "Expected 部分说明 MCP 返回格式"
      - "附录中列出工蜂 MCP 工具参考"
      
  # 根据 has_dbproxy 决定是否填充 DBProxy 检查
  - if: "has_dbproxy == true"
    fill:
      - "Step 2.2: 检查 Proto-DBProxy 映射"
      - "使用 scripts/check-protocol-mapping.py"
      - "闸门检查中包含 DBProxy 映射一致性"
      
  # 根据 script_implementations 决定脚本内容
  - if: "script_implementations.check_gates == 'with_gongfeng'"
    fill:
      - "check-gates.py 包含工蜂 MCP 集成"
      - "支持检查 MR 状态"
```

### Step 4: 渲染元模板
          
  # 框架特定规则
  framework_rules:
    fastapi:
      constitution:
        - rule: fastapi-dependency-injection
          
    nextjs:
      constitution:
        - rule: nextjs-no-relative-imports
```

### Step 4: 加载元模板

```yaml
# 根据匹配结果，加载对应的元模板
meta_templates_dir: your-harness/meta-templates/

# 元模板文件：
- templates/TEMPLATE-protocol-change.md.tmpl
- templates/TEMPLATE-feature.md.tmpl
- scripts/check-gates.py.tmpl
- references/gate-protocol.md.tmpl
- skills/SKILL.md.tmpl

# 元模板包含变量占位符：
- {{project_name}}
- {{protocol_chain_stages}}
- {{has_security_surface}}
- ...
```

### Step 4: 提取变量值

```yaml
# 从项目特征中提取变量值
variable_extraction:
  project_name: "smartmate"
  protocol_chain_stages_count: 6
  has_database_change: true
  has_proto_change: true
  has_security_surface: true
  has_multi_repo: true
  spec_dir: "docs/spec/<feature>"
  # ...
```

### Step 5: 渲染模板

```yaml
# 使用变量值渲染元模板
render_process:
  1. 读取元模板文件
  2. 替换变量占位符
     - {{project_name}} → "smartmate"
     - {{has_security_surface}} → true
     - ...
  3. 处理条件块（{{#if ...}}）
  4. 处理循环块（{{#each ...}}）
  5. 输出最终产物

# 渲染示例
input: meta-templates/templates/TEMPLATE-protocol-change.md.tmpl
variables:
  project_name: "smartmate"
  protocol_chain_stages_count: 6
  has_database_change: true
  
output: .harness/templates/TEMPLATE-protocol-change.md
  # 包含 smartmate 特定的内容
```

### Step 6: 组装产物

```yaml
# 将所有生成的产物组装到 .harness/ 目录
output_structure:
  .harness/
  ├── SKILL.md                    # 从 meta-templates/skills/SKILL.md.tmpl 渲染
  ├── templates/
  │   ├── TEMPLATE-protocol-change.md  # 从元模板渲染
  │   ├── TEMPLATE-feature.md
  │   └── ...
  ├── references/
  │   ├── gate-protocol.md        # 从元模板渲染
  │   ├── clarification-loop.md
  │   └── ...
  ├── scripts/
  │   ├── check-gates.py          # 从元模板渲染（包含项目特定配置）
  │   ├── check-dbproxy-mapping.py
  │   └── ...
  └── constitution-profiles.json  # 从匹配的规则生成
```

---

## 项目模式知识库

### 模式定义位置

```
your-harness/knowledge/project-patterns.yaml
```

### 支持的模式

| 模式 | 描述 | 触发信号 |
|------|------|----------|
| `protocol-driven` | 协议驱动开发 | proto 文件、cgi、dbproxy、yunapi |
| `ai-llm-project` | AI/LLM 项目 | LLM 调用、Prompt 工程、API Key |
| `multi-repo` | 多仓库项目 | 多个 .git 目录、跨仓库依赖 |
| `frontend-backend` | 前后端分离 | API 契约、前后端目录 |
| `frontend-only` | 纯前端项目 | 只有前端文件 |

### 模式组合

```yaml
# 知识库中的组合规则
combination_rules:
  - if: ["protocol-driven", "ai-llm-project"]
    merge_strategy: union
    
  - if: ["protocol-driven", "multi-repo"]
    merge_strategy: union
```

---

## 元模板系统

### 元模板位置

```
your-harness/meta-templates/
├── templates/
│   ├── TEMPLATE-protocol-change.md.tmpl
│   ├── TEMPLATE-feature.md.tmpl
│   └── ...
├── scripts/
│   ├── check-gates.py.tmpl
│   ├── workflow-preflight.py.tmpl
│   └── ...
├── references/
│   ├── gate-protocol.md.tmpl
│   ├── clarification-loop.md.tmpl
│   └── ...
└── skills/
    └── SKILL.md.tmpl
```

### 模板变量规范

```yaml
# 项目信息
project_name: string          # 项目名称
project_description: string   # 项目描述

# 协议链路（如果有）
protocol_chain_stages_count: number
protocol_chain_stages: array
has_database_change: boolean
has_proto_change: boolean
has_downstream_sync: boolean
has_yunapi_registration: boolean
database_sql_dir: string
proto_dir: string
cgi_dir: string
dbproxy_dir: string

# AI Pipeline（如果有）
has_ai_pipeline: boolean
has_model_config: boolean
has_prompts_dir: boolean
has_env_example: boolean

# 多仓库（如果有）
has_multi_repo: boolean
repo_list: array

# 安全和性能
has_security_surface: boolean
has_performance_surface: boolean

# 文档结构
has_spec_structure: boolean
spec_dir: string

# 闸门配置
is_clarification_required: boolean
is_prd_required: boolean
is_tasks_required: boolean
is_verify_required: boolean
is_review_required: boolean
is_ship_required: boolean
additional_gates: array
```

### 条件块语法

```markdown
{{#if has_security_surface}}
- [ ] 安全影响已评估
{{/if}}

{{#if has_protocol_chain}}
### 协议链路专用闸门
...
{{/if}}

{{#each protocol_chain_stages}}
## Task {{@index_plus_one}}: {{this.name}}
...
{{/each}}
```

---

## 分类维度（保留 v4.0）

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
├── SKILL.md                          # 主流程 Skill（类似 smartmate-dev-workflow）
├── README.md                         # 使用说明
├── constitution-profiles.json        # 宪法模型（机器可读）
├── constitution-profiles.md          # 宪法模型（人类可读）
├── hard-constraints-constitution.md  # 硬约束
├── references/                       # 流程参考文档
│   ├── gate-protocol.md              # 闸门协议
│   ├── task-execution-protocol.md    # 任务执行协议
│   ├── clarification-loop.md         # 需求澄清协议
│   ├── change-control.md             # 变更控制
│   ├── artifact-layout.md            # 产物布局
│   ├── validation-insertion.md       # 验证插入点
│   ├── review-quality-protocol.md    # 审查质量协议
│   └── ...
├── templates/                        # 开发模板（按需生成）
│   ├── TEMPLATE-feature.md           # 功能开发模板
│   ├── TEMPLATE-bugfix.md            # Bug 修复模板
│   ├── TEMPLATE-db-change.md         # 数据库变更模板
│   └── ...
└── scripts/                          # 流程检查脚本
    ├── workflow-preflight.py         # 流程预检
    ├── create-feature-constitution.py  # 生成宪法
    ├── check-gates.py                # 闸门检查
    ├── check-artifacts.py            # 产物检查
    ├── gate-status.py                # 闸门状态管理
    ├── check-hard-constraints.py     # 硬约束检查
    └── ...
```

### 中等模式（Medium Depth）

```
.harness/
├── SKILL.md                    # 主流程 Skill
├── constitution-profiles.json  # 宪法模型
├── references/                 # 流程参考文档
│   ├── gate-protocol.md
│   ├── clarification-loop.md
│   └── ...
└── scripts/                    # 流程检查脚本
    ├── check-gates.py
    ├── gate-status.py
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

## 生成流程

### Step 1: 接收项目特征

从 harness-archaeology 接收项目特征识别结果：

```yaml
# 项目特征识别结果
project_features:
  project_name: smartmate
  
  # 技术栈
  language: [Go, Python, TypeScript]
  frameworks: [tRPC, FastAPI, React]
  
  # 业务特征
  patterns:
    - 前后端分离
    - 微服务架构
    - 协议驱动开发（DBProxy/proto/cgi/YunAPI）
    
  # 识别的特殊流程
  special_flows:
    - name: protocol-chain
      description: DB → Proto → Tag → 下游同步 → 云 API 注册
      stages: 6
      
    - name: api-contract-sync
      description: 前后端 API 契约同步
      stages: 4
      
  # 质量要求
  quality_requirements:
    - 严格的 API 契约管理
    - 多阶段闸门控制
    - 完整的文档落盘
```

### Step 2: 判断生成深度

```yaml
# 基于项目特征判断
depth_decision:
  project: smartmate
  
  signals:
    - has_special_flows: true
      flows: [protocol-chain, api-contract-sync]
      
    - has_strict_gate_requirements: true
      gates: [PRD审批, 代码审查, API兼容性检查, 云API注册]
      
    - has_domain_specific_sop: true
      sos: [DBProxy变更SOP, Proto同步SOP]
      
    - general_skills_sufficient: false
      reason: "协议链路需要特定的多阶段流程"
  
  decision: full
  reason: "项目有特殊的协议驱动开发流程，需要完整的 Workflow + Templates"
```

### Step 3: 生成 Workflow（SKILL.md）

生成主流程 Skill，类似 smartmate-dev-workflow：

```markdown
---
name: <project-name>-dev-workflow
description: |
  <项目名> 开发流程编排器。
  基于 intent + surfaces + risk 分类，编排开发流程、闸门控制、产物要求。
tags: [<project-tags>]
---

# <Project Name> Dev Workflow

## Overview

本 Skill 作为 <项目名> 的开发流程编排器。它将用户请求转化为：

```
intent + surfaces + risk
```

然后编排原子 Skill、流程契约、闸门、产物和验证。

## When to Use

- 新功能开发
- Bug 修复
- 数据库变更
- 协议/Proto 变更
- API 契约变更
- 线上事故修复

## Core Principles

1. **Intent before route**: 先分类开发意图
2. **Surfaces before contracts**: 先识别影响面
3. **Risk before gates**: 先确定风险等级，再决定闸门严格程度
4. **Clarify before plan**: 澄清模糊需求
5. **Plan before code**: 先有计划再写代码
6. **Gate before next phase**: 每个阶段必须通过闸门
7. **Task before change**: 每个任务有上下文包
8. **Evidence before done**: 每个声明有证据

## Classification Model

### Intents

- `new-capability` - 新功能
- `incremental-change` - 增量变更
- `bugfix` - Bug 修复
- `incident-hotfix` - 线上事故修复
- `architecture-decision` - 架构决策
- `consistency-check` - 一致性检查

### Surfaces（可组合）

- `api-contract` - API 契约
- `data-model` - 数据模型
- `frontend-ui` - 前端 UI
- `backend-logic` - 后端逻辑
- `protocol-chain` - 协议链路（<如果项目有>）
- `security-sensitive` - 安全敏感
- `performance-sensitive` - 性能敏感

### Risk Profiles

- `low` - 低风险
- `medium` - 中风险
- `high` - 高风险
- `incident` - 线上事故

## Gate Protocol

### Required Gates

| Gate | 阶段 | 必须产物 | 继续条件 |
|------|------|----------|----------|
| `G0_CLARIFICATION` | 需求澄清后 | clarifications.md | 用户确认需求 |
| `G1_PRD` | PRD 完成后 | prd.md | 用户审批 PRD |
| `G2_TASKS` | 任务拆解后 | tasks.md | 用户审批任务 |
| `G3_SLICE` | 每个代码切片后 | develop/3X-*.md | 用户确认切片 |
| `G4_VERIFY` | 验证完成后 | test-report.md | 测试通过 |
| `G5_REVIEW` | 审查后 | review/*.md | 无阻塞问题 |
| `G6_SHIP` | 上线前 | ship-checklist.md | 用户审批上线 |

### Gate Handoff Format

每个闸门必须使用以下格式：

```markdown
## Gate: <GATE_ID>

**状态**：WAITING_USER_APPROVAL

**本阶段产物**
- <path>

**通过标准**
- [ ] <check>

**需要用户确认**
1. 通过，进入下一阶段
2. 有问题，按反馈修改
3. 暂停，不继续
```

## Routing Rules

### 当识别到 protocol-chain 影响面时

加载 `templates/TEMPLATE-protocol-change.md`，按模板中的 Step 1/2/3 执行。

### 当识别到 data-model 影响面时

加载 `templates/TEMPLATE-db-change.md`，执行数据库变更流程。

### 当识别到 security-sensitive 影响面时

在合并前插入安全检查，加载相关检查清单。

## Scripts

使用以下脚本进行确定性检查：

```bash
# 流程预检
python .harness/scripts/workflow-preflight.py --feature <name> --intent <intent> --surface <surface> --risk <risk>

# 闸门状态检查
python .harness/scripts/gate-status.py can-advance --feature <name> --gate <gate-id>

# 产物检查
python .harness/scripts/check-artifacts.py --feature <name> --stage <stage>

# 硬约束检查
python .harness/scripts/check-hard-constraints.py --feature <name>
```

## Verification

完成前确认：

- [ ] Intent、Surfaces、Risk 已分类并用户确认
- [ ] 必须的闸门已通过
- [ ] 必须的产物已生成
- [ ] 代码审查已完成
- [ ] 测试已通过
```

### Step 4: 生成 Templates（按需）

只为项目特有的流程生成 Template：

```markdown
# templates/TEMPLATE-protocol-change.md

# [Feature Name] 协议变更 Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use `executing-plans` to implement this plan task-by-task.

**Goal:** [一句话描述要做的功能/变更]

**Architecture:** 遵循 <项目的协议链路> 的 N 阶段变更链路。

## 变更范围自检清单

- [ ] 是否需要改数据库结构？
- [ ] 是否新增/修改 RPC 或 Message？
- [ ] 下游服务是否需要同步？

---

## Task 1: <第一阶段名称>

**Files:**
- 查看/修改: `<具体文件路径>`

**Step 1: <步骤名称>**

Run:
```bash
<具体命令>
```

Expected: <预期结果>

---

## Task 2: <第二阶段名称>

...

---

## 完工检查清单

- [ ] Task 1 完成
- [ ] Task 2 完成
- [ ] ...
```

### Step 5: 生成 References

生成流程参考文档：

```markdown
# references/gate-protocol.md

## Purpose

定义闸门协议，确保关键节点有人工确认。

## Universal Gate Output

每个闸门必须使用以下格式：

...

## Stop Rules

- 不得在没有显式用户批准的情况下跨越阶段闸门
- ...
```

### Step 6: 生成 Scripts

生成流程检查脚本：

```python
#!/usr/bin/env python3
"""
闸门状态检查脚本
检查特定闸门是否可以通过
"""
import json
import sys
from pathlib import Path

def check_gate(feature: str, gate_id: str) -> dict:
    """检查闸门状态"""
    gate_file = Path(f".harness/workflow-gates/{feature}.json")
    
    if not gate_file.exists():
        return {"status": "BLOCKED", "reason": "Gate state file not found"}
    
    with open(gate_file) as f:
        gate_state = json.load(f)
    
    gate = gate_state.get(gate_id)
    if not gate:
        return {"status": "BLOCKED", "reason": f"Gate {gate_id} not found"}
    
    if gate.get("status") != "approved":
        return {"status": "BLOCKED", "reason": "Gate not approved", "current": gate.get("status")}
    
    return {"status": "PASS", "approved_by": gate.get("approved_by")}

def main():
    import argparse
    parser = argparse.ArgumentParser()
    parser.add_argument("action", choices=["can-advance", "status"])
    parser.add_argument("--feature", required=True)
    parser.add_argument("--gate", required=True)
    args = parser.parse_args()
    
    result = check_gate(args.feature, args.gate)
    
    if result["status"] == "PASS":
        print(f"✅ Gate {args.gate} passed")
        sys.exit(0)
    else:
        print(f"❌ Gate {args.gate} blocked: {result['reason']}")
        sys.exit(1)

if __name__ == "__main__":
    main()
```

### Step 7: 生成 Constitution

生成宪法模型：

```json
{
  "profiles": {
    "new-capability_high": {
      "intent": "new-capability",
      "risk": "high",
      "requiredReferences": ["gate-protocol", "clarification-loop"],
      "requiredGates": ["G0_CLARIFICATION", "G1_PRD", "G2_TASKS", "G3_SLICE", "G4_VERIFY", "G5_REVIEW", "G6_SHIP"],
      "requiredChecks": ["prd-quality", "acceptance-testability", "api-compatibility"]
    },
    "incremental-change_low": {
      "intent": "incremental-change",
      "risk": "low",
      "requiredReferences": [],
      "requiredGates": ["G3_SLICE"],
      "requiredChecks": []
    }
  }
}
```

---

## 闸门机制

### 闸门定义

```yaml
gates:
  - id: G0_CLARIFICATION
    stage: 需求澄清
    required_artifacts:
      - docs/spec/<feature>/analyze/00-clarifications.md
    checks:
      - "需求已澄清"
      - "歧义已解决"
    approval_required: true
    
  - id: G1_PRD
    stage: PRD 完成
    required_artifacts:
      - docs/spec/<feature>/analyze/01-prd.md
    checks:
      - "PRD 包含完整的 User Stories"
      - "Acceptance Criteria 可测"
    approval_required: true
    
  - id: G2_TASKS
    stage: 任务拆解
    required_artifacts:
      - docs/spec/<feature>/design/01-tasks.md
    checks:
      - "任务粒度合理"
      - "依赖关系明确"
    approval_required: true
    
  - id: G3_SLICE
    stage: 代码切片
    required_artifacts:
      - 代码
      - docs/spec/<feature>/develop/3X-*.md
    checks:
      - "测试通过"
      - "代码审查通过"
    approval_required: true
    
  - id: G4_VERIFY
    stage: 验证完成
    required_artifacts:
      - docs/spec/<feature>/test/01-test-plan.md
      - docs/spec/<feature>/test/02-test-report.md
    checks:
      - "覆盖率达标"
      - "关键路径有集成测试"
    approval_required: true
    
  - id: G5_REVIEW
    stage: 审查完成
    required_artifacts:
      - docs/spec/<feature>/test/review/*.md
    checks:
      - "无阻塞问题"
    approval_required: true
    
  - id: G6_SHIP
    stage: 上线前
    required_artifacts:
      - docs/spec/<feature>/develop/ship-checklist.md
    checks:
      - "灰度方案已确认"
      - "回滚预案已准备"
    approval_required: true
```

### 闸门状态管理

```yaml
# .harness/workflow-gates/<feature>.json
gates:
  G0_CLARIFICATION:
    status: approved
    approved_by: user
    approved_at: "2026-06-18T10:00:00Z"
    evidence: "docs/spec/xxx/analyze/00-clarifications.md"
    
  G1_PRD:
    status: pending
    required_artifacts:
      - "docs/spec/xxx/analyze/01-prd.md"
    checks:
      - name: "PRD 包含完整的 User Stories"
        status: not_checked
      - name: "Acceptance Criteria 可测"
        status: not_checked
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
📊 Harness 状态
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

项目: smartmate
生成深度: full
生成时间: 2026-06-18 10:00:00

📋 生成产物
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Workflow: .harness/SKILL.md
Templates: 3 个
  • TEMPLATE-protocol-change.md
  • TEMPLATE-db-change.md
  • TEMPLATE-feature.md

References: 7 个
  • gate-protocol.md
  • task-execution-protocol.md
  • ...

Scripts: 6 个
  • workflow-preflight.py
  • check-gates.py
  • ...

Gates: 7 个
  • G0_CLARIFICATION
  • G1_PRD
  • ...

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### /harness-adjust

调整生成的配置：

```bash
/harness-adjust <配置项> <新值>
```

示例：

```bash
/harness-adjust 闸门数量 5
/harness-adjust 风险等级 high
```

### /harness-add-template

添加新的 Template：

```bash
/harness-add-template <template-name>
```

### /harness-remove-template

移除 Template：

```bash
/harness-remove-template <template-name>
```

---

## 进度反馈

### 生成进度

```
[1/7] 分析项目特征...
[2/7] 判断生成深度...
[3/7] 生成 Workflow (SKILL.md)...
[4/7] 生成 Templates...
[5/7] 生成 References...
[6/7] 生成 Scripts...
[7/7] 生成 Constitution...

✅ 生成完成

生成深度: full
生成产物:
  • SKILL.md
  • templates/TEMPLATE-protocol-change.md
  • templates/TEMPLATE-db-change.md
  • references/gate-protocol.md
  • scripts/workflow-preflight.py
  • ...
```

---

## 版本历史

- v4.0.0 (2026-06-18): 重大重构，从"配置生成器"转型为"开发流程体系生成器"
  - 新增 Workflow 生成能力
  - 新增 Template 按需生成能力
  - 新增闸门机制（7 个强制闸门）
  - 新增宪法模型
  - 引入 intent + surfaces + risk 分类模型
  - 支持三种生成深度（light/medium/full）
- v3.5.1: 历史记录保存功能
- v3.5.0: 迭代优化命令
- v3.2.0: 融合 SDD 最佳实践
- v3.1.0: CI/CD 集成
- v3.0.0: 重构为定制化系统输出
