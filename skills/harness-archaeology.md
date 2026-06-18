---
name: harness-archaeology
description: |
  Harness Archaeology — 从项目代码中识别特征，为 harness-designer 提供生成决策依据。
  
  核心能力：
  - 识别项目的技术栈（语言、框架、构建系统）
  - 识别项目的业务特征（前后端分离、微服务、协议驱动等）
  - 识别项目的特殊流程（协议链路、API 同步等）
  - 判断生成深度（light/medium/full）
  - 为 harness-designer 提供分类决策依据
  
  v4.0.0: 重大重构，支持 intent + surfaces + risk 分类模型，判断生成深度
version: 4.0.0
author: agent_created
tags: [harness, archaeology, code-analysis, intent, surfaces, risk, depth-decision]
---

# Harness Archaeology

从项目代码中识别特征，为 harness-designer 提供生成决策依据。

## 触发条件

- 为项目生成开发流程体系
- 需要从现有代码库识别项目模式和特征
- 需要判断生成深度（light/medium/full）

## 核心职责（v4.0 重构）

```
输入: 项目源代码
输出: 项目特征识别结果（供 harness-designer 决策）

识别维度:
  1. 技术栈特征（语言、框架、构建系统）
  2. 业务特征（前后端分离、微服务、协议驱动等）
  3. 特殊流程（协议链路、API 同步等）
  4. 质量要求（测试、审查、文档）

决策输出:
  - intent: 开发意图类型
  - surfaces: 影响面列表（可组合）
  - risk: 风险等级
  - depth: 生成深度（light/medium/full）
```

## 分类模型（v4.0 新增）

### Intents（开发意图）

```yaml
intents:
  - id: new-capability
    description: 新功能开发
    signals:
      - 新增 API 端点
      - 新增业务模块
      - 新增 UI 页面
      
  - id: incremental-change
    description: 增量变更
    signals:
      - 修改现有功能
      - 优化性能
      - 代码重构
      
  - id: bugfix
    description: Bug 修复
    signals:
      - 修复已知问题
      - 回归测试
      
  - id: incident-hotfix
    description: 线上事故修复
    signals:
      - 紧急修复
      - 灰度发布
      
  - id: architecture-decision
    description: 架构决策
    signals:
      - 技术选型
      - 模块划分
      
  - id: consistency-check
    description: 一致性检查
    signals:
      - 契约验证
      - 文档同步
```

### Surfaces（影响面，可组合）

```yaml
surfaces:
  - id: api-contract
    description: API 契约
    signals:
      - 存在 API 定义文件
      - 有前端 API 调用
      - 有 API 文档
      
  - id: data-model
    description: 数据模型
    signals:
      - 存在数据库迁移
      - 有 ORM 模型
      - 有数据验证
      
  - id: frontend-ui
    description: 前端 UI
    signals:
      - 存在前端组件
      - 有 UI 框架
      - 有样式系统
      
  - id: backend-logic
    description: 后端逻辑
    signals:
      - 存在业务逻辑层
      - 有服务层
      
  - id: protocol-chain
    description: 协议链路（如 DBProxy/proto/cgi/YunAPI）
    signals:
      - 存在 proto 文件
      - 有代码生成
      - 有下游同步
      
  - id: security-sensitive
    description: 安全敏感
    signals:
      - 存在认证逻辑
      - 有加密处理
      - 有权限控制
      
  - id: performance-sensitive
    description: 性能敏感
    signals:
      - 存在性能监控
      - 有缓存策略
      - 有并发处理
      
  - id: deployment-config
    description: 部署配置
    signals:
      - 存在部署脚本
      - 有环境配置
      
  - id: docs-sync
    description: 文档同步
    signals:
      - 存在文档目录
      - 有文档生成
```

### Risk（风险等级）

```yaml
risks:
  - id: low
    description: 低风险
    signals:
      - 文档更新
      - 样式调整
      - 非核心功能
      
  - id: medium
    description: 中风险
    signals:
      - API 修改
      - 业务逻辑变更
      - 依赖升级
      
  - id: high
    description: 高风险
    signals:
      - 数据库迁移
      - 认证变更
      - 核心流程
      
  - id: incident
    description: 线上事故
    signals:
      - 紧急修复
      - 回滚操作
```

## 生成深度决策（v4.0 新增）

```yaml
# 基于识别结果判断生成深度
depth_decision:
  
  # 特征信号收集
  signals:
    special_flows:
      - name: protocol-chain
        detected: true/false
        description: DB → Proto → Tag → 下游同步
        
      - name: api-contract-sync
        detected: true/false
        description: 前后端 API 契约同步
        
    strict_gate_requirements:
      detected: true/false
      gates: [PRD审批, 代码审查, API兼容性检查]
      
    domain_specific_sop:
      detected: true/false
      sos: [DBProxy变更SOP, Proto同步SOP]
      
    general_skills_sufficient:
      value: true/false
      reason: "通用 Skill 能否覆盖"
  
  # 深度决策
depth:
  - id: full
    condition:
      - special_flows.detected == true
      OR strict_gate_requirements.detected == true
      OR domain_specific_sop.detected == true
    generate:
      - workflow (SKILL.md)
      - templates
      - references
      - scripts
      - constitution
    
  - id: medium
    condition:
      - general_skills_sufficient == false
      AND (special_flows OR strict_gate_requirements OR domain_specific_sop) == false
    generate:
      - workflow (SKILL.md)
      - references
      - scripts
      - constitution
    
  - id: light
    condition:
      - general_skills_sufficient == true
    generate:
      - workflow (SKILL.md)
      - scripts (基础闸门检查)
```

## 输出格式（v4.0 更新）

```yaml
# harness-archaeology 输出结果
project_features:
  project_name: smartmate
  
  # 技术栈
  languages: [Go, Python, TypeScript]
  frameworks: [tRPC, FastAPI, React]
  
  # 业务特征
  patterns:
    - 前后端分离
    - 微服务架构
    - 协议驱动开发
    
  # 特殊流程检测
  special_flows:
    - name: protocol-chain
      detected: true
      stages: 6
      description: DB → Proto → Tag → 下游同步 → 云 API 注册
      
    - name: api-contract-sync
      detected: true
      stages: 4
      
  # 质量要求
  quality_requirements:
    - 严格的 API 契约管理
    - 多阶段闸门控制
    - 完整的文档落盘
    
  # 分类结果
  classification:
    intent: new-capability
    surfaces:
      - api-contract
      - data-model
      - protocol-chain
      - security-sensitive
    risk: high
    
  # 生成深度决策
  depth_decision:
    depth: full
    reason: "项目有特殊的协议驱动开发流程，需要完整的 Workflow + Templates"
    generate:
      - workflow
      - templates
      - references
      - scripts
      - constitution
```

## 核心原则

**智能协作，而非盲目自动化**。

```
❌ 盲目自动化（v3.0-3.3 的问题）：
  扫描项目 → 输出推断结果 → 直接生成
  → 推断可能不准确
  → 用户不知道为什么这样推断
  → 生成的内容可能不符合预期

✅ 智能协作（v3.4 的方式）：
  扫描项目 → 展示推断结果 → 等待用户确认/修正
          → 基于确认的结果生成
          → 支持用户调整后再生成
```

### 确认环节（v3.4 新增）

**核心原则**：关键决策点必须与用户确认，不盲目推断。

#### 确认点设计

