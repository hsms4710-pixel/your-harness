---
name: langchain-dev-workflow
description: |
  LangChain 项目开发流程 Skill
  
  基于项目特征自动生成，适用于 LangChain 这类 LLM 开发框架项目。
  
  核心能力：
  - LLM 集成开发流程
  - Prompt 管理
  - 模型兼容性测试
  - 文档同步
  
  触发方式：
  - "/lc" 或 "langchain workflow"
version: 1.0.0
auto_generated: true
generated_by: your-harness
---

# LangChain 开发流程

## 项目概述

- **项目类型**: LLM 开发框架 (Python)
- **核心特征**: 模块化设计、插件系统、多模型支持
- **开发重点**: 模型集成、Prompt 管理、RAG 支持

## 意图分类模型

### 开发意图 (Intents)

```yaml
intents:
  # 新功能开发
  - name: new-integration
    description: "新的 LLM/工具集成"
    typical_surfaces: [model-integration, api-contract]
    
  - name: new-feature
    description: "框架新功能"
    typical_surfaces: [api-contract, documentation]
    
  - name: new-prompt-template
    description: "新的 Prompt 模板"
    typical_surfaces: [prompt-management, testing]
    
  - name: new-rag-component
    description: "新的 RAG 组件"
    typical_surfaces: [data-model, api-contract]
    
  # 增量变更
  - name: incremental-change
    description: "现有功能增强"
    typical_surfaces: [api-contract, testing]
    
  # Bug 修复
  - name: bugfix
    description: "Bug 修复"
    typical_surfaces: [testing, documentation]
    
  # 兼容性更新
  - name: compatibility-update
    description: "模型/依赖兼容性更新"
    typical_surfaces: [model-integration, testing]
```

### 影响面 (Surfaces)

```yaml
surfaces:
  # 模型集成
  - name: model-integration
    description: "LLM 模型集成"
    requires: [api-key-management, cost-estimation]
    
  # Prompt 管理
  - name: prompt-management
    description: "Prompt 模板管理"
    requires: [version-control, testing]
    
  # API 契约
  - name: api-contract
    description: "公共 API 契约"
    requires: [documentation, backward-compatibility]
    
  # 数据模型
  - name: data-model
    description: "数据结构定义"
    requires: [type-checking, testing]
    
  # 文档
  - name: documentation
    description: "文档和示例"
    requires: [docs-build, examples]
    
  # 测试
  - name: testing
    description: "测试覆盖"
    requires: [unit-tests, integration-tests]
    
  # 向后兼容
  - name: backward-compatibility
    description: "向后兼容性"
    requires: [versioning, deprecation]
```

### 风险等级 (Risk)

```yaml
risks:
  - name: low
    description: "低风险变更"
    criteria: ["文档更新", "示例代码", "内部重构"]
    
  - name: medium
    description: "中风险变更"
    criteria: ["新功能", "API 增强", "依赖更新"]
    
  - name: high
    description: "高风险变更"
    criteria: ["API 破坏性变更", "模型集成", "核心逻辑修改"]
    
  - name: critical
    description: "关键变更"
    criteria: ["安全修复", "性能优化", "紧急热修复"]
```

## 开发流程

### 阶段 0: 需求澄清

**触发**: 开始任何开发任务前

**澄清问题**:
1. 这是什么类型的变更？（新集成/新功能/Bug 修复/兼容性更新）
2. 影响哪些模块？（langchain-core/langchain-text-splitters/...）
3. 是否涉及 API 变更？
4. 是否需要更新文档？
5. 是否影响现有用户？

**输出**:
- 需求描述文档
- 影响范围分析

**闸门**: G0_CLARIFICATION_REVIEW
- 必须产物: 需求描述
- 通过标准: 需求清晰、范围明确

### 阶段 1: 设计

**触发**: 需求澄清通过后

**设计内容**:
1. API 设计（如果是新功能）
2. 类/方法设计
3. 测试策略
4. 文档计划

**输出**:
- 设计文档
- API 规范

**闸门**: G1_DESIGN_REVIEW
- 必须产物: 设计文档
- 通过标准: API 设计合理、测试策略明确

### 阶段 2: 实现

**触发**: 设计通过后

**实现要求**:
1. 遵循项目代码规范
2. 添加类型注解
3. 编写单元测试
4. 更新文档注释

**检查点**:
- 代码通过 lint 检查
- 类型检查通过
- 单元测试通过

**闸门**: G2_IMPLEMENTATION_REVIEW
- 必须产物: 代码 + 测试
- 通过标准: 测试全绿、代码审查通过

### 阶段 3: 集成测试

**触发**: 实现完成后

**测试内容**:
1. 集成测试
2. 兼容性测试
3. 性能测试（如需要）

**输出**:
- 测试报告

**闸门**: G3_INTEGRATION_REVIEW
- 必须产物: 测试报告
- 通过标准: 所有测试通过、覆盖率达标

### 阶段 4: 文档更新

**触发**: 集成测试通过后

**文档内容**:
1. API 文档
2. 使用示例
3. 迁移指南（如果是破坏性变更）

**输出**:
- 更新的文档

**闸门**: G4_DOCUMENTATION_REVIEW
- 必须产物: 文档更新
- 通过标准: 文档完整、示例可运行

### 阶段 5: 发布准备

**触发**: 文档更新完成后

**准备内容**:
1. 版本号更新
2. CHANGELOG 更新
3. 发布说明

**闸门**: G5_RELEASE_APPROVAL
- 必须产物: 版本更新、CHANGELOG
- 通过标准: 版本号正确、CHANGELOG 完整

### 阶段 6: 发布

**触发**: 发布准备通过后

