---
name: harness-onboarding
description: |
  Harness Onboarding — 给已有 Harness 的项目处理存量代码不合规问题。
  核心场景：项目已有 Harness，但存量代码不符合规范。通过现状锚定→兼容期→渐进收紧→豁免清单实现平滑过渡。
  注意：如果是"从零生成 Harness"，应使用 harness-archaeology → harness-designer 流程。
version: 2.2.0
author: agent_created
tags: [harness, onboarding, legacy, gradual-adoption]
---

# Harness Onboarding

给已有 Harness 的项目处理存量代码不合规问题。

## 触发条件

- 项目**已有 Harness**（`.agents/` 目录存在）
- 存量代码不符合新 Harness 的要求
- 团队希望逐步提升代码质量到规范水平

## 核心场景说明（v2.2 新增）

**本 Skill 的适用场景**：
- ✅ 项目已有 Harness，但存量代码不达标
- ✅ 团队引入了新的质量标准，需要渐进式迁移
- ✅ 需要对不合规代码进行豁免管理

**本 Skill 不适用的场景**：
- ❌ 项目没有 Harness，需要从零生成 → 使用 `harness-archaeology` → `harness-designer`
- ❌ 新项目，从零开始 → 使用 `harness-designer` (Greenfield)

## 核心矛盾

现有代码大概率不满足新 Harness 的要求。因此需要**渐进式收紧**。

## 四阶段流程

### Phase 1：现状锚定

不是"改代码"，而是**把现有代码的真实状态记录下来**。

#### Step 1：生成现状快照

```yaml
# .agents/legacy-snapshot.yaml
generated: 2026-06-15
project: payment-service
scanned_files: 1247
total_lines: 45320

# 质量指标现状
quality_metrics:
  test_coverage: 72%          # 实际覆盖率
  lint_errors: 47             # ESLint 错误数
  type_check_errors: 12       # TypeScript 错误数
  technical_debt: 89          # SonarQube 技术债

# 不符合规范的项
legacy_violations:
  - rule: "所有 API 必须有 OpenAPI spec"
    affected: 12 endpoints
    severity: medium
    files:
      - src/routes/legacy-routes.ts
      - src/routes/internal.ts
  
  - rule: "测试覆盖率 >= 85%"
    current: 72%
    severity: high
    affected: 全局
  
  - rule: "错误处理统一用 AppError"
    affected: 23 files
    severity: low
    files:
      - src/services/old-service.ts
      - src/utils/helpers.ts

# 代码质量分布
quality_distribution:
  excellent: 15%    # 符合所有规范
  good: 45%         # 基本符合，小问题
  needs_work: 30%   # 不符合，需要重构
  legacy: 10%       # 明显过时，计划废弃
```

#### Step 2：运行 Archaeology 扫描

调用 `harness-archaeology` 扫描现有代码，推断现有规范作为 Harness 基线的参考。

---

### Phase 2：生成兼容 Harness

关键原则：**生成的 Harness 必须与现有代码兼容，不能让现有代码全部"不通过"**。

#### Step 1：生成兼容 Constitution

基于 Archaeology 的推断结果，生成与现状兼容的 Constitution：

```markdown
# Constitution — payment-service（兼容期）

## 质量基线（基于现状）
- 测试覆盖率: >= 72%（现状值）
- Lint 错误: 允许 47 个（现状值，但新代码必须为 0）
- Type check: 允许 12 个错误（现状值）

## 新代码要求（严格）
- 新增文件必须有测试，覆盖率 >= 85%
- 新增文件 lint 错误必须为 0
- 新增文件 type check 错误必须为 0

## 旧代码要求（宽松）
- 不强制修改，但修改时必须符合新代码要求
- 逐步改进，不追求一步到位

## 改进目标
- Month 2: lint-errors 上限 47 -> 30
- Month 3: lint-errors 上限 30 -> 15
- Month 4: 测试覆盖率 72% -> 80%
- Month 5: 测试覆盖率 80% -> 85%
- Month 6: 移除所有兼容期宽松策略
```

#### Step 2：配置兼容 Gate

Gates 的阈值基于现状设置：

```yaml
# gates/coverage.yml
test_coverage:
  threshold: 72%        # 现状值，不是目标值
  new_code_threshold: 85%  # 新代码严格要求
  
# gates/lint.yml
lint:
  new_files: strict     # 新文件严格检查
  modified_files: strict
  unchanged_files: warn # 旧文件仅警告，不阻断
```

#### Step 3：配置兼容 Hook