```yaml
# 确认点配置

confirmation_points:
  # 确认点 1: 项目特征推断
  - id: project_features
    trigger: "扫描完成后"
    title: "📋 项目特征识别结果"
    message_template: |
      我识别到以下项目特征：
      
      **语言和框架**
      - 主语言: {language}
      - 框架: {framework}
      - 包管理器: {package_manager}
      
      **工具链**
      - Lint: {lint_tool}
      - 测试: {test_framework}
      - CI 平台: {ci_platform}
      
      **项目模式**
      - 模式: {project_pattern}
      - 置信度: {confidence}
      
      这些识别结果正确吗？有需要修正的吗？
    options:
      - label: "✅ 全部正确"
        action: "continue"
      - label: "✏️ 需要修正"
        action: "enter_modification"
      - label: "❓ 我也不确定"
        action: "show_details_and_recommend"
    wait_for_user: true
    allow_modification: true

  # 确认点 2: 存量代码问题
  - id: legacy_code_handling
    trigger: "发现存量代码问题时"
    title: "⚠️ 存量代码问题"
    message_template: |
      扫描发现 {total_count} 个文件可能存在规范问题：
      
      **问题分类**
      - 代码风格问题: {style_count} 个
      - 类型错误: {type_count} 个
      - 测试缺失: {test_count} 个
      - 文档缺失: {doc_count} 个
      
      **影响范围**
      {affected_files_preview}
      
      您希望如何处理这些存量代码？
    options:
      - label: "🔧 立即全部修复"
        description: "生成修复脚本，立即修复所有问题"
        impact: "生成 fix-all.sh 脚本，可能耗时较长"
      - label: "📊 渐进式收紧（推荐）"
        description: "分阶段修复，先对新代码强制检查"
        impact: "生成收紧计划，3-6个月逐步修复"
      - label: "⏭️ 暂不处理"
        description: "只对新提交的代码进行检查"
        impact: "存量代码标记为 exempt，只检查新代码"
    wait_for_user: true
    default: "📊 渐进式收紧（推荐）"

  # 确认点 3: 优先级确认
  - id: priority_confirmation
    trigger: "准备生成时"
    title: "🎯 生成优先级确认"
    message_template: |
      基于您的需求和项目特征，我建议按以下优先级生成：
      
      **高优先级** (必须生成)
      {high_priority_items}
      
      **中优先级** (建议生成)
      {medium_priority_items}
      
      **低优先级** (可选生成)
      {low_priority_items}
      
      您希望调整优先级吗？
    options:
      - label: "✅ 按建议生成"
        description: "按上述优先级生成所有内容"
        action: "generate_all"
      - label: "📝 调整优先级"
        description: "修改某些项的优先级"
        action: "enter_priority_adjustment"
      - label: "⚡ 只生成高优先级"
        description: "只生成高优先级内容，节省时间"
        action: "generate_high_only"
    wait_for_user: true

  # 确认点 4: 最终确认
  - id: final_confirmation
    trigger: "生成前"
    title: "🚀 开始生成确认"
    message_template: |
      确认生成以下内容：
      
      **输出目录**: `.harness/`
      
      **将生成的内容**:
      - constitution.md (项目治理原则)
      - scripts/ (定制脚本: {script_count} 个)
      - hooks/ (自动化 Hook: {hook_count} 个)
      - skills/ (可调用 Skill: {skill_count} 个)
      - workflows/ (工作流: {workflow_count} 个)
      {ci_config_if_applicable}
      
      **不会影响的内容**:
      - 不修改现有代码
      - 不删除现有文件
      - 所有生成的文件都在 .harness/ 目录
      
      确认开始生成吗？
    options:
      - label: "🚀 开始生成"
        action: "start_generation"
      - label: "↩️ 返回修改"
        action: "return_to_previous"
      - label: "❌ 取消"
        action: "cancel"
    wait_for_user: true
```

#### 确认流程实现

```
扫描项目
    ↓
【步骤 1】项目特征识别
    - 检测语言、框架、工具链
    - 识别项目模式
    - 计算置信度
    
【步骤 2】展示推断结果
    展示识别到的特征：
    - 主语言: Python
    - 框架: FastAPI
    - 包管理器: PDM
    - ...
    
【步骤 3】等待用户确认
    用户可以：
    - 确认全部正确
    - 指出需要修正的地方
    - 表示不确定（显示更多细节）
    
【步骤 4】处理存量代码问题（如有）
    展示发现的问题：
    - 45 个文件有规范问题
    - 提供处理选项
    
【步骤 5】确认优先级
    展示生成优先级：
    - 高优先级: check-pydantic-sync.py
    - 中优先级: check-tdd.py
    - 低优先级: check-performance.py
    
【步骤 6】最终确认并生成
```

#### 置信度分层

```yaml
# 不同置信度的处理方式

confidence_levels:
  high: 
    threshold: 0.9
    handling: "直接使用，不需要特别确认"
    example: "检测到 pyproject.toml + pdm.lock → 确定是 Python + PDM"
    
  medium:
    threshold: 0.7
    handling: "展示推断结果，建议用户确认"
    example: "检测到 fastapi 依赖 → 推断是 FastAPI 项目（也可能是其他框架）"
    
  low:
    threshold: 0.5
    handling: "必须用户确认，提供多个候选"
    example: "检测到多个框架依赖 → 无法确定主框架"

  uncertain:
    threshold: 0.3
    handling: "询问用户，不进行推断"
    example: "项目结构不清晰 → 询问用户项目类型"
```

#### 确认输出格式

```
📋 项目特征识别结果

**语言和框架**
- 主语言: Python (置信度: 高)
- 框架: FastAPI (置信度: 高)
- 包管理器: PDM (置信度: 高)

**工具链**
- Lint: ruff (置信度: 高)
- 测试: pytest (置信度: 高)
- CI 平台: GitHub Actions (置信度: 高)

**项目模式**
- 模式: API-Driven Development (置信度: 中)
- 特征依据: 
  • 存在 routers/ 目录
  • 存在 Pydantic models
  • 有 OpenAPI 文档生成

**项目规模**
- 源文件数: 1121
- 目录结构: 标准 FastAPI 项目
- 复杂度: 中等

这些识别结果正确吗？有需要修正的吗？

[✅ 全部正确] [✏️ 需要修正] [❓ 我也不确定]
```

## 语言检测矩阵（v3.1 增强版）

### 编程语言识别

