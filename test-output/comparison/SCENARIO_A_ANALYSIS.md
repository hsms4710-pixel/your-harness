# 场景 A 分析：对已有项目使用 Your Harness

## 测试目标

LangChain 项目（AI/LLM 开发框架）

## Your Harness 生成的产物

### 1. SKILL.md（主流程）

```yaml
# 自动生成的意图分类
intents:
  - new-integration      # 新的 LLM/工具集成
  - new-feature          # 框架新功能
  - new-prompt-template  # 新的 Prompt 模板
  - new-rag-component    # 新的 RAG 组件
  - incremental-change   # 现有功能增强
  - bugfix               # Bug 修复
  - compatibility-update # 兼容性更新

# 自动生成的影响面
surfaces:
  - model-integration    # LLM 模型集成
  - prompt-management    # Prompt 模板管理
  - api-contract         # 公共 API 契约
  - data-model           # 数据结构定义
  - documentation        # 文档和示例
  - testing              # 测试覆盖
```

### 2. TEMPLATE-llm-integration.md（LLM 集成模板）

```markdown
## 阶段 1: 需求分析
### 1.1 明确集成目标
- 确定要集成的模型（OpenAI/Claude/本地模型）
- 分析模型的 API 特性
- 评估性能要求
- 确定成本影响

## 阶段 2: 设计
### 2.1 模型抽象设计
- 遵循 LangChain 的 BaseChatModel 抽象
- 实现 _generate 和 _stream 方法
- 使用 ChatResult 和 ChatGenerationChunk

## 阶段 3: 实现
### 3.1 基础实现
### 3.2 测试
### 3.3 文档

## 阶段 4: 验证
### 4.1 兼容性测试
### 4.2 性能测试
### 4.3 成本估算
```

## 与直接使用 Superpowers 的区别

### 区别 1：流程编排 vs 单点调用

| 维度 | 使用 Your Harness | 直接使用 Superpowers |
|------|------------------|---------------------|
| **流程定义** | ✅ 自动生成 `new-integration → model-integration → testing` 流程 | ❌ 无流程定义 |
| **Skill 调用顺序** | ✅ 按模板定义的阶段调用 | ❌ 用户自己决定何时调用 |
| **检查点** | ✅ 每个阶段有闸门检查 | ❌ 无强制检查 |

### 区别 2：项目特定 vs 通用

| 维度 | 使用 Your Harness | 直接使用 Superpowers |
|------|------------------|---------------------|
| **项目理解** | ✅ 识别 LangChain 是 LLM 框架 | ❌ 不知道项目类型 |
| **特定流程** | ✅ 生成 `TEMPLATE-llm-integration.md` | ❌ 无特定模板 |
| **特定检查** | ✅ 检查 API Key 管理、成本估算 | ❌ 通用检查 |

### 区别 3：知识沉淀 vs 无沉淀

| 维度 | 使用 Your Harness | 直接使用 Superpowers |
|------|------------------|---------------------|
| **文档落盘** | ✅ 每个阶段产出文档 | ❌ 无强制文档 |
| **历史追溯** | ✅ 有完整的开发历史 | ❌ 无历史记录 |
| **团队协作** | ✅ 新成员可按流程开发 | ❌ 需要学习他人经验 |

## 具体示例：新增 OpenAI GPT-5 集成

### 使用 Your Harness 的流程

```
1. 用户: "我要新增 OpenAI GPT-5 集成"

2. AI 自动:
   - 识别意图: new-integration
   - 识别影响面: [model-integration, api-contract, testing]
   - 加载模板: TEMPLATE-llm-integration.md
   - 展示流程:
     
   阶段 1: 需求分析
     - 1.1 明确集成目标
       Run: 分析 OpenAI GPT-5 API 特性
       Expected: 输出 API 分析文档
     
   3. 用户按流程执行，每个阶段有明确的输入/输出
   
   4. 通过闸门检查后进入下一阶段
   
   5. 最终产出:
      - docs/spec/gpt5/analyze/01-api-analysis.md
      - docs/spec/gpt5/design/01-architecture.md
      - src/langchain/models/gpt5.py
      - tests/models/test_gpt5.py
      - docs/integrations/gpt5.md
```

### 直接使用 Superpowers 的流程

```
1. 用户: "帮我集成 OpenAI GPT-5"

2. AI 可能调用:
   - superpowers/test-driven-development
   - superpowers/code-review
   
3. 问题:
   - 不知道先分析 API 再设计
   - 不知道需要考虑成本估算
   - 不知道需要更新文档
   - 没有强制的检查点
   - 没有产出文档
```

## 结论

**Your Harness 适合**：
- 需要规范化开发流程的团队
- 需要知识沉淀和历史追溯的项目
- 复杂项目（多个模块、多人协作）

**直接使用 Superpowers 适合**：
- 简单任务
- 个人开发
- 快速原型

## 评分

| 维度 | Your Harness | Superpowers | 说明 |
|------|--------------|-------------|------|
| 流程规范性 | 95 | 40 | Your Harness 提供完整的流程定义 |
| 项目适配度 | 90 | 60 | Your Harness 根据项目特征定制 |
| 知识沉淀 | 95 | 20 | Your Harness 强制文档落盘 |
| 执行效率 | 70 | 90 | Superpowers 更灵活快速 |
| 学习成本 | 60 | 90 | Superpowers 更容易上手 |
| **综合** | **82** | **52** | 复杂项目 Your Harness 更优 |
