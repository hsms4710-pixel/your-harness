---
name: harness-registry
description: |
  Harness Registry — 多项目 Harness 管理中心。
  功能：项目注册、版本化、共享资源管理、Drift 检测、跨项目对齐、工蜂/GitHub 集成。
version: 2.2.0
author: agent_created
tags: [harness, registry, multi-project, versioning, drift]
---

# Harness Registry

多项目 Harness 管理中心。

## 触发条件

- 查看多项目 Harness 状态
- 注册/注销项目
- 同步共享资源升级
- 检测 Drift
- 跨项目对齐规范
- 与工蜂/GitHub 仓库集成（v2.2 新增）

## Registry 目录结构

```
harness-registry/
  registry.yaml         # 项目注册表
  shared/               # 跨项目共享资源
    skills/             # 共享 Skill 库
    gates/              # 共享 Gates 模板
    hooks/              # 共享 Hooks 配置
    templates/          # 共享 Templates
  projects/
    project-a/          # 项目 A 的 Harness
      constitution.md
      manifest.yaml     # 版本、依赖、同步状态
    project-b/          # 项目 B 的 Harness
      constitution.md
      manifest.yaml
```

## 核心功能

### 1. 项目注册

将项目 Harness 注册到 Registry：

```yaml
# registry.yaml
version: 1
projects:
  - name: payment-service
    repo: git.woa.com/team/payment-service
    harness_version: 1.2.0
    constitution_hash: abc123
    skills: [6-core + staged-rollout + api-versioning]
    shared_resources:
      skills: 1.0.0
      gates: 1.0.0
      hooks: 1.0.0
    last_synced: 2026-06-15
    status: active

  - name: user-service
    repo: git.woa.com/team/user-service
    harness_version: 2.0.0
    constitution_hash: def456
    skills: [6-core + incident-response + observability]
    shared_resources:
      skills: 1.0.0
      gates: 1.0.0
      hooks: 1.0.0
    last_synced: 2026-06-10
    status: active
```

### 2. 工蜂/GitHub 集成（v2.2 新增）

#### 仓库自动发现

```yaml
# 从工蜂/GitHub 获取项目列表
git_discovery:
  # 工蜂
  gongfeng:
    group: "SmartMate"
    branch: "main"
    pattern: "*/.agents/constitution.md"
    
  # GitHub
  github:
    org: "my-org"
    branch: "main"
    pattern: ".agents/constitution.md"
```

#### Webhook 同步

```yaml
# Webhook 触发同步
webhook_sync:
  # 工蜂 MR 合并触发
  gongfeng_mr:
    event: "merge_request.merged"
    action: "sync_harness"
    
  # GitHub PR 合并触发
  github_pr:
    event: "pull_request.closed"
    condition: "merged == true"
    action: "sync_harness"
    
  # 推送触发
  push:
    branches: ["main", "master"]
    action: "check_drift"
```

#### CI 集成

```yaml
# 在 CI 中检查 Harness 同步状态
ci_integration:
  # GitHub Actions
  github_actions:
    - name: Check Harness Drift
      run: |
        python scripts/check-drift.py --repo $GITHUB_REPOSITORY
        
  # 工蜂 CI
  gongfeng_ci:
    - name: Check Harness Drift
      script: |
        python scripts/check-drift.py --repo $CI_PROJECT_PATH
```

### 3. 共享资源管理

#### 共享 Skill 库

```yaml
# shared/skills/manifest.yaml
version: 1.0.0
skills:
  - name: spec-driven-development
    version: 1.2.0
    description: 规格驱动开发
    
  - name: test-driven-development
    version: 1.0.0
    description: 测试驱动开发
    
  - name: code-review-and-quality
    version: 1.1.0
    description: 代码审查和质量
```

#### 共享 Gates 模板

```yaml
# shared/gates/manifest.yaml
version: 1.0.0
gates:
  - name: lint-type-test
    version: 1.0.0
    description: lint + type check + test
    
  - name: coverage
    version: 1.0.0
    description: 覆盖率检查
```

#### 升级通知

当共享资源更新时，所有项目收到通知：

```yaml
# .agents/shared-updates.yaml
pending_updates:
  - resource: skills/spec-driven-development
    from: 1.2.0
    to: 1.3.0
    changelog: "新增假设清单模板"
    status: pending
    
  - resource: gates/coverage
    from: 1.0.0
    to: 1.1.0
    changelog: "支持分支覆盖率"
    status: pending
```

### 3. Drift 检测

定期检查项目 Harness 是否与 Registry 记录一致：

```yaml
# drift-check 结果
project: payment-service
last_check: 2026-06-15T10:30:00Z

# Constitution 变更
drifts:
  - type: constitution_modified
    field: quality_gates.test_coverage
    registry_value: 85%
    actual_value: 80%
    severity: warning
    action: "确认是否需要同步到 Registry"
    
  - type: skill_removed
    skill: performance-optimization
    severity: info
    action: "确认是否需要重新注册"

# Skill 变更
skill_drifts:
  - skill: spec-driven-development
    registry_hash: abc123
    actual_hash: def456
    severity: warning
    action: "Skill 文件有修改，需要同步"

# 总结
summary:
  critical: 0
  warning: 2
  info: 1
  recommendation: "建议同步 Constitution 变更到 Registry"
```