| 语言 | 源文件模式 | 配置文件 | 依赖管理 | 构建工具 |
|------|-----------|---------|---------|---------|
| JavaScript | `*.js`, `!*.test.js` | `package.json` | npm/pnpm/yarn/bun | Webpack/Vite/esbuild/tsc |
| TypeScript | `*.ts`, `!*.test.ts` | `tsconfig.json` | npm/pnpm/yarn/bun | Webpack/Vite/esbuild/tsc |
| Go | `*.go` | `go.mod` | go modules | go build/Make |
| Rust | `*.rs` | `Cargo.toml` | Cargo | cargo build |
| Python | `*.py` | `pyproject.toml`, `setup.py` | pip/poetry/**uv** | pytest/py.test |
| Haskell | `*.hs` | `package.yaml`, `*.cabal` | Cabal/Stack | cabal build/stack build |
| Java | `*.java` | `pom.xml`, `build.gradle` | Maven/Gradle | mvn/gradle |
| C | `*.c`, `*.h` | `Makefile`, `CMakeLists.txt` | Make/CMake | gcc/clang |
| C++ | `*.cpp`, `*.cc`, `*.cxx` | `CMakeLists.txt` | CMake | g++/clang++ |
| Ruby | `*.rb` | `Gemfile` | Bundler | rake |
| PHP | `*.php` | `composer.json` | Composer | php |
| .NET | `*.cs`, `*.fs` | `*.csproj`, `*.sln` | NuGet | dotnet build |
| Scala | `*.scala` | `build.sbt`, `pom.xml` | SBT/Maven | sbt |
| Proto/gRPC | `*.proto` | `buf.yaml` | buf | buf generate |

### 多语言项目检测（v2.5 增强）

当检测到多个语言标志时，按以下规则处理：

```
1. 列出所有检测到的语言
2. 识别主语言：
   - 统计各语言源文件数量
   - 主语言 = 文件数最多的语言
   - 或从主配置文件的 main/module 字段推断

3. 识别辅助语言：
   - 辅助语言 = 用于构建工具、原生绑定、性能优化
   - 常见组合：
     * TypeScript + Rust (原生绑定)
     * JavaScript + Rust (编译器)
     * Go + C (系统调用)
     * Python + C++ (性能优化)
     * PHP + JavaScript (前端依赖)

4. 子目录语言检测（v2.5 增强）：
   - 扫描 python/, src/, lib/ 等子目录
   - 识别多语言项目结构
   - 常见结构：
     * python/ → Python 项目
     * R/ → R 项目
     * src/ → 主语言源码
     * lib/ → 库代码

5. 分别生成规范：
   - 为主语言生成完整规范
   - 为辅助语言生成简要规范（标注为辅助）
```

### 语言优先级（v3.1 更新）

| 优先级 | 条件 | 语言 | 备注 |
|--------|------|------|------|
| 1 | 有 `composer.json` 且有 PHP 代码 | PHP | **v2.6 提升** |
| 2 | 有 `pyproject.toml` 且有 Python 代码 | Python | - |
| 3 | 有 `go.mod` 且有 Go 代码 | Go | - |
| 4 | 有 `Cargo.toml` 且有 **5+** Rust 文件 | Rust | - |
| 5 | 有 `package.yaml` 或 `*.cabal` 且有 `*.hs` | Haskell | **v3.1 新增** |
| 6 | 有 `build.gradle` 或 `pom.xml` 且有 Java 代码 | Java | - |
| 7 | 有 `package.json` 且有 TypeScript 配置 | TypeScript | - |
| 8 | 有 `package.json` 且无 TypeScript | JavaScript | - |
| 9 | 有 `buf.yaml` 且有 `*.proto` | Proto/gRPC | **v3.1 新增** |
| 10 | 有 `CMakeLists.txt` | C/C++ | - |
| 11 | 有 `Makefile` 且无其他配置 | C/C++ | - |

### 源文件计数检测（v3.1 更新）

```
当配置文件无法明确判断时，使用源文件计数：

1. 统计各语言源文件数量（排除 node_modules, vendor, .git 等）
2. 比较数量确定主语言：
   - TypeScript/JavaScript: *.ts + *.js（排除 node_modules, .next）
   - Rust: *.rs
   - Go: *.go
   - Python: *.py（排除 site-packages）
   - Haskell: *.hs（排除 dist-newstyle）
   - PHP: *.php（排除 vendor）
   - Java: *.java
   - Proto: *.proto

3. 特殊处理 C/C++ 项目：
   - 如果 python/ 目录存在且有 *.py → 可能是 Python 绑定
   - 检查 src/ 或 lib/ 目录中的 *.c/*.cpp
   - 如果 src/*.c 或 src/*.cpp 存在 → C/C++ 主语言

4. 辅助语言识别：
   - 如果 Rust 文件 < TypeScript/JavaScript 文件
   - 且 package.json 存在 → Rust 为辅助语言
   - 示例：Next.js (14729 ts/js, 993 rs auxiliary)

5. Haskell 项目识别（v3.1 新增）：
   - 检查 package.yaml 或 *.cabal 文件
   - 检查 .ghcversion 或 stack.yaml
   - 常见框架：Yesod, Servant, MTL
   - 构建工具：Cabal, Stack, Nix
```

### Python Monorepo (uv) 检测（v3.1 新增）

Python 项目使用 uv 进行依赖管理和包管理时：

```
检测标志：
1. 存在 uv.lock 文件
2. 存在多个 pyproject.toml（在子目录中）
3. pyproject.toml 中有 [tool.uv.sources] 配置

目录结构示例（LangChain）：
langchain/
├── libs/
│   ├── core/pyproject.toml      # langchain-core
│   ├── langchain/pyproject.toml # langchain-classic
│   ├── langchain_v1/pyproject.toml # langchain v1
│   └── partners/
│       ├── openai/pyproject.toml
│       ├── anthropic/pyproject.toml
│       └── ... (15+ 集成包)
├── uv.lock                       # 锁定依赖
└── pyproject.toml                # 主配置

生成脚本：
├── scripts/check-package-deps.py
│   ├── 检查包间依赖一致性
│   ├── 验证版本兼容性
│   └── 检查循环依赖
└── scripts/check-uv-sync.py
    ├── 检查 uv.lock 是否最新
    └── 验证依赖版本
```

### AI/LLM 项目细分（v3.1 新增）

AI/LLM 项目需要更细的分类：

```
AI 项目类型：
1. AI Framework（如 LangChain）
   - 提供核心抽象（ChatModel, Agent, Chain）
   - 多 Provider 集成（partners/）
   - 完整的应用开发能力
   
2. AI SDK（如 Vercel AI SDK）
   - 封装特定平台的 AI API
   - Provider 模式（packages/anthropic, packages/openai）
   - 统一的使用接口
   
3. AI Application（如某个 AI Agent）
   - 使用 AI Framework/SDK 构建
   - 特定业务场景
   - 配置模型和提示词

检测标志：
- pyproject.toml 依赖: langchain, openai, anthropic, transformers, torch
- package.json 依赖: @ai-sdk, openai, anthropic
- 代码中包含: LLM, ChatModel, Embedding, Agent, Tool, Chain
- 目录结构: libs/partners/ (Framework), packages/ (SDK)

生成脚本（Framework/SDK）：
├── scripts/check-api-keys.py
│   ├── 检查 API Key 环境变量配置
│   ├── 验证 Key 是否有效
│   └── 检测硬编码 Key
├── scripts/check-provider-compat.py
│   ├── 检查 Provider 实现完整性
│   ├── 验证 API 版本兼容性
│   └── 检查模型配置
└── scripts/estimate-tokens.py
    └── 估算 API 调用成本

生成脚本（Application）：
├── scripts/check-model-config.py
│   ├── 检查模型配置完整性
│   └── 验证提示词模板
└── scripts/monitor-cost.py
    └── 监控 API 调用成本
```

### 微服务/gRPC 检测（v3.1 新增）

微服务项目通常使用 gRPC 进行服务间通信：

```
检测标志：
1. 存在 *.proto 文件
2. 存在 buf.yaml 或 buf.gen.yaml
3. 目录结构: proto/, grpc/, rpc/
4. 代码中包含: grpc, protobuf, service definition

目录结构示例（Kratos）：
kratos/
├── proto/
│   └── oidc/v1/state.proto
├── buf.yaml
├── buf.gen.yaml
└── oryx/  # gRPC 生成的代码

生成脚本：
├── scripts/check-grpc-contract.py
│   ├── 检查 proto 文件语法
│   ├── 验证服务定义完整性
│   └── 检查字段兼容性
├── scripts/check-service-deps.py
│   ├── 检查服务依赖关系
│   ├── 验证配置一致性
│   └── 检查循环依赖
└── scripts/generate-grpc-docs.py
    └── 生成 gRPC API 文档
```

### 安全项目检测（v3.1 新增）

安全敏感项目需要特殊处理：

```
检测标志：
1. 存在 SECURITY.md 文件
2. 存在 .grype.yaml (CVE 扫描配置)
3. 存在 .snyk (Snyk 配置)
4. golangci-lint 配置中有 forbidigo 规则
5. 项目类型: 认证、加密、支付、医疗

安全相关配置示例（Kratos）：
- SECURITY.md: 安全报告流程
- .goreleaser.yml: 发布安全检查
- .grype.yaml: CVE 扫描
- .golangci.yml: forbidigo 规则（禁止 http.DefaultClient）

生成脚本：
├── scripts/check-security-config.py
│   ├── 检查 SECURITY.md 存在性
│   ├── 验证 CVE 扫描配置
│   └── 检查安全头配置
├── scripts/check-sensitive-data.py
│   ├── 检查硬编码密钥
│   ├── 验证环境变量使用
│   └── 检测敏感信息泄露
└── scripts/verify-cve-scan.py
    └── 验证 CVE 扫描结果
```

## 构建系统检测矩阵（v3.1 增强版）

| 构建系统 | 检测标志 | 配置文件 | 复杂度 |
|---------|---------|---------|--------|
| npm | `package.json` + 无 lock 文件 | `package.json` | 低 |
| yarn | `yarn.lock` | `package.json` | 低 |
| pnpm | `pnpm-lock.yaml` 或 `pnpm-workspace.yaml` | `pnpm-workspace.yaml` | 中 |
| bun | `package.json` + bun 配置 | `bunfig.toml` | 低 |
| Vite | `vite.config.*` | `vite.config.ts/js` | 中 |
| Webpack | `webpack.config.*` | `webpack.config.js` | 中 |
| esbuild | `esbuild` 依赖 | 无专用配置 | 低 |
| **Turborepo** | `turbo.json` | `turbo.json` | 高 |
| Bazel | `WORKSPACE`, `BUILD` | `WORKSPACE` | 高 |
| Nx | `nx.json` | `nx.json` | 高 |
| Cargo | `Cargo.toml` | `Cargo.toml` | 中 |
| Go build | `go.mod` | `go.mod` | 低 |
| **Make** | `Makefile`, `makefile` | `Makefile` | 中 |
| CMake | `CMakeLists.txt` | `CMakeLists.txt` | 高 |
| Just | `justfile` | `justfile` | 低 |
| Maven | `pom.xml` | `pom.xml` | 中 |
| Gradle | `build.gradle`, `build.gradle.kts` | `build.gradle` | 中 |
| Composer | `composer.json` | `composer.json` | 低 |
| Poetry | `pyproject.toml` + poetry 配置 | `pyproject.toml` | 低 |
| **uv** | `uv.lock` | `pyproject.toml` + `uv.lock` | 低 |
| **PDM** | `pdm.lock` 或 `pyproject.toml` + pdm 配置 | `pyproject.toml` | 低 |
| Cabal | `package.yaml`, `*.cabal` | `package.yaml` | 中 |
| Stack | `stack.yaml` | `stack.yaml` | 中 |
| buf | `buf.yaml` | `buf.yaml` | 中 |
| **goreleaser** | `.goreleaser.yml` | `.goreleaser.yml` | 中 |

### Monorepo 检测

| Monorepo 工具 | 检测标志 | 配置文件 |
|--------------|---------|---------|
| pnpm workspace | `pnpm-workspace.yaml` | `pnpm-workspace.yaml` |
| npm workspace | `package.json` + `workspaces` 字段 | `package.json` |
| **Lerna** | `lerna.json` | `lerna.json` |
| Turborepo | `turbo.json` | `turbo.json` |
| Nx | `nx.json` | `nx.json` |
| Bazel | `WORKSPACE` + `MODULE.bazel` | `WORKSPACE` |
| Cargo workspace | `Cargo.toml` + `[workspace]` | `Cargo.toml` |
| PHP Composer | `composer.json` + `require` 数组 | `composer.json` |
| **uv workspace (PDM)** | `pyproject.toml` + `[tool.pdm]` 或 `pdm.lock` | `pyproject.toml` |
| **LangChain libs/** | `libs/*/pyproject.toml` | `libs/*/pyproject.toml` |

### Monorepo 目录结构识别

```
常见 Monorepo 目录结构：

packages/    → npm/pnpm workspace (最常见)
libs/        → Python uv workspace (LangChain) / Nx 工作区
apps/        → 应用入口
apps/*/      src/ → 应用源码
crates/      → Rust workspace
projects/    → .NET 解决方案
modules/     → 通用模块
server/      → Java/Elasticsearch
plugins/     → Java/Elasticsearch

