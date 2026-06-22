---
capability: project-analysis
name: 项目特征提取
version: 1.0.0
description: |
  教 AI 如何分析项目，提取技术、架构、业务、团队四个维度的特征。
  
  这是元能力的核心，决定了后续所有推理的基础。
  
  关键原则：
  - 从代码和配置中提取，不猜测
  - 结构化输出，便于后续推理使用
  - 识别不确定性，标注需要澄清的点
---

# 元能力 1: 项目特征提取

## 核心目标

从项目代码、配置文件、目录结构中提取结构化的项目特征，为后续的开发阶段推理提供输入。

## 分析维度

### 维度 1: 技术维度 (Tech Stack)

```yaml
tech_stack:
  # 语言
  languages:
    - name: "Python"
      version: "3.11"
      confidence: "high"  # high/medium/low
      signals:
        - "pyproject.toml"
        - "python 3.11" in README
        
    - name: "Go"
      version: "1.21"
      confidence: "high"
      signals:
        - "go.mod"
        
  # 框架
  frameworks:
    - name: "FastAPI"
      confidence: "high"
      signals:
        - "from fastapi import FastAPI"
        - "uvicorn" in requirements
        
    - name: "Next.js"
      confidence: "high"
      signals:
        - "next.config.js"
        
  # 包管理器
  package_managers:
    - name: "poetry"
      confidence: "high"
      signals:
        - "pyproject.toml" with poetry config
        
    - name: "pnpm"
      confidence: "high"
      signals:
        - "pnpm-lock.yaml"
        
  # 构建系统
  build_systems:
    - name: "Webpack"
      confidence: "medium"
      signals:
        - "webpack.config.js"
        
  # 测试框架
  test_frameworks:
    - name: "pytest"
      confidence: "high"
      signals:
        - "pytest.ini"
        - "conftest.py"
        
    - name: "jest"
      confidence: "high"
      signals:
        - "jest.config.js"
        
  # CI/CD
  ci_cd:
    - name: "GitHub Actions"
      confidence: "high"
      signals:
        - ".github/workflows/"
        
    - name: "GitLab CI"
      confidence: "medium"
      signals:
        - ".gitlab-ci.yml"
```

### 维度 2: 架构维度 (Architecture)

```yaml
architecture:
  # 项目类型
  project_type:
    type: "full-stack"  # full-stack/frontend-only/backend-only/cli-tool/library/saas
    confidence: "high"
    signals:
      - "前后端目录都存在"
      - "有 API 契约文件"
      
  # 架构模式
  patterns:
    - name: "前后端分离"
      confidence: "high"
      signals:
        - "frontend/" and "backend/" directories
        - "api/" or "services/" layer
        
    - name: "微服务"
      confidence: "medium"
      signals:
        - "多个服务目录"
        - "docker-compose.yml"
        - "服务间调用代码"
        
    - name: "Monorepo"
      confidence: "medium"
      signals:
        - "packages/" or "apps/" directory
        - "turborepo" or "nx" config
        
    - name: "协议驱动"
      confidence: "low"
      signals:
        - "*.proto" files
        - "cgi/" directory
        - "dbproxy" references
        
  # 依赖关系
  dependencies:
    complexity: "medium"  # simple/medium/complex
    internal_modules: 5
    external_services: 3
    circular_dependencies: false
    
  # 部署方式
  deployment:
    type: ["docker", "kubernetes"]
    confidence: "high"
    signals:
      - "Dockerfile"
      - "k8s/" or "kubernetes/" directory
      
  # 代码规模
  code_scale:
    total_lines: 15000
    source_files: 350
    LOC_per_file_avg: 50
```

### 维度 3: 业务维度 (Business Context)

```yaml
business_context:
  # AI/LLM 相关
  ai_llm_features:
    has_ai_integration: true/false
    confidence: "high"
    signals:
      - "openai" or "anthropic" or "langchain" in code
      - "prompts/" or "agents/" directory
      - ".env.example" contains API keys
      
    llm_providers:
      - name: "OpenAI"
        confidence: "high"
        signals: ["openai", "gpt-4", "gpt-3.5"]
        
      - name: "Anthropic"
        confidence: "medium"
        signals: ["anthropic", "claude"]
        
    prompt_engineering: true/false
    has_embedding: true/false
    has_vector_store: true/false
    
  # 数据密集型
  data_intensive:
    has_large_datasets: true/false
    has_data_pipeline: true/false
    has_batch_processing: true/false
    confidence: "medium"
    signals:
      - "dask" or "spark" in dependencies
      - "data/" or "datasets/" directory
      - ".parquet" or ".csv" files
      
  # 实时性要求
  real_time_requirements:
    has_websocket: true/false
    has_streaming: true/false
    has_realtime_api: true/false
    confidence: "high"
    signals:
      - "websocket" or "socket.io" in code
      - "streaming" or "Server-Sent Events"
      
  # 安全敏感度
  security_sensitivity:
    level: "high"  # low/medium/high/critical
    confidence: "medium"
    signals:
      - "auth/" or "authentication/" directory
      - "encrypt" or "hash" in code
      - "GDPR" or "compliance" in docs
      - handles: ["payment", "pii", "healthcare"]
      
  # 性能要求
  performance_requirements:
    has_high_concurrency: true/false
    has_low_latency: true/false
    has_resource_limits: true/false
    confidence: "low"
    signals:
      - "async" or "await" heavy usage
      - "rate limiting" code
      - performance tests
```