```yaml
# hooks/pre-commit
# 新文件和修改的文件必须通过检查
# 未修改的旧文件不检查（避免阻断）

# hooks/pre-push
# 跑覆盖率检查，但阈值是现状值
# 确保不会因为旧代码问题阻断推送
```

---

### Phase 3：兼容期（Week 1-4）

Harness 生成后，Gate 不立即全量生效：

| Gate | 生效范围 | 兼容期策略 |
|------|----------|-----------|
| pre-commit lint | 仅新文件和修改文件 | 旧文件不检查 |
| pre-commit format | 仅新文件和修改文件 | 旧文件不检查 |
| 测试覆盖率 | 新代码覆盖率 >= 85% | 旧代码不计入 |
| commit message | 全量生效 | 无兼容期 |
| type check | 仅新文件 strict | 旧文件 any 允许 |

---

### Phase 4：渐进收紧（Month 2+）

每月收紧一个维度：

```yaml
# .agents/tighten-plan.yaml
tighten_schedule:
  - month: 2
    target: lint_errors
    from: 47
    to: 30
    action: "团队修复 Top 10 lint 错误"
  
  - month: 3
    target: lint_errors
    from: 30
    to: 15
    action: "自动修复 + 手动修复"
  
  - month: 4
    target: coverage
    from: 72%
    to: 80%
    action: "为低覆盖模块补充测试"
  
  - month: 5
    target: coverage
    from: 80%
    to: 85%
    action: "持续补充测试"
  
  - month: 6
    target: all
    action: "移除所有兼容期宽松策略，全部使用目标值"
```

---

## 豁免清单机制

不是所有旧代码都值得改。豁免清单标记"不改"的部分：

```yaml
# .agents/exemptions.yaml
exemptions:
  - path: "src/legacy/v1/"
    reason: "V1 API 即将废弃，计划 Q3 下线"
    until: 2026-09-30
    auto_action: "到期后自动转为 violation"
    
  - path: "src/third-party-integrations/"
    reason: "第三方 SDK 代码，不建议修改"
    until: null  # 永久豁免
    
  - path: "src/utils/old-helpers.ts"
    reason: "历史工具函数，正在逐步迁移到新模块"
    until: 2026-08-15
    auto_action: "到期后自动转为 violation"
```

### 豁免规则

1. **豁免必须有理由**：不能无理由豁免
2. **豁免必须有截止日期**：除非是永久豁免（如第三方代码）
3. **到期自动升级**：到期后自动从 exemption 转为 violation
4. **豁免数量限制**：单个项目豁免数量不超过总文件数的 20%

---

## 维护命令

| 命令 | 作用 |
|------|------|
| `/harness-legacy-snapshot` | 生成/更新现状快照 |
| `/harness-tighten` | 手动触发渐进收紧的下一步 |
| `/harness-exemptions` | 查看/管理豁免清单 |
| `/harness-progress` | 查看收紧进度 |

---

## 与 harness-archaeology 的分工（v2.2 新增）

| 场景 | 使用 Skill | 说明 |
|------|-----------|------|
| 项目无 Harness，需要从零生成 | `harness-archaeology` → `harness-designer` | 先推断现有规范，再生成 Harness |
| 项目已有 Harness，存量代码不合规 | `harness-onboarding` | 本 Skill，处理渐进收紧 |
| 项目无 Harness，代码完全符合某规范 | 直接 `harness-designer` (Brownfield) | 参考已有规范生成 |

**判断流程**：
```
项目是否有 .agents/ 目录？
  → 否 → 使用 harness-archaeology 推断
  → 是 → 存量代码是否符合 Harness？
           → 是 → 无需 onboarding
           → 否 → 使用 harness-onboarding
```

---

## 注意事项

1. **前置条件**：本 Skill 要求项目已有 Harness（`.agents/` 目录存在）
2. **不要一步到位**：兼容期 + 渐进收紧比一步到位的成功率高得多
3. **先稳后快**：前 4 周是适应期，不要急于收紧
4. **定期回顾**：每月回顾收紧进度，必要时调整计划
5. **团队沟通**：收紧计划需要团队共识，不能单方面决定
6. **豁免要克制**：豁免太多会失去 Harness 的意义
7. **避免误用**：如果是从零生成 Harness，不要使用本 Skill

---

## 版本历史

- v2.2.0 (2026-06-15): 明确与 harness-archaeology 的分工、增加核心场景说明、增加前置条件检查
- v2.1.0: 初始版本