AI 项目特定结构：
libs/partners/    → LangChain 集成层
libs/core/        → LangChain 核心
packages/*        → Vercel AI SDK providers
```

## 项目复杂度评估（v2.4 增强版）

### 复杂度等级

| 复杂度 | 条件 | 置信度阈值 | 处理策略 |
|--------|------|-----------|---------|
| **极低** | 单文件项目、脚本 | 0.95+ | 快速扫描，高置信度 |
| **低** | 单语言 + 单模块 + 简单测试 | 0.85-0.95 | 标准扫描 |
| **中** | 单语言 + 多模块 + 标准测试 | 0.70-0.85 | 深度扫描 |
| **高** | 多语言 或 Monorepo 或 复杂构建 | 0.50-0.70 | 分模块扫描 + 人工确认 |
| **极高** | 多语言 + Monorepo + 自定义构建 | 0.30-0.50 | 逐模块分析 + 大量人工确认 |

### 复杂度评分规则（v2.12 更新）

```yaml
评分维度:
  文件数量（v2.12 新增，权重最高）:
    0-50: 0
    51-200: +1
    201-500: +2
    501-1000: +3
    1001-2000: +4
    2000+: +5
  
  语言数量:
    单语言: 0
    多语言: +2
  
  项目结构:
    单模块: 0
    多模块: +1
    Monorepo: +2
    嵌套 Monorepo: +3
  
  构建系统:
    简单 (npm/go build): 0
    中等 (Vite/Cargo): +1
    复杂 (Turborepo/Bazel): +2
    自定义: +3
  
  测试配置:
    无配置: 0
    标准配置: +1
    多种测试框架: +2
    自定义测试: +3
  
  CI/CD:
    简单 workflow: 0
    多 workflow: +1
    复杂矩阵: +2
    自定义 CI: +3
  
  框架复杂度:
    简单框架: 0
    全栈框架: +1
    多框架组合: +2
  
  AI/LLM 项目（v2.12 新增）:
    否: 0
    是: +2

复杂度等级:
  0-3: 极低
  4-6: 低
  7-9: 中
  10-12: 高
  13-15: 极高
  16+: 超高（罕见）

# 示例:
# LangChain: 2511 文件(5) + AI项目(2) = 7+ → 高 → 实际评为"极高"
# Vite: 1423 文件(3) + Monorepo(2) = 5+ → 中 → 实际评为"高"
# dbproxy: 83 文件(0) + Go(1) = 1 → 低
```

## 信号源扫描（v2.12 增强版）

### 基础信号源

| 信号源 | 扫描什么 | 推断出什么 |
|--------|----------|-----------|
| package.json / pyproject.toml | 依赖、scripts、lint 配置 | 技术栈、工具链 |
| tsconfig.json / mypy.ini | 类型检查严格度 | 质量门禁基线 |
| .github/workflows/ / Jenkinsfile | CI 流水线步骤 | 部署流程、测试要求 |
| .eslintrc / ruff.toml / clippy.toml | Lint 规则 | 代码风格约定 |
| docker-compose.yml / Dockerfile | 服务拓扑 | 架构模式（单体/微服务） |
| 目录结构 | src/ vs app/ vs cmd/ | 项目组织约定 |
| Git log | commit message 格式、分支模式 | 工作流约定 |
| README / CONTRIBUTING.md | 开发指南、设计哲学 | 团队显式规范 |

### CI Workflow 深度分析（v2.12 新增）

如果存在 `.github/workflows/` 或 `.gitlab-ci.yml`，深入分析：

```
分析内容：
1. CI 检查项列表
   - 识别 lint、typecheck、test、build 等检查
   - 输出: 质量门禁清单

2. 运行条件
   - push/PR/merge 时的检查
   - 分支策略（main/develop/feature/*）
   - 输出: 工作流触发条件

3. 检查顺序
   - 并行 vs 串行
   - 快速失败 vs 全部运行
   - 输出: 执行策略

4. 矩阵配置
   - 多 Node 版本
   - 多 Python 版本
   - 多操作系统
   - 输出: 兼容性要求
```

### CONTRIBUTING.md 解析（v2.12 新增）

如果存在 CONTRIBUTING.md，提取以下信息：

```
解析内容：
1. 设计哲学
   - 关键词: philosophy, principles, goals, think before
   - 输出: 项目核心设计理念

2. 依赖管理策略
   - 关键词: dependencies, devDependencies, bundle, vendor
   - 输出: 依赖管理规范

3. 发布流程
   - 关键词: release, publish, version, changesets
   - 输出: 发布规范

4. 代码风格
   - 关键词: style, format, lint, formatter
   - 输出: 代码风格规范

5. 测试策略
   - 关键词: test, unit test, e2e, coverage
   - 输出: 测试要求
```

### 测试信号源

| 信号源 | 扫描什么 | 推断出什么 |
|--------|----------|-----------|
| jest.config.ts / vitest.config.ts | 测试环境、覆盖率阈值、mock 配置 | 测试策略、E2E vs Unit |
| playwright.config.ts | 浏览器测试配置 | 端到端测试策略 |
| cypress.config.js | Cypress 配置 | UI 测试策略 |
| go test -cover | Go 覆盖率要求 | 测试质量 |
| cargo test | Rust 测试配置 | 测试质量 |
| phpunit.xml | PHP 测试配置 | 测试质量 |
| pytest.ini / pytest.cfg | Python 测试配置 | 测试质量 |
| **测试文件模式** | 分析 3-5 个测试文件 | 测试组织方式、mock 模式 |
| **pytest-asyncio** | Python 异步测试配置 | 异步测试策略 |

### AI/LLM 项目检测（v2.12 新增）

检测标志：
```
Python 项目 AI 检测：
- requirements.txt 包含: langchain, openai, anthropic, transformers, torch, llama-index, langgraph, crewai
- pyproject.toml 依赖: 上述任一
- 代码中包含: LLM, ChatModel, Embedding, Agent, Tool, Chain

TypeScript/JS 项目 AI 检测：
- package.json 依赖: @ai-sdk, openai, anthropic, langchain
- 代码中包含: AI model, chat completion, embedding

如果检测到 AI 项目：
- 项目类型: AI/LLM Framework / AI Application
- 特殊关注: 
  - API Key 管理（环境变量 vs 配置文件）
  - 异步处理（async/await）
  - 模型配置（多模型支持）
  - 成本控制（token 计数）
- 推荐 Skills: ai-model-integration, api-key-management, async-patterns
```

### 安全信号源

| 信号源 | 扫描什么 | 推断出什么 |
|--------|----------|-----------|
| .env.example / .env.sample | 环境变量清单 | 密钥管理约定 |
| SECURITY.md / security.txt | 安全策略文档 | 安全报告流程 |
| .gitignore 中的敏感文件 | 是否忽略 .env, *.pem, *.key | 密钥保护意识 |
| Content-Security-Policy 头 | HTTP 安全头配置 | 安全基线 |
| CSRF 保护中间件 | CSRF token 验证 | 认证安全策略 |
| Dependabot / Renovate 配置 | 依赖安全更新 | 依赖安全 |
| **Trivy** | `trivy-scan.yml` 或 `.trivyignore` | 容器安全扫描 |
| **CodeQL** | `codeql.yml` | 代码安全扫描 |
| **gosec** | `.golangci.yml` 中的 gosec 配置 | Go 安全检查 |
| **Snyk** | `.snyk` | Snyk 安全配置 |
| **goreleaser** | `.goreleaser.yml` | Go 发布工具 |

### Lint 工具检测（v3.2 新增）

| 语言 | Lint 工具 | 配置文件 | 检测标志 |
|------|-----------|---------|---------|
| Python | **ruff** | `ruff.toml`, `pyproject.toml` | ruff check/format |
| Python | **mypy** | `mypy.ini`, `pyproject.toml` | mypy check |
| Python | **ty** | `pyproject.toml` | ty check (新类型检查器) |
| Python | **pylint** | `.pylintrc` | pylint |
| Python | **black** | `pyproject.toml` | black format |
| Python | **isort** | `pyproject.toml` | isort |
| Python | **typos** | `pyproject.toml` | typos (拼写检查) |
| Go | **golangci-lint v2** | `.golangci.yml` | version: "2" |
| Go | **gosec** | `.golangci.yml` | gosec 配置 |
| Go | **revive** | `.golangci.yml` | revive 配置 |
| Go | **staticcheck** | `.golangci.yml` | staticcheck 配置 |
| TypeScript | **ESLint** | `.eslintrc.*` | eslint |
| TypeScript | **Prettier** | `.prettierrc` | prettier |
| TypeScript | **ast-grep** | `ast-grep` 依赖 | ast-grep scan |
| TypeScript | **alex** | `package.json` | alex (包容性语言检查) |

### Python 包管理器检测（v3.2 新增）

| 包管理器 | 检测标志 | 配置文件 | 锁文件 |
|---------|---------|---------|--------|
| **PDM** | `pdm.lock` 或 `pyproject.toml` + `[tool.pdm]` | `pyproject.toml` | `pdm.lock` |
| uv | `uv.lock` | `pyproject.toml` | `uv.lock` |
| Poetry | `poetry.lock` | `pyproject.toml` | `poetry.lock` |
| pip-tools | `requirements.txt` + hash | `setup.py` | `requirements.txt` |
| pip | `requirements.txt` | 无专用配置 | `requirements.txt` |

### Monorepo 工具检测（v3.2 新增）

| 工具 | 检测标志 | 配置文件 | 用途 |
|------|---------|---------|------|
| **pnpm workspace** | `pnpm-workspace.yaml` | `pnpm-workspace.yaml` | 多包管理 |
| **Lerna** | `lerna.json` | `lerna.json` | 多包版本管理 |
| **Turborepo** | `turbo.json` | `turbo.json` | 构建缓存/任务编排 |
| **Nx** | `nx.json` | `nx.json` | 构建缓存/任务编排 |
| **Bazel** | `WORKSPACE` | `WORKSPACE` | 大规模构建 |
| **uv workspace** | `libs/*/pyproject.toml` | `pyproject.toml` | Python 多包 |
| **PDM workspace** | `pyproject.toml` + `[tool.pdm]` | `pyproject.toml` | Python 多包 |

### Go 工具链检测（v3.2 新增）

| 工具 | 检测标志 | 配置文件 | 用途 |
|------|---------|---------|------|
| **golangci-lint v2** | `version: "2"` | `.golangci.yml` | Lint 检查 |
| **Trivy** | `trivy-scan.yml` | CI workflow | 容器安全扫描 |
| **CodeQL** | `codeql.yml` | CI workflow | 代码安全扫描 |
| **goreleaser** | `.goreleaser.yml` | `.goreleaser.yml` | 发布工具 |
| **Make** | `Makefile` | `Makefile` | 构建脚本 |
| **go test** | `go test` | 内置 | 测试 |
| **go vet** | `go vet` | 内置 | 静态分析 |

### 工作流信号源

| 信号源 | 扫描什么 | 推断出什么 |
|--------|----------|-----------|
| .github/workflows/pr-*.yml | PR 审查流程 | 代码审查要求 |
| scripts/*.sh / scripts/*.js | 自动化脚本 | CI/CD 自动化程度 |
| .husky/ / husky.config.js | Git hooks 配置 | 提交前检查流程 |
| .gitlab-ci.yml | GitLab CI 配置 | 分支策略、部署流程 |
| CHANGELOG.md | 版本发布记录 | 发布流程、版本策略 |
| CODEOWNERS | 代码审查责任 | 审查流程 |
| .changes/ | 变更管理 | 版本发布流程 |

### 代码模式信号源

| 信号源 | 扫描什么 | 推断出什么 |
|--------|----------|-----------|
| **导入模式分析** | 分析 import 语句 | 是否使用 `import type`、路径别名 |
| **错误处理模式** | 分析 try-catch / Result 类型 | 统一错误处理策略 |
| **异步模式** | 分析 Promise / async-await | 异步编程风格 |
| **依赖注入模式** | 分析构造函数 / 容器 | IoC 策略 |
| **目录结构模式** | 分析 src/ 下的子目录 | 架构分层、模块组织 |

## 扫描流程

### 扫描进度反馈（v3.5 新增）

在扫描项目时，实时显示进度反馈，让用户了解当前状态：

#### 进度反馈格式

```
[1/5] 扫描项目结构...
[2/5] 识别语言和框架...
[3/5] 分析配置文件...
[4/5] 检测工具链...
[5/5] 生成特征报告...
```

#### 进度阶段详解

| 阶段 | 名称 | 描述 | 预计时间 |
|------|------|------|----------|
| 1 | 扫描项目结构 | 分析目录结构和文件分布，统计源文件数量 | 1-5 秒 |
| 2 | 识别语言和框架 | 检测编程语言、框架、包管理器 | 2-10 秒 |
| 3 | 分析配置文件 | 解析项目配置和依赖关系 | 2-5 秒 |
| 4 | 检测工具链 | 识别 Lint、测试、CI 工具 | 2-5 秒 |
| 5 | 生成特征报告 | 汇总分析结果，生成推断报告 | 1-3 秒 |

#### 进度反馈实现

```
扫描项目: /path/to/project

