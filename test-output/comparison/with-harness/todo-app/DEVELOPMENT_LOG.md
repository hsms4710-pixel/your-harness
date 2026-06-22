# TODO App 开发日志（使用 Your Harness）

## 开发方式

使用 Your Harness 生成开发流程，按流程执行。

---

## 会话 0: 生成开发流程

### 用户请求
```
"帮我生成开发流程"
```

### AI 响应

1. 调用 harness-archaeology
   - 识别技术栈: TypeScript + Next.js
   - 识别项目类型: Web App
   - 匹配模式: web-app

2. 调用 harness-designer
   - 生成 .harness/SKILL.md
   - 生成 .harness/templates/TEMPLATE-feature.md
   - 生成闸门定义

3. 展示生成结果，等待用户确认

### 产出

```
.harness/
├── SKILL.md
├── templates/
│   └── TEMPLATE-feature.md
└── references/
    └── gate-protocol.md
```

---

## 会话 1: 新功能开发 - TODO 基础功能

### 用户请求
```
"我要开发 TODO 基础功能（增删改查）"
```

### AI 响应（按 SKILL.md 流程执行）

#### 阶段 1: 需求分析 (G0)

**AI**: "进入需求分析阶段"

1. 调用 superpowers/idea-refine
2. 创建需求文档

**Run**:
```markdown
创建: docs/spec/todo-basic/analyze/01-requirements.md

内容:
# 需求分析
## 功能点
- [ ] 创建 TODO
- [ ] 删除 TODO
- [ ] 更新 TODO 状态
- [ ] 获取 TODO 列表

## 非功能需求
- 响应时间 < 200ms
- 支持本地存储

## 验收标准
- 能够创建、删除、更新 TODO
- 刷新页面数据不丢失
```

**闸门检查**:
- [x] 需求理解正确
- [x] 验收标准可测
- [ ] 用户确认 ← 等待确认

**用户**: "确认"

#### 阶段 2: 任务拆解 (G1)

**AI**: "进入任务拆解阶段"

1. 调用 superpowers/planning-and-task-breakdown
2. 创建任务文档

**Run**:
```markdown
创建: docs/spec/todo-basic/design/01-tasks.md

内容:
# 任务列表
## 任务 1: [P0] 数据模型设计
- 预估: 1h
- 依赖: 无

## 任务 2: [P0] 后端 API 开发
- 预估: 2h
- 依赖: 任务 1

## 任务 3: [P0] 前端组件开发
- 预估: 3h
- 依赖: 任务 1

## 任务 4: [P1] 集成测试
- 预估: 1h
- 依赖: 任务 2, 3
```

**闸门检查**:
- [x] 任务粒度合适（最大 3h）
- [x] 依赖关系明确
- [x] 优先级合理

#### 阶段 3: 设计 (G2)

**AI**: "进入设计阶段"

1. 前端设计
2. 后端设计

**Run**:
```markdown
创建: docs/spec/todo-basic/design/02-frontend-design.md
创建: docs/spec/todo-basic/design/03-backend-design.md
```

**闸门检查**:
- [x] 设计评审通过
- [x] API 契约确定

#### 阶段 4: 实现 (G3)

**AI**: "进入实现阶段"

1. 调用 superpowers/test-driven-development
2. TDD 实现

**Run**:
```bash
# 创建测试
npm test  # 失败
# 实现代码
npm test  # 通过
```

**创建开发记录**:
```markdown
创建: docs/spec/todo-basic/develop/31-backend-impl.md
创建: docs/spec/todo-basic/develop/32-frontend-impl.md
```

**闸门检查**:
- [x] 测试全绿
- [x] 代码审查通过
- [x] 无 P0/P1 问题

#### 阶段 5: 验证 (G4)

**AI**: "进入验证阶段"

**Run**:
```bash
npm run test:coverage  # 覆盖率 85%
```

**闸门检查**:
- [x] 覆盖率 ≥ 80% ✓
- [x] 无性能问题

#### 阶段 6: 文档更新 (G5)

**AI**: "更新文档"

**Run**:
```markdown
更新: README.md
更新: docs/api.md
```

**闸门检查**:
- [x] 回滚预案就绪
- [x] 文档更新完成

### 产出文件

```
todo-app/
├── .harness/
│   ├── SKILL.md
│   └── templates/
│       └── TEMPLATE-feature.md
├── docs/
│   └── spec/
│       └── todo-basic/
│           ├── analyze/
│           │   └── 01-requirements.md
│           ├── design/
│           │   ├── 01-tasks.md
│           │   ├── 02-frontend-design.md
│           │   └── 03-backend-design.md
│           └── develop/
│               ├── 31-backend-impl.md
│               └── 32-frontend-impl.md
├── src/
│   ├── app/
│   │   └── page.tsx
│   ├── components/
│   │   └── TodoList.tsx
│   └── lib/
│       └── todo.ts
├── tests/
│   └── todo.test.ts
└── README.md
```