### 维度 4: 团队维度 (Team Context)

```yaml
team_context:
  # 代码风格
  code_style:
    has_linter: true/false
    linter_type: ["ruff", "eslint", "golangci-lint"]
    has_type_hints: true/false
    has_docstrings: true/false
    confidence: "high"
    signals:
      - ".linterrc" or "pyproject.toml" with lint config
      - type hints in code
      
  # 文档完善度
  documentation:
    level: "good"  # poor/medium/good/excellent
    has_readme: true/false
    has_architecture_doc: true/false
    has_api_doc: true/false
    has_contributing_guide: true/false
    confidence: "high"
    signals:
      - "README.md" exists and size > 1KB
      - "docs/" directory
      - "ARCHITECTURE.md" or "DESIGN.md"
      
  # 现有规范
  existing_standards:
    has_code_standards: true/false
    has_review_checklist: true/false
    has_testing_standards: true/false
    confidence: "medium"
    signals:
      - "CONTRIBUTING.md"
      - ".reviewchecklist"
      - test coverage thresholds
      
  # 版本控制
  version_control:
    provider: "github"  # github/gitlab/gongfeng/bitbucket
    has_conventional_commits: true/false
    branch_strategy: "feature-branch"  # trunk/feature-branch/gitflow
    confidence: "high"
    signals:
      - remote URL
      - branch naming patterns
      
  # 协作方式
  collaboration:
    has_issue_tracking: true/false
    has_project_board: true/false
    communication_channel: "github-issues"  # github-issues/slack/teams/wecom
    confidence: "medium"
    signals:
      - ".github/ISSUE_TEMPLATE/"
      - project boards
```

## 分析流程

### Step 1: 扫描项目结构

```yaml
scan_process:
  # 目录扫描
  directory_scan:
    - check: "src/", "lib/", "app/", "packages/", "apps/"
      meaning: "源代码目录"
      
    - check: "tests/", "test/", "spec/"
      meaning: "测试目录"
      
    - check: "docs/", "documentation/"
      meaning: "文档目录"
      
    - check: "scripts/", "tools/"
      meaning: "脚本目录"
      
    - check: "config/", ".config/"
      meaning: "配置目录"
      
  # 文件扫描
  file_scan:
    - check: "package.json", "package-lock.json", "yarn.lock", "pnpm-lock.yaml"
      meaning: "Node.js 项目"
      extract: package_manager, dependencies
      
    - check: "pyproject.toml", "requirements.txt", "Pipfile"
      meaning: "Python 项目"
      extract: package_manager, dependencies
      
    - check: "go.mod", "go.sum"
      meaning: "Go 项目"
      extract: go_version, dependencies
      
    - check: "Cargo.toml"
      meaning: "Rust 项目"
      extract: dependencies
      
    - check: "pom.xml", "build.gradle"
      meaning: "Java 项目"
      extract: dependencies
      
  # 配置扫描
  config_scan:
    - check: ".github/workflows/"
      meaning: "GitHub Actions CI"
      
    - check: ".gitlab-ci.yml"
      meaning: "GitLab CI"
      
    - check: "Dockerfile", "docker-compose.yml"
      meaning: "Docker 支持"
      
    - check: "k8s/", "kubernetes/", "helm/"
      meaning: "Kubernetes 部署"
```

### Step 2: 代码模式识别

```yaml
code_pattern_detection:
  # AI/LLM 模式
  ai_llm_patterns:
    - pattern: "from openai import OpenAI"
      feature: "openai-integration"
      confidence: "high"
      
    - pattern: "from anthropic import Anthropic"
      feature: "anthropic-integration"
      confidence: "high"
      
    - pattern: "from langchain"
      feature: "langchain-framework"
      confidence: "high"
      
    - pattern: "from llama_index"
      feature: "llama-index-framework"
      confidence: "high"
      
    - pattern: "prompt_template"
      feature: "prompt-engineering"
      confidence: "medium"
      
    - pattern: "embedding", "vector_store"
      feature: "embedding-vector"
      confidence: "medium"
      
  # API 模式
  api_patterns:
    - pattern: "FastAPI()"
      feature: "fastapi-backend"
      confidence: "high"
      
    - pattern: "@app.route"
      feature: "flask-backend"
      confidence: "high"
      
    - pattern: "Next.js", "next.config"
      feature: "nextjs-frontend"
      confidence: "high"
      
    - pattern: "createApp", "pinia"
      feature: "vue-frontend"
      confidence: "high"
      
  # 数据模式
  data_patterns:
    - pattern: "SQLAlchemy", "Base"
      feature: "sqlalchemy-orm"
      confidence: "high"
      
    - pattern: "Prisma", "prisma.schema"
      feature: "prisma-orm"
      confidence: "high"
      
    - pattern: "redis"
      feature: "redis-cache"
      confidence: "medium"
      
  # 安全模式
  security_patterns:
    - pattern: "password_hash", "bcrypt"
      feature: "password-security"
      confidence: "high"
      
    - pattern: "jwt", "token"
      feature: "authentication"
      confidence: "high"
      
    - pattern: "encrypt", "cipher"
      feature: "data-encryption"
      confidence: "medium"
```