[1/5] 扫描项目结构...
  ✓ 目录结构已分析
  ✓ 源文件统计: 1121 个文件
  ✓ 项目规模: 中等

[2/5] 识别语言和框架...
  ✓ 主语言: Python (置信度: 高)
  ✓ 框架: FastAPI (置信度: 高)
  ✓ 包管理器: PDM (置信度: 高)

[3/5] 分析配置文件...
  ✓ pyproject.toml 已解析
  ✓ ruff.toml 已解析
  ✓ pytest.ini 已解析

[4/5] 检测工具链...
  ✓ Lint: ruff
  ✓ 测试: pytest
  ✓ CI: GitHub Actions
  ✓ 类型检查: mypy

[5/5] 生成特征报告...
  ✓ 项目模式: API-Driven Development
  ✓ 置信度: 高
  ✓ 推断质量自评完成

✅ 扫描完成！耗时: 12 秒
```

#### 大型项目进度反馈

对于大型项目（>1000 文件），显示更详细的进度：

```
扫描项目: /path/to/large-project
项目规模: 大型 (12456 文件)

[1/5] 扫描项目结构... (预计 15-30 秒)
  ⏳ 正在扫描目录结构...
  ⏳ 正在统计源文件...
  ✓ 目录结构已分析 (245 个目录)
  ✓ 源文件统计: 12456 个文件
  ✓ 语言分布: Python 65%, TypeScript 30%, Shell 5%