**发布流程**:
1. 推送到 PyPI
2. 更新文档站点
3. 发布 GitHub Release

**闸门**: G6_SHIP
- 必须产物: 发布
- 通过标准: 发布成功、无回滚

## 闸门协议

### 闸门定义

```yaml
gates:
  # G0: 需求澄清
  - id: G0_CLARIFICATION_REVIEW
    stage: 需求澄清
    required_artifacts:
      - 需求描述
      - 影响范围分析
    checks:
      - "需求描述清晰"
      - "影响范围明确"
      - "API 变更已识别"
    approval_required: true
    
  # G1: 设计审查
  - id: G1_DESIGN_REVIEW
    stage: 设计
    required_artifacts:
      - 设计文档
    checks:
      - "API 设计合理"
      - "测试策略明确"
      - "文档计划完整"
    approval_required: true
    
  # G2: 实现审查
  - id: G2_IMPLEMENTATION_REVIEW
    stage: 实现
    required_artifacts:
      - 代码
      - 单元测试
    checks:
      - "测试全绿"
      - "代码审查通过"
      - "类型检查通过"
    approval_required: true
    
  # G3: 集成测试
  - id: G3_INTEGRATION_REVIEW
    stage: 集成测试
    required_artifacts:
      - 集成测试
      - 测试报告
    checks:
      - "集成测试通过"
      - "兼容性测试通过"
      - "覆盖率达标"
    approval_required: true
    
  # G4: 文档审查
  - id: G4_DOCUMENTATION_REVIEW
    stage: 文档更新
    required_artifacts:
      - 文档更新
      - 示例代码
    checks:
      - "API 文档完整"
      - "示例可运行"
      - "迁移指南完整（如需要）"
    approval_required: true
    
  # G5: 发布审批
  - id: G5_RELEASE_APPROVAL
    stage: 发布准备
    required_artifacts:
      - 版本更新
      - CHANGELOG
    checks:
      - "版本号正确"
      - "CHANGELOG 完整"
      - "发布说明清晰"
    approval_required: true
    
  # G6: 上线
  - id: G6_SHIP
    stage: 发布
    required_artifacts:
      - 发布
    checks:
      - "PyPI 发布成功"
      - "文档站点更新"
      - "GitHub Release 创建"
    approval_required: true
```

## 宪法模型

### 核心原则

```yaml
constitution:
  # 类型注解必须
  - rule: type-hints-required
    description: "所有公开 API 必须有类型注解"
    severity: critical
    check: "mypy --strict"
    
  # 测试必须
  - rule: tests-required
    description: "所有新功能必须有测试"
    severity: critical
    check: "pytest tests/ -v"
    
  # 文档必须更新
  - rule: docs-updated
    description: "API 变更必须更新文档"
    severity: high
    check: "mkdocs build"
    
  # 向后兼容
  - rule: api-stable
    description: "公开 API 必须保持向后兼容"
    severity: critical
    check: "API 兼容性测试"
    
  # 代码风格
  - rule: code-style-consistent
    description: "代码风格必须一致"
    severity: medium
    check: "ruff check"
```

## 路由规则

### 意图到模板的映射

```yaml
routing:
  # 新模型集成
  - intent: new-integration
    template: TEMPLATE-llm-integration.md
    surfaces: [model-integration, api-contract]
    
  # 新功能
  - intent: new-feature
    template: TEMPLATE-feature.md
    surfaces: [api-contract, documentation]
    
  # Bug 修复
  - intent: bugfix
    template: TEMPLATE-bugfix.md
    surfaces: [testing, documentation]
    
  # 兼容性更新
  - intent: compatibility-update
    template: TEMPLATE-llm-integration.md
    surfaces: [model-integration, testing]
```

## 实用工具

### 常见借口 (Common Rationalizations)

```yaml
# 预防 AI 偷懒的借口
rationalizations:
  - excuse: "这个改动太小了，不需要测试"
    response: "所有改动都需要测试，即使是小改动"
    
  - excuse: "这只是一个内部函数，不需要文档"
    response: "内部函数也需要注释，方便维护"
    
  - excuse: "先提交，之后再补测试"
    response: "测试必须和代码一起提交，不能事后补"
    
  - excuse: "这个模型的 API 可能会变，先不写测试"
    response: "正是因为 API 可能变，更需要测试来检测变化"
```

### 反模式 (Red Flags)

```yaml
# 需要警惕的反模式
red_flags:
  - pattern: "硬编码 API Key"
    severity: critical
    action: "立即停止，使用环境变量"
    
  - pattern: "忽略类型错误"
    severity: high
    action: "修复类型错误，不要用 type: ignore"
    
  - pattern: "删除测试来通过 CI"
    severity: critical
    action: "修复测试失败的原因，不要删除测试"
    
  - pattern: "复制粘贴代码"
    severity: medium
    action: "提取公共方法，避免重复"
    
  - pattern: "不检查错误"
    severity: high
    action: "检查所有错误返回值"
```

## 交互协议

### 首次响应格式

```yaml
# AI 首次响应必须包含
first_response:
  - "识别开发意图"
  - "列出影响面"
  - "评估风险等级"
  - "确认需要的闸门数量"
  - "询问是否继续"
```

### 确认请求格式

```yaml
# 每个闸门需要的确认格式
confirmation_request:
  - "当前阶段: [阶段名称]"
  - "必须产物: [产物列表]"
  - "检查项: [检查项列表]"
  - "是否继续下一阶段？"
```

---

*此 Skill 由 Your Harness 自动生成*
*生成时间: 2026-06-18*
*基于项目特征: LLM 开发框架 + Python + Monorepo*
