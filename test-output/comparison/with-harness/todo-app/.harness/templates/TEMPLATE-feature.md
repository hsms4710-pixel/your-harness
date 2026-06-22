---
description: 功能开发模板
version: 1.0.0
auto_generated: true
generated_by: your-harness
---

# 功能开发模板

本模板用于 TODO App 的功能开发。

---

## 阶段 1: 需求分析

### 1.1 功能点提取

**Input**
- 用户需求描述

**Process**
1. 调用 superpowers/idea-refine
2. 提取核心功能点
3. 确定功能边界
4. 识别非功能需求

**Output**
- 功能点列表
- 非功能需求列表

**Run**
```markdown
创建需求文档:
docs/spec/<feature>/analyze/01-requirements.md

内容:
# 需求分析
## 功能点
- [ ] 功能点 1
- [ ] 功能点 2

## 非功能需求
- 性能要求
- 安全要求

## 验收标准
- 可测的验收标准
```

**Expected**
- 文档创建成功
- 功能点清晰
- 验收标准可测

---

## 阶段 2: 任务拆解

### 2.1 任务分解

**Input**
- 需求文档

**Process**
1. 调用 superpowers/planning-and-task-breakdown
2. 分解为可执行任务
3. 评估工作量
4. 确定优先级

**Output**
- 任务列表（每个任务 ≤ 1 天）

**Run**
```markdown
创建任务文档:
docs/spec/<feature>/design/01-tasks.md

内容:
# 任务列表
## 任务 1: [P0] 前端组件开发
- 预估: 4h
- 依赖: 无

## 任务 2: [P0] 后端 API 开发
- 预估: 4h
- 依赖: 无

## 任务 3: [P1] 集成测试
- 预估: 2h
- 依赖: 任务 1, 2
```

**Expected**
- 任务粒度合适
- 依赖关系清晰
- 优先级明确

---

## 阶段 3: 设计

### 3.1 前端设计

**Input**
- 任务列表

**Process**
1. 设计组件结构
2. 定义 Props 接口
3. 规划状态管理
4. 设计样式方案

**Output**
- 组件设计文档
- Props 类型定义

**Run**
```markdown
创建前端设计文档:
docs/spec/<feature>/design/02-frontend-design.md

内容:
# 前端设计
## 组件结构
- TodoList
- TodoItem
- TodoForm

## Props 定义
interface TodoItemProps {
  id: string;
  title: string;
  completed: boolean;
}

## 状态管理
- 使用 React Context
- 状态: todos[], loading, error
```

**Expected**
- 组件职责清晰
- 接口定义完整
- 状态管理方案明确

### 3.2 后端设计

**Input**
- 任务列表

**Process**
1. 设计数据模型
2. 定义 API 接口
3. 规划数据库表结构
4. 设计错误处理

**Output**
- API 设计文档
- 数据模型定义

**Run**
```markdown
创建后端设计文档:
docs/spec/<feature>/design/03-backend-design.md

内容:
# 后端设计
## API 接口
- GET /api/todos - 获取列表
- POST /api/todos - 创建
- PUT /api/todos/:id - 更新
- DELETE /api/todos/:id - 删除

## 数据模型
interface Todo {
  id: string;
  title: string;
  completed: boolean;
  createdAt: Date;
  updatedAt: Date;
}
```

**Expected**
- API 契约明确
- 数据模型完整
- 错误码定义

---

## 阶段 4: 实现

### 4.1 TDD 实现

**Input**
- 设计文档

**Process**
1. 调用 superpowers/test-driven-development
2. 先写测试
3. 实现功能
4. 测试通过

**Run**
```bash
# 1. 编写测试
cat > tests/todo.test.ts << 'EOF'
describe('Todo', () => {
  it('should create a todo', () => {
    // test
  });
});
EOF

# 2. 运行测试（应该失败）
npm test

# 3. 实现代码
# ...

# 4. 再次运行测试（应该通过）
npm test
```

**Expected**
- 测试先失败
- 实现后通过
- 覆盖率达标

### 4.2 代码审查

**Input**
- 实现代码

**Process**
1. 调用 superpowers/code-review
2. 检查代码质量
3. 修复问题

**Output**
- 审查报告
- 修复后的代码

**Run**
```markdown
创建开发记录:
docs/spec/<feature>/develop/31-frontend-impl.md

内容:
# 开发记录
## 变更文件
- src/components/TodoList.tsx
- src/lib/todo.ts

## 测试结果
- 通过: 10
- 失败: 0

## 代码审查
- 问题: 2
- 已修复: 2
```

**Expected**
- 无 P0/P1 问题
- 代码质量达标

---

## 阶段 5: 验证

### 5.1 测试覆盖率

**Input**
- 实现代码

**Process**
1. 运行完整测试套件
2. 生成覆盖率报告
3. 检查是否达标

**Run**
```bash
npm run test:coverage
```

**Expected**
- 覆盖率 ≥ 80%
- 无未测试的关键路径

### 5.2 集成测试

**Input**
- 实现代码

**Process**
1. 运行端到端测试
2. 检查功能完整性

**Run**
```bash
npm run test:e2e
```

**Expected**
- 所有场景通过
- 无回归问题

---

## 阶段 6: 文档更新

### 6.1 API 文档

**Input**
- API 实现

**Process**
1. 生成 API 文档
2. 更新 README

**Run**
```markdown
更新 README.md:
## API
- GET /api/todos
- POST /api/todos
...
```

**Expected**
- 文档与实现一致
- 示例可运行

---

## 闸门检查清单

### G0: 需求澄清后

- [ ] 需求理解正确
- [ ] 验收标准可测
- [ ] 用户确认

### G1: 任务拆解后

- [ ] 任务粒度合适
- [ ] 依赖关系明确
- [ ] 优先级合理

### G2: 设计完成后

- [ ] 设计评审通过
- [ ] API 契约确定
- [ ] 无设计冲突

### G3: 代码实现后

- [ ] 测试全绿
- [ ] 代码审查通过
- [ ] 无 P0/P1 问题

### G4: 验证完成后

- [ ] 覆盖率 ≥ 80%
- [ ] 无性能问题
- [ ] 无安全问题

### G5: 上线前

- [ ] 回滚预案就绪
- [ ] 监控告警配置
- [ ] 文档更新完成