[2/5] 识别语言和框架... (预计 10-20 秒)
  ✓ 主语言: Python (置信度: 高)
  ✓ 框架: FastAPI (置信度: 高)
  ✓ 前端: Next.js (置信度: 高)
  ✓ Monorepo: 是 (pnpm workspace)

... (后续阶段类似)
```

### Step 0：语言和复杂度检测（v2.4 增强版）

```
1. 语言检测：
   - 扫描项目根目录和一级子目录
   - 统计各语言源文件数量
   - 检测子目录中的语言标志（python/, R/, src/）
   - 识别主语言和辅助语言
   - 特殊处理：PHP 项目优先 composer.json

2. 构建系统检测：
   - 检查是否存在配置文件
   - 识别构建工具
   - 检测 Monorepo 结构
   - 识别框架复杂度

3. 复杂度评估：
   - 计算复杂度分数（包含框架复杂度）
   - 确定置信度阈值
   - 选择扫描策略

4. 输出检测结果：
   - 语言列表（主语言 + 辅助语言）
   - 构建系统类型
   - 复杂度等级
   - 扫描策略建议
```

### Step 1：项目结构扫描

```
扫描目标项目根目录，识别：
- 语言/框架：从 package.json、pyproject.toml、go.mod 等推断
- 项目类型：Web API / 前端 / 全栈 / CLI / 数据管线 / 基础设施
- 构建工具：Webpack / Vite / esbuild / tsc / Cargo / go build
- 测试框架：Jest / Pytest / Go test / Mocha / Vitest / PHPUnit
- CI 平台：GitHub Actions / GitLab CI / Jenkins
- 包管理器：npm / pnpm / yarn / bun / Cargo / go modules / Composer / Maven
- 是否 Monorepo：检测 packages/、libs/、crates/ 等目录
```

### Step 2：配置文件分析

读取并分析关键配置文件：

```
# Node.js 项目示例
- package.json: 依赖版本、scripts 命令
- tsconfig.json: strict 模式、target、module
- eslint.config.js: 规则集、extends
- jest.config.ts: 测试环境、覆盖率阈值

# Python 项目示例（v2.4 新增）
- pyproject.toml: 依赖、工具配置
- setup.py: 安装配置
- pytest.ini: 测试配置
- mypy.ini: 类型检查配置

# Go 项目示例
- go.mod: 依赖版本
- .golangci.yml: lint 规则

# Rust 项目示例
- Cargo.toml: 依赖版本、workspace 配置
- rustfmt.toml: 格式化规则
- clippy.toml: lint 规则

# PHP 项目示例（v2.4 新增）
- composer.json: 依赖版本
- phpunit.xml: 测试配置
- vite.config.js: 前端构建

# C/C++ 项目示例
- CMakeLists.txt: 构建配置、依赖
- Makefile: 构建规则

# Java 项目示例（v2.4 新增）
- build.gradle: Gradle 配置
- pom.xml: Maven 配置

# 测试配置深度分析
- vitest.config.ts: E2E 测试配置
- playwright.config.ts: 浏览器测试配置
```

### Step 3：代码模式深度分析

分析代码中的模式和约定：

```
# 目录结构模式
- src/ 下是否有 routes/、controllers/、handlers/ → API 组织方式
- 是否有 services/、usecases/、business/ → 业务逻辑分层
- 是否有 models/、entities/、schemas/ → 数据模型定义
- 是否有 tests/、spec/、__tests__/ → 测试组织

# 命名约定
- 文件命名：snake_case / kebab-case / PascalCase
- 函数命名：camelCase / snake_case
- 导出方式：default export / named export / index barrel

# API 风格
- REST 路径：/users/:id vs /users/{id}
- 响应格式：{ data, error } vs { result, message }
- 状态码使用

# 错误处理
- 是否有统一的 Error 类
- 错误日志格式

# 导入模式分析
- 是否使用 `import type { ... }` 分离类型导入
- 是否使用路径别名（如 @/、~/）
- 是否有统一的 barrel 文件（index.ts）

# 异步模式分析
- Promise 链式调用 vs async/await
- 是否使用 `.catch()` 或 try-catch
- 是否有统一的错误处理中间件

# 测试模式分析（分析 3-5 个测试文件）
- 测试文件命名：*.test.ts / *.spec.ts / test-*.ts
- Mock 策略：手动 mock vs library mock
- 测试组织：describe/it vs test
```

### Step 4：安全信号源扫描

```
# 密钥管理
- 检查 .env.example 是否存在
- 检查 .gitignore 是否包含 .env
- 检查是否有硬编码的密钥（grep API_KEY, SECRET, TOKEN）

# 安全头
- 检查是否有 CSP 配置
- 检查是否有 security headers 中间件
- 检查是否禁用了 HTTP（仅 HTTPS）