### Step 3: 特征聚合

```yaml
aggregation:
  # 从多个信号聚合到一个特征
  feature_aggregation:
    - feature: "ai-llm-project"
      required_signals:
        - "has_openai_or_anthropic_import"
        - "has_api_key_config"
      optional_signals:
        - "has_prompts_directory"
        - "has_agents_directory"
      confidence_formula: "required_all_high AND (optional_any OR medium)"
      
    - feature: "microservice-architecture"
      required_signals:
        - "has_multiple_service_directories"
        - "has_service_discovery"
      optional_signals:
        - "has_docker_compose"
        - "has_k8s_config"
      confidence_formula: "required_majority_medium"
      
    - feature: "data-intensive"
      required_signals:
        - "has_large_data_files"
        - "has_batch_processing_code"
      optional_signals:
        - "has_spark_or_dask"
        - "has_data_pipeline"
      confidence_formula: "required_any OR optional_majority"
      
  # 不确定性标注
  uncertainty_handling:
    - condition: "signals_conflict"
      action: "mark_confidence_low"
      
    - condition: "signals_insufficient"
      action: "mark_needs_clarification"
      
    - condition: "pattern_not_recognized"
      action: "flag_for_manual_review"
```

### Step 4: 输出结构化特征

```yaml
output:
  format: "yaml"
  structure:
    project_name: string
    analysis_timestamp: datetime
    
    # 四个维度
    tech_stack: {...}
    architecture: {...}
    business_context: {...}
    team_context: {...}
    
    # 识别的特征
    identified_features:
      - name: "ai-llm-project"
        confidence: "high"
        signals: [...]
        
      - name: "microservice-architecture"
        confidence: "medium"
        signals: [...]
        
    # 不确定项
    uncertainties:
      - question: "项目是否需要处理支付？"
        reason: "未发现支付相关代码或配置"
        
      - question: "是否有特殊的合规要求？"
        reason: "未发现合规相关文档"
        
    # 需要澄清的问题
    clarification_needed:
      - "项目的部署环境是什么？"
      - "团队的代码审查流程是怎样的？"
```

## 分析质量要求

```yaml
quality_requirements:
  # 信心度要求
  confidence_thresholds:
    high: ">= 3 strong signals"
    medium: "2 signals OR 1 strong + 1 medium"
    low: "1 signal OR conflicting signals"
    
  # 完整性要求
  completeness:
    - "每个维度至少有 1 个识别的特征"
    - "未识别的维度需说明原因"
    - "不确定性项需列出"
    
  # 可验证性
  verifiability:
    - "每个特征都有对应的 signals"
    - "signals 可以追溯到具体文件"
    - "confidence 可以解释"
```

## 与其他元能力的关系

```yaml
input_to:
  - capability: "stage-reasoning"
    uses: ["identified_features", "business_context"]
    
  - capability: "flow-generation"
    uses: ["tech_stack", "architecture"]
    
  - capability: "knowledge-composition"
    uses: ["identified_features"]
    
  - capability: "learning-feedback"
    uses: ["all_output"]

output_from:
  - source: "harness-archaeology"
    provides: "raw_project_scan"
```

## 示例

### 输入

```
项目路径: /path/to/my-ai-app
扫描结果: 
  - pyproject.toml (poetry, python 3.11)
  - src/agent.py (from langchain.agents import ...)
  - src/prompts/ (多个 prompt 模板文件)
  - .env.example (OPENAI_API_KEY=xxx)
  - tests/ (pytest)
  - Dockerfile
  - .github/workflows/ci.yml
```

### 输出

```yaml
project_name: my-ai-app
analysis_timestamp: "2026-06-22T15:30:00Z"

identified_features:
  - name: "ai-llm-project"
    confidence: "high"
    signals:
      - "langchain import"
      - "prompts directory"
      - "OPENAI_API_KEY in .env.example"
      
  - name: "agent-framework"
    confidence: "high"
    signals:
      - "langchain.agents import"
      - "agent.py file"
      
  - name: "containerized"
    confidence: "high"
    signals:
      - "Dockerfile exists"
      
  - name: "ci-enabled"
    confidence: "high"
    signals:
      - ".github/workflows/ci.yml"

tech_stack:
  languages: [{ name: "Python", version: "3.11", confidence: "high" }]
  frameworks: [{ name: "LangChain", confidence: "high" }]
  package_managers: [{ name: "Poetry", confidence: "high" }]
  test_frameworks: [{ name: "pytest", confidence: "high" }]

business_context:
  ai_llm_features:
    has_ai_integration: true
    llm_providers: [{ name: "OpenAI", confidence: "high" }]
    prompt_engineering: true

uncertainties: []
clarification_needed:
  - "项目是否有特定的性能要求？"
  - "是否有成本控制要求？"
```
