---
name: todo-app-dev-workflow
description: |
  TODO App 项目开发流程 Skill
  
  基于项目特征自动生成：TypeScript + Next.js + Web App
  
  触发方式：
  - "/todo" 或 "todo workflow"
version: 1.0.0
auto_generated: true
generated_by: your-harness
generated_at: 2026-06-22
---

# TODO App 开发流程

## 项目概述

- **项目类型**: Web App
- **技术栈**: TypeScript + Next.js
- **核心特征**: 前端应用、用户认证、数据持久化

## 意图分类模型

### 开发意图 (Intents)

```yaml
intents:
  - name: new-feature
    description: "新功能开发"
    typical_surfaces: [frontend-ui, backend-logic, testing]
    
  - name: incremental-change
    description: "现有功能增强"
    typical_surfaces: [frontend-ui, testing]
    
  - name: bugfix
    description: "Bug 修复"
    typical_surfaces: [testing, documentation]
    
  - name: refactor
    description: "代码重构"
    typical_surfaces: [backend-logic, testing]
```

### 影响面 (Surfaces)

```yaml
surfaces:
  - name: frontend-ui
    description: "前端界面"
    requires: [component-test]
    
  - name: backend-logic
    description: "后端逻辑"
    requires: [api-test]
    
  - name: database
    description: "数据存储"
    requires: [migration, data-test]
    
  - name: authentication
    description: "用户认证"
    requires: [security-test]
    
  - name: testing
    description: "测试覆盖"
    requires: [unit-test, integration-test]
    
  - name: documentation
    description: "文档"
    requires: [api-doc]
```

## 开发流程

### 阶段 1: 需求定义 (G0)

**Input**: 用户需求描述

**Process**:
1. 调用 superpowers/idea-refine
2. 提取功能点
3. 确定验收标准

**Output**:
- `docs/spec/<feature>/analyze/01-requirements.md`

**Gate**:
- [ ] 需求理解确认
- [ ] 验收标准可测

---

### 阶段 2: 任务拆解 (G1)

**Input**: 需求文档

**Process**:
1. 调用 superpowers/planning-and-task-breakdown
2. 分解任务
3. 评估工作量

**Output**:
- `docs/spec/<feature>/design/01-tasks.md`

**Gate**:
- [ ] 任务粒度 ≤ 1 天
- [ ] 依赖关系明确
- [ ] 优先级确定

---

### 阶段 3: 设计 (G2)

**Input**: 任务列表

**Process**:
1. 前端设计
   - 组件结构
   - API 接口定义
   
2. 后端设计
   - 数据模型
   - API 实现方案

**Output**:
- `docs/spec/<feature>/design/02-frontend-design.md`
- `docs/spec/<feature>/design/03-backend-design.md`

**Gate**:
- [ ] 设计评审通过
- [ ] API 契约确定

---

### 阶段 4: 实现 (G3)

**Input**: 设计文档

**Process**:
1. 调用 superpowers/test-driven-development
2. 按 TDD 方式实现
3. 代码审查

**Output**:
- 源代码
- 测试代码
- `docs/spec/<feature>/develop/3X-<slice>.md`

**Gate**:
- [ ] 测试全绿
- [ ] 代码审查通过
- [ ] 无 P0/P1 问题

---

### 阶段 5: 验证 (G4)

**Input**: 实现代码

**Process**:
1. 运行测试套件
2. 检查覆盖率
3. 性能测试

**Output**:
- 测试报告
- 覆盖率报告

**Gate**:
- [ ] 覆盖率 ≥ 80%
- [ ] 无性能问题

---

### 阶段 6: 上线 (G5)

**Input**: 验证通过的代码

**Process**:
1. 调用 superpowers/deployment
2. 灰度发布
3. 监控告警

**Output**:
- 部署配置
- 监控配置

**Gate**:
- [ ] 回滚预案就绪
- [ ] 监控告警配置
- [ ] 文档更新完成

---

## 闸门状态

```yaml
gates:
  G0: null  # 需求澄清后
  G1: null  # 任务拆解后
  G2: null  # 设计完成后
  G3: null  # 代码实现后
  G4: null  # 验证完成后
  G5: null  # 上线前
```

---

## 引用的 Superpowers Skills

本流程会调用以下 Superpowers Skills：

- superpowers/idea-refine
- superpowers/planning-and-task-breakdown
- superpowers/test-driven-development
- superpowers/code-review
- superpowers/debugging
- superpowers/deployment