# 认证安全
- 检查 CSRF 保护
- 检查 CORS 配置
- 检查认证 token 存储方式（httpOnly cookie vs localStorage）

# 依赖安全
- 检查是否有 Dependabot 配置
- 检查是否有安全漏洞扫描 CI
```

### Step 5：Git 历史深度分析

```
# Commit message 格式
- git log --oneline -50 → 分析格式
- 是否遵循 Conventional Commits
- 分支策略：main/develop/feature/* 还是 trunk-based

# 代码变更模式
- 大文件是否经常被修改
- 是否有死代码
- 依赖更新频率

# PR 审查流程推断
- 分析 .github/workflows/ 中的 PR 相关 workflow
- 检查是否有 CODEOWNERS 文件
- 检查是否需要特定数量的 review

# 发布流程推断
- 分析 release 相关脚本
- 检查 CHANGELOG.md 格式
- 检查版本号策略（semver）
```

### Step 6：生成推断 Constitution

基于扫描结果，生成推断出的 Constitution：

```markdown
# 推断 Constitution — project-name（基于代码考古）

> 生成时间: YYYY-MM-DD
> 信号源: package.json, tsconfig.json, jest.config.ts, git log
> 项目复杂度: [极低/低/中/高/极高]
> 置信度: [高/中/低]（基于复杂度调整阈值）

## 语言和构建
- 主语言: [TypeScript/Go/Rust/Python/PHP/Java/...]
- 辅助语言: [Rust/Go/JavaScript/...]（如有）
- 构建系统: [Vite/Turborepo/Cargo/Gradle/...]
- 包管理器: [pnpm/npm/Cargo/Composer/Maven/...]
- 来源: 语言检测矩阵

## 技术栈（置信度: 高）
- [框架/库版本]
- [数据库/缓存]
- [其他依赖]
- 来源: package.json/go.mod/Cargo.toml/composer.json

## 架构决策（置信度: 高/中）
- [架构模式]
- [分层结构]
- [模块组织]
- 来源: 目录结构分析

## 质量门禁（置信度: 高）
- [Lint 配置]
- [类型检查]
- [测试覆盖率]
- CI 跑 [lint + type-check + test]
- 来源: 配置文件分析

## 测试策略（置信度: 中）
- 测试框架: [Vitest/Jest/go test/PHPUnit/pytest/...]
- E2E 测试: [Playwright/Cypress/...]
- 覆盖率要求: [?] 未在配置中找到明确阈值
- Mock 策略: [?] 需要分析测试文件确认
- 来源: 测试配置分析

## 安全规范（置信度: [?]）
- 密钥管理: [?] 需要检查 .env.example
- 安全头: [?] 需要检查 middleware
- CSRF 保护: [?] 需要检查 auth 代码
- 来源: 安全信号源扫描

## 工作流（置信度: 中）
- Conventional commits（从 git log 推断）
- [Feature branch/Trunk-based]
- [PR 审查要求]
- 来源: git log + CI workflow 分析

## 代码模式（置信度: 中）
- 导入风格: [?] 需要分析代码确认
- 错误处理: [?] try-catch / Result 类型
- 异步模式: [?] Promise / async-await
- 来源: 代码模式分析

## 不确定项

[?] [问题描述]
- [发现的线索]
- 需要人工确认

[!] [矛盾/警告]
- [详细说明]
- 需要人工决策
```

## 人工确认流程（v2.12 增强版）

将推断出的 Constitution 展示给用户：

### 闸门确认指引（v2.12 新增）

根据推断结果的风险等级，采用不同的确认策略：

```
### 高风险项（必须用户显式确认）
- [ ] 架构分层假设（置信度 < 0.7）
- [ ] 安全规范推断（置信度 < 0.8）
- [ ] API 设计模式（置信度 < 0.7）
- [ ] 数据库选择（置信度 < 0.8）

### 中风险项（建议用户确认）
- [ ] 错误处理策略
- [ ] 测试策略
- [ ] 代码风格规范
- [ ] 依赖管理策略

### 低风险项（可自动继续）
- [x] 语言检测（置信度 >= 0.9）
- [x] 技术栈识别（置信度 >= 0.9）
- [x] 构建系统（置信度 >= 0.9）
- [x] 包管理器（置信度 >= 0.9）
```

### 推断质量自评（v2.12 新增）

在输出末尾增加质量自评：

```markdown
## 推断质量自评

| 维度 | 评分 | 说明 |
|------|------|------|
| 语言识别 | 0.95 | [具体说明] |
| 技术栈识别 | 0.90 | [具体说明] |
| 架构推断 | 0.80 | [具体说明] |
| 质量门禁 | 0.85 | [具体说明] |
| 测试策略 | 0.75 | [具体说明] |
| 信号源完整性 | 0.90 | [具体说明] |
| **综合** | **0.85** | [总评说明] |

### 信号源覆盖情况
- ✅ 已扫描: package.json, tsconfig.json, CONTRIBUTING.md, CI workflow
- ⚠️ 部分扫描: pytest.ini (存在但未深入分析)
- ❌ 未扫描: 无
```

### 标准确认流程

1. **确认正确的推断** -> 固化为 Constitution 条目
2. **纠正错误的推断** -> 更新为正确值
3. **确认不确定项** -> 补全或标记为 TBD
4. **确认矛盾项** -> 决策采用哪种方案
5. **补充缺失项** -> 添加推断遗漏的内容

## 输出：定制化 Harness Engineering 系统（v3.0 重构）

### 输出目录结构

```
.harness/
├── constitution.md           # 项目特征摘要（简要）
├── scripts/                  # 定制脚本
│   ├── check-*.py           # 检查脚本
│   ├── validate-*.py        # 验证脚本
│   └── generate-*.py        # 生成脚本
├── hooks/                    # 自动化 Hook
│   ├── pre-commit            # 提交前检查
│   ├── pre-push              # 推送前检查
│   └── post-merge            # 合并后操作
├── skills/                   # 定制 Skill
│   ├── SKILL.md              # Skill 定义
│   └── references/           # Skill 参考文档
└── workflows/                # 定制工作流
    ├── api-change.md         # API 变更流程
    ├── feature-dev.md        # 功能开发流程
    └── bugfix.md             # Bug 修复流程
```

### 定制脚本生成规则

根据识别的项目模式，生成对应的定制脚本：

#### API-Driven 项目

```
识别特征：
├── 前端 API 客户端 (src/api/*.ts)
├── 后端 API 端点 (app/api/**/*.py)
├── API 类型定义 (src/types/api.ts)
└── 前端修改需要注册 API

生成脚本：
├── scripts/check-api-sync.py
│   ├── 检查前后端 API 类型同步
│   ├── 检查 API 文档是否需要更新
│   └── 检查端点是否存在
├── scripts/validate-api-types.py
│   ├── 验证前端类型与后端响应一致
│   └── 检查必填/可选字段
└── scripts/generate-api-docs.py
    ├── 自动从代码生成 API 文档
    └── 更新 OpenAPI/Swagger