---

## 会话 2: 新功能开发 - 用户认证

### 用户请求
```
"添加用户认证功能"
```

### AI 响应

#### 阶段 1: 需求分析 (G0)

**AI**: "进入需求分析阶段"

1. 调用 superpowers/idea-refine
2. 分析与现有 TODO 功能的兼容性

**Run**:
```markdown
创建: docs/spec/user-auth/analyze/01-requirements.md

内容:
# 需求分析
## 功能点
- [ ] 用户注册
- [ ] 用户登录
- [ ] 退出登录
- [ ] TODO 数据隔离

## 与现有功能的兼容性
- TODO 数据需要按用户隔离
- 现有的 localStorage 需要迁移到服务端
- API 需要添加认证头

## 验收标准
- 用户登录后只能看到自己的 TODO
- 退出登录后需要重新登录
```

**闸门检查**:
- [x] 需求理解正确
- [x] 与现有功能兼容性分析完成
- [x] 验收标准可测

#### 阶段 2: 任务拆解 (G1)

**AI**: "进入任务拆解阶段"

**Run**:
```markdown
创建: docs/spec/user-auth/design/01-tasks.md

内容:
# 任务列表
## 任务 1: [P0] 数据库设计
- 预估: 1h
- 依赖: 无

## 任务 2: [P0] 认证 API 开发
- 预估: 2h
- 依赖: 任务 1

## 任务 3: [P0] 前端认证组件
- 预估: 2h
- 依赖: 任务 2

## 任务 4: [P1] TODO API 改造
- 预估: 1h
- 依赖: 任务 2

## 任务 5: [P1] 迁移脚本
- 预估: 1h
- 依赖: 任务 1-3
```

**闸门检查**:
- [x] 任务粒度合适
- [x] 依赖关系明确
- [x] 迁移方案考虑

### 产出文件

```
todo-app/
└── docs/
    └── spec/
        └── user-auth/
            ├── analyze/
            │   └── 01-requirements.md
            └── design/
                └── 01-tasks.md
```

---

## 会话 3: Bug 修复

### 用户请求
```
"登录后 TODO 列表显示空白"
```

### AI 响应

#### 阶段 1: Bug 分析

**AI**: "进入 Bug 分析阶段"

1. 调用 superpowers/debugging
2. 分析问题原因

**Run**:
```markdown
创建: docs/spec/bugfix-todo-empty/analyze/01-analysis.md

内容:
# Bug 分析
## 问题描述
登录后 TODO 列表显示空白

## 根因分析
TODO API 查询时没有传递 user_id 参数

## 影响范围
- 所有登录用户
- 新创建的 TODO 无法显示

## 修复方案
在查询 TODO 时添加 user_id 过滤
```

**闸门检查**:
- [x] 根因分析完成
- [x] 影响范围明确
- [x] 修复方案确定

#### 阶段 2: 修复实现

**AI**: "进入修复实现阶段"

1. 调用 superpowers/test-driven-development
2. 修复代码
3. 更新测试

**Run**:
```bash
# 修复代码
# 更新测试
npm test  # 通过
```

**创建修复记录**:
```markdown
创建: docs/spec/bugfix-todo-empty/develop/01-fix.md

内容:
# 修复记录
## 变更文件
- src/lib/todo.ts
- tests/todo.test.ts

## 修复内容
- 添加 user_id 过滤参数
- 更新相关测试

## 测试结果
- 通过: 12
- 失败: 0
```

**闸门检查**:
- [x] 测试全绿
- [x] 无回归问题
- [x] 修复记录完成

---

## 最终产出

```
todo-app/
├── .harness/
│   ├── SKILL.md
│   └── templates/
│       └── TEMPLATE-feature.md
├── docs/
│   └── spec/
│       ├── todo-basic/
│       │   ├── analyze/
│       │   ├── design/
│       │   └── develop/
│       ├── user-auth/
│       │   ├── analyze/
│       │   └── design/
│       └── bugfix-todo-empty/
│           ├── analyze/
│           └── develop/
├── src/
│   ├── app/
│   │   └── page.tsx
│   ├── components/
│   │   ├── TodoList.tsx
│   │   └── LoginForm.tsx
│   └── lib/
│       ├── todo.ts
│       └── auth.ts
├── tests/
│   ├── todo.test.ts
│   └── auth.test.ts
└── README.md
```

---

## 优势总结

1. **流程连贯**：每次会话都按流程执行，前后衔接
2. **知识沉淀**：所有决策、设计、开发记录都有文档
3. **质量可控**：每个阶段都有闸门检查
4. **协作友好**：其他人可以快速了解项目历史
