---
name: nextjs-app-dev-workflow
description: Next.js App 开发工作流
version: 1.0.0
generated_by: your-harness-v4.6.0
generated_at: 2026-06-22T14:30:00
---

# Next.js App 开发工作流

## 概述

本工作流适用于基于 Next.js 的全栈 Web 应用开发，涵盖页面开发、API 路由、数据库迁移等核心流程。

**适用项目特征**:
- Next.js 14.x + TypeScript
- App Router（React Server Components）
- API Routes
- PostgreSQL + Prisma
- pnpm workspaces

---

## 项目模式

根据项目特征，匹配到以下模式：

1. **web-app** (0.95 confidence)
   - 全栈 Web 应用
   - 包含前端页面和后端 API
   
2. **frontend-backend-separation** (0.9 confidence)
   - 前后端分离但共存于同一项目
   - Next.js 的 App Router 实现 SSR
   
3. **typescript-project** (1.0 confidence)
   - TypeScript 严格模式
   - 类型检查是强制的

---

## 开发流程

### 阶段 1: 需求定义 (DEFINE)

**目标**: 明确功能需求和验收标准

**步骤**:
1. 分析需求，确定功能范围
2. 识别涉及的页面和 API
3. 确定数据模型变更（如有）

**闸门**: G0_REQUIREMENTS
- [ ] 需求文档已完成
- [ ] 验收标准已定义
- [ ] 相关人员已确认

---

### 阶段 2: 设计 (DESIGN)

**目标**: 完成技术设计，确定实现方案

**步骤**:
1. 设计页面结构和路由
2. 设计 API 接口
3. 设计数据模型（如需数据库变更）

**闸门**: G1_DESIGN
- [ ] 页面路由设计已完成
- [ ] API 接口已定义
- [ ] 数据模型已设计（如适用）
- [ ] 设计已评审

---

### 阶段 3: 实现 (BUILD)

**目标**: 完成代码实现

**步骤**:
1. 创建页面组件
2. 实现 API 路由
3. 执行数据库迁移（如需）
4. 添加样式和交互

**闸门**: G2_CODE
- [ ] 代码已实现
- [ ] TypeScript 编译通过
- [ ] 无 lint 错误
- [ ] 代码已审查

---

### 阶段 4: 测试 (VERIFY)

**目标**: 确保功能正确性

**步骤**:
1. 运行单元测试
2. 运行集成测试
3. 手动验证页面和 API

**闸门**: G3_TEST
- [ ] 单元测试通过
- [ ] 集成测试通过
- [ ] 手动验证通过
- [ ] 测试覆盖率达标

---

### 阶段 5: 部署 (SHIP)

**目标**: 安全部署到生产环境

**步骤**:
1. 构建应用
2. 执行数据库迁移（如有）
3. 部署到 Vercel
4. 验证生产环境

**闸门**: G4_DEPLOY
- [ ] 构建成功
- [ ] 数据库迁移已执行（如适用）
- [ ] 部署检查清单已完成
- [ ] 监控告警已配置

---

## 特定流程

### 页面开发流程

```markdown
## Step 1: 创建页面组件

**Input**:
- 页面设计稿
- 路由路径

**Run**:
```bash
# 创建页面目录
mkdir -p app/[dynamic]/

# 创建页面组件
touch app/[dynamic]/page.tsx
```

**Expected**:
- 页面组件创建成功
- TypeScript 编译通过

## Step 2: 添加样式

**Run**:
```bash
# 使用 Tailwind CSS
# 或创建 CSS 模块
touch app/[dynamic]/page.module.css
```

## Step 3: 测试页面

**Run**:
```bash
pnpm dev
# 访问 http://localhost:3000/[dynamic]
```
```

### API 路由开发流程

```markdown
## Step 1: 创建 API 路由

**Input**:
- API 设计文档
- 请求/响应格式

**Run**:
```bash
# 创建 API 路由目录
mkdir -p app/api/[resource]

# 创建路由处理文件
touch app/api/[resource]/route.ts
```

**Expected**:
- 路由文件创建成功
- 包含请求处理逻辑

## Step 2: 添加验证

**Run**:
```bash
# 使用 Zod 进行验证
# 安装: pnpm add zod
```

## Step 3: 测试 API

**Run**:
```bash
# 使用 curl 测试
curl http://localhost:3000/api/[resource]
```
```

### 数据库迁移流程

```markdown
## Step 1: 创建迁移

**Input**:
- 数据模型变更

**Run**:
```bash
# 使用 Prisma
npx prisma migrate dev --name add_[model]
```

## Step 2: 验证迁移

**Run**:
```bash
# 生成 Prisma Client
npx prisma generate

# 验证数据库
npx prisma studio
```

## Step 3: 应用到生产

**Run**:
```bash
# 推送迁移到远程
npx prisma migrate deploy
```
```

---

## 质量门禁

### TypeScript 检查

```bash
# 严格模式类型检查
pnpm type-check

# 或使用 tsc
tsc --noEmit
```

### 构建检查

```bash
# 生产构建
pnpm build

# 检查构建产物
ls -lh .next/
```

### Lint 检查

```bash
# ESLint 检查
pnpm lint
```

---

## 常见问题

### Q1: 如何处理动态路由？

**A**: 使用 Next.js 的动态路由语法

```typescript
// app/users/[id]/page.tsx
import { useParams } from 'next/navigation';

export default function UserPage() {
  const params = useParams();
  const userId = params.id;
  // ...
}
```

### Q2: 如何处理表单提交？

**A**: 使用 Next.js 14 的 Server Actions

```typescript
'use server';

async function handleSubmit(formData: FormData) {
  // 处理表单数据
}
```

### Q3: 如何处理数据库连接？

**A**: 使用 Prisma Client

```typescript
import { PrismaClient } from '@prisma/client';

const prisma = new PrismaClient();

// 在 API 路由中使用
export async function GET() {
  const items = await prisma.item.findMany();
  return Response.json(items);
}
```

---

## 参考资源

- [Next.js Documentation](https://nextjs.org/docs)
- [App Router](https://nextjs.org/docs/app)
- [API Routes](https://nextjs.org/docs/app/api-reference)
- [Prisma Documentation](https://www.prisma.io/docs)
- [pnpm Workspaces](https://pnpm.io/workspaces)