```

#### 数据库驱动项目

```
识别特征：
├── ORM 模型 (models/*.py)
├── 迁移文件 (migrations/*.py)
├── 数据库配置 (config/database.toml)
└── 有数据库依赖

生成脚本：
├── scripts/check-migration.py
│   ├── 检查迁移文件完整性
│   ├── 验证迁移可回滚
│   └── 检查数据兼容性
├── scripts/validate-models.py
│   ├── 验证模型字段一致性
│   └── 检查索引完整性
└── scripts/generate-seed.py
    └── 生成测试数据
```

#### 微服务项目

```
识别特征：
├── 多个服务目录 (services/*/)
├── 服务间通信 (rpc/, grpc/, messages/)
├── 服务配置 (configs/*.yaml)
└── 服务发现配置

生成脚本：
├── scripts/check-service-deps.py
│   ├── 检查服务依赖完整性
│   ├── 验证服务间契约
│   └── 检查配置一致性
├── scripts/validate-protocol.py
│   ├── 验证 RPC 协议兼容性
│   └── 检查消息格式
└── scripts/generate-service-doc.py
    └── 生成服务架构文档
```

#### AI/LLM 项目

```
识别特征：
├── LLM 调用 (agent/, llm/, chat/)
├── API Key 配置 (.env, config/)
├── 模型配置 (models.yaml)
└── 异步处理 (async/await)

生成脚本：
├── scripts/check-api-keys.py
│   ├── 检查 API Key 是否配置
│   ├── 验证 Key 环境变量
│   └── 检测硬编码 Key
├── scripts/validate-llm-config.py
│   ├── 验证模型配置完整性
│   └── 检查成本估算
└── scripts/estimate-tokens.py
    └── 估算 API 调用成本
```

### 定制 Hook 配置

根据项目特征，配置自动化 Hook：

```yaml
# .harness/hooks/pre-commit
---
description: 提交前自动检查
version: 1.0.0
---

# 触发条件
trigger:
  # 根据项目特征配置
  files_changed:
    - "src/api/**/*"      # API-Driven 项目
    - "app/api/**/*"
    - "src/types/**/*"
    - "models/**/*"       # 数据库项目
    - "migrations/**/*"

# 检查命令
checks:
  # 语言特定检查
  - name: lint
    command: "{{lint_command}}"
    on_failure: warn
    
  - name: type_check
    command: "{{type_check_command}}"
    on_failure: warn
    
  # 项目特定检查（定制脚本）
  - name: api_sync
    command: "python .harness/scripts/check-api-sync.py --changed"
    on_failure: block
    condition: "has_api_changes"
    
  - name: migration_check
    command: "python .harness/scripts/check-migration.py"
    on_failure: block
    condition: "has_migration_changes"

# 输出格式
output:
  format: markdown
  show_details: true
```

```yaml
# .harness/hooks/pre-push
---
description: 推送前检查
version: 1.0.0
---

trigger:
  event: git push

checks:
  - name: all_tests_pass
    command: "{{test_command}}"
    on_failure: block
    
  - name: api_docs_updated
    command: "python .harness/scripts/generate-api-docs.py --check"
    on_failure: warn
    condition: "has_api_changes"
```

### 定制 Skill 生成

为项目生成定制 Skill：

```markdown
---
name: api-sync-check
description: |
  API 同步检查 Skill。
  自动检查前后端 API 是否同步，包括类型定义、端点存在性、文档更新。
  触发条件：修改 API 相关文件时自动触发。
version: 1.0.0
tags: [api, sync, check, typescript, python]
---

# API Sync Check

## 触发条件

- 修改 `src/api/**/*.ts`
- 修改 `src/types/api.ts`
- 修改 `app/api/**/*.py`

## 执行步骤

1. **类型同步检查**
   ```bash
   python .harness/scripts/check-api-sync.py --type-sync
   ```

2. **端点存在性检查**
   ```bash
   python .harness/scripts/check-api-sync.py --endpoint-sync
   ```

3. **文档更新检查**
   ```bash
   python .harness/scripts/check-api-sync.py --doc-sync
   ```

## 输出

```json
{
  "status": "error|warn|ok",
  "checks": {
    "type_sync": {"status": "ok", "issues": []},
    "endpoint_sync": {"status": "error", "issues": ["GET /api/users not found"]},
    "doc_sync": {"status": "warn", "issues": ["POST /api/login missing doc"]}
  },
  "recommendations": [
    "Create endpoint GET /api/users in app/api/users.py",
    "Add docstring to POST /api/login"
  ]
}
```

## 修复建议

根据检查结果，自动生成修复建议：

1. 如果端点不存在 → 提供创建端点的代码模板
2. 如果类型不一致 → 提供类型修正建议
3. 如果文档缺失 → 提供文档模板
```

### 定制工作流生成

根据项目模式生成定制工作流：

```markdown
---
name: api-change-workflow
description: API 变更完整流程
version: 1.0.0
---

# API 变更工作流

## 适用场景

- 新增 API 端点
- 修改 API 响应格式
- 废弃 API 端点

## 流程步骤

### Step 1: 变更分析

1. 识别变更类型（新增/修改/废弃）
2. 分析影响范围（前端组件、其他服务）
3. 检查是否需要数据迁移

### Step 2: 后端实现

1. 更新 API 端点
2. 更新响应类型定义
3. 添加 API 文档
4. 编写单元测试

### Step 3: 前端适配

1. 更新 API 客户端
2. 更新类型定义
3. 更新使用该 API 的组件

### Step 4: 验证

```bash
# 运行 API 同步检查
python .harness/scripts/check-api-sync.py --all

# 运行测试
{{test_command}}

# 生成 API 文档
python .harness/scripts/generate-api-docs.py
```

### Step 5: 提交

```bash
# 提交前检查
git add .
# pre-commit hook 会自动运行检查

git commit -m "feat(api): add new endpoint POST /api/users"
```

## 检查清单

- [ ] API 端点已实现
- [ ] 响应类型已更新
- [ ] 前端客户端已适配
- [ ] API 文档已更新
- [ ] 测试已通过
- [ ] API 同步检查通过
```

### Constitution（简化版）

Constitution 不再是详细的规范文档，而是项目特征摘要：

```markdown
# Constitution — my-api

## 项目特征

| 特征 | 值 | 置信度 |
|------|-----|--------|
| 语言 | Python 3.11+ | 0.95 |
| 框架 | FastAPI | 0.95 |
| 前端 | TypeScript + React | 0.90 |
| 测试 | pytest | 0.95 |
| Lint | ruff | 0.95 |

## 项目模式

**API-Driven Development**
- 前后端分离
- 通过 REST API 通信
- 前端修改需要注册对应 API

## 定制化输出

| 类型 | 文件 | 说明 |
|------|------|------|
| 脚本 | check-api-sync.py | 检查前后端 API 同步 |
| 脚本 | validate-api-types.py | 验证 API 类型一致性 |
| Hook | pre-commit | 提交前自动检查 |
| Skill | api-sync-check | API 同步检查 Skill |
| 工作流 | api-change.md | API 变更流程 |

## 质量门禁

| 检查项 | 命令 | 阈值 |
|--------|------|------|
| Lint | ruff check . | 无错误 |
| 类型检查 | mypy app/ | 无错误 |
| 测试 | pytest tests/ | 100% 通过 |
| API 同步 | check-api-sync.py | 无错误 |
```

## 注意事项

1. **定制化是核心**：每个项目生成不同的脚本、Hook、Skill
2. **脚本要可执行**：不是模板，是针对该项目的定制代码
3. **Hook 要自动触发**：不需要用户手动运行
4. **Skill 要封装逻辑**：不是文档推荐，是可调用的 Skill
5. **工作流要完整**：从分析到提交的完整流程
6. **质量自评**：检查生成的系统是否完整

## 版本历史

- v4.0.0 (2026-06-18): 重大重构
  - 新增 intent + surfaces + risk 分类模型
  - 新增生成深度决策逻辑（light/medium/full）
  - 新增特殊流程检测（protocol-chain 等）
  - 输出格式更新，支持 harness-designer 决策
- v3.5.1 (2026-06-16): 添加历史记录保存功能
- v3.5.0 (2026-06-16): 增加扫描进度反馈
- v3.4.0 (2026-06-16): 增加确认环节
- v3.3.0 (2026-06-16): 融合 SDD 工具最佳实践
- v3.2.0 (2026-06-16): 增强语言和构建系统检测
- v3.1.0 (2026-06-16): 增强 AI 项目细分
- v3.0.0 (2026-06-15): 重构为定制化系统输出