### 4. 跨项目对齐

当需要多个项目遵循同一套规范时：

#### Step 1：选择基准项目

```
基准项目: payment-service
基准版本: v2.0.0
```

#### Step 2：生成差异报告

```yaml
# 与基准项目的差异
diff_report:
  project: user-service
  base: payment-service@v2.0.0
  
  differences:
    - type: skill_missing
      skill: staged-rollout
      base_has: true
      target_has: false
      recommendation: "建议添加 staged-rollout Skill"
      
    - type: gate_threshold_diff
      gate: coverage
      base_value: 85%
      target_value: 80%
      recommendation: "建议提升覆盖率阈值到 85%"
      
    - type: workflow_missing
      workflow: /build-with-review
      base_has: true
      target_has: false
      recommendation: "建议添加 build-with-review 工作流"
```

#### Step 3：对齐策略

| 策略 | 说明 | 适用场景 |
|------|------|----------|
| 全量同步 | 完全复制基准项目配置 | 新项目、低风险 |
| 选择性同步 | 仅同步特定组件 | 存量项目、高风险 |
| 保持差异 | 记录差异但不同步 | 有合理理由的差异 |

#### Step 4：执行对齐

```yaml
# 对齐计划
alignment_plan:
  project: user-service
  base: payment-service@v2.0.0
  strategy: selective
  
  actions:
    - action: add_skill
      skill: staged-rollout
      priority: high
      
    - action: update_gate
      gate: coverage
      from: 80%
      to: 85%
      priority: medium
      gradual: true  # 渐进提升
      
    - action: skip
      item: /build-with-review
      reason: "user-service 不需要此工作流"
```

---

## 操作命令

| 命令 | 作用 |
|------|------|
| `/registry-status` | 查看所有项目状态 |
| `/registry-register` | 注册当前项目到 Registry |
| `/registry-unregister` | 从 Registry 注销项目 |
| `/registry-sync` | 同步共享资源升级 |
| `/registry-drift` | 检测当前项目的 Drift |
| `/registry-align <project>` | 与指定项目对齐 |
| `/registry-diff <project>` | 与指定项目对比 |

---

## 自动同步机制（v2.2 新增）

### 同步触发条件

```yaml
auto_sync:
  # 共享资源版本变更
  shared_resource_update:
    trigger: "shared/*/manifest.yaml 版本变更"
    action: "通知所有项目有新版本可用"
    
  # 项目 Constitution 变更
  constitution_change:
    trigger: ".agents/constitution.md 修改"
    action: "更新 Registry 记录，标记 Drift"
    
  # 定期检查
  periodic_check:
    interval: "daily"
    action: "检查所有项目 Drift 状态"
```

### 同步脚本模板

```python
# scripts/sync-registry.py
import hashlib
import json
import os
from datetime import datetime
from pathlib import Path

class RegistrySync:
    def __init__(self, registry_path: str = "~/.harness-registry"):
        self.registry_path = Path(registry_path).expanduser()
        self.registry_file = self.registry_path / "registry.yaml"
    
    def compute_hash(self, file_path: Path) -> str:
        """计算文件哈希"""
        with open(file_path, 'rb') as f:
            return hashlib.sha256(f.read()).hexdigest()[:12]
    
    def check_drift(self, project_name: str, project_path: str) -> dict:
        """检查项目的 Drift 状态"""
        agents_path = Path(project_path) / ".agents"
        
        drifts = {
            "constitution": None,
            "skills": [],
            "gates": [],
        }
        
        # 检查 Constitution
        constitution_file = agents_path / "constitution.md"
        if constitution_file.exists():
            current_hash = self.compute_hash(constitution_file)
            # 与 Registry 记录对比...
        
        return drifts
    
    def sync_shared_resources(self, project_path: str) -> dict:
        """同步共享资源"""
        shared_path = self.registry_path / "shared"
        target_path = Path(project_path) / ".agents"
        
        results = {
            "skills": [],
            "gates": [],
        }
        
        # 检查共享资源版本...
        # 复制更新的资源...
        
        return results

if __name__ == "__main__":
    import sys
    
    if len(sys.argv) < 2:
        print("Usage: python sync-registry.py <project_path>")
        sys.exit(1)
    
    sync = RegistrySync()
    
    if sys.argv[1] == "check-drift":
        # 检查所有项目
        pass
    elif sys.argv[1] == "sync":
        # 同步共享资源
        pass
    else:
        # 检查指定项目
        drifts = sync.check_drift("project", sys.argv[1])
        print(json.dumps(drifts, indent=2))
```

---

## 注意事项

1. **Registry 存储位置**：
   - 单用户：`~/.harness-registry/`
   - 团队：Git 仓库
   - 企业：私有服务器

2. **版本号规则**：
   - Constitution 变更：minor 版本 +1
   - Skill 添加/删除：minor 版本 +1
   - 重大重构：major 版本 +1

3. **Drift 不是坏事**：项目间有合理差异，Drift 检测是为了发现意外偏差

4. **同步要谨慎**：共享资源升级可能影响所有项目，需要评估影响

5. **工蜂/GitHub 集成**：建议配合 Webhook 实现自动同步

---

## 版本历史

- v2.2.0 (2026-06-15): 增加工蜂/GitHub 集成、自动同步机制、同步脚本模板
- v2.1.0: 初始版本
