---
name: harness-validator
description: |
  Harness Validator — 5 层质量验证套件。
  确保生成的 Harness 可用、一致、可执行。
version: 2.2.0
author: agent_created
tags: [harness, validator, quality, validation]
---

# Harness Validator

5 层质量验证套件，确保生成的 Harness 可用、一致、可执行。

## 触发条件

- 生成 Harness 后自动验证
- 用户要求检查 Harness 质量
- 修改 Harness 后验证一致性

## 5 层验证

### L1：Constitution 一致性检查

检查 Constitution 内部是否有矛盾条目，以及每条约束是否有对应的执行机制。

#### 检查项

| 检查项 | 通过条件 | 失败动作 |
|--------|---------|---------|
| 条目矛盾 | 不存在互相矛盾的条目 | 标记矛盾对，人工裁决 |
| Skill 覆盖 | 每条约束至少有 1 个 Skill 执行 | 自动推荐匹配 Skill |
| Gate 覆盖 | 每条约束至少有 1 个 Gate 验证 | 自动推荐匹配 Gate |
| TBD 条目 | TBD 条目有最小约束和解锁条件 | 提醒补充 |

#### 检查逻辑

```yaml
# L1 检查结果
l1_results:
  contradictions:
    - item1: "必须使用 TypeScript"
    - item2: "允许 JavaScript"
    - status: fail
    - action: "标记矛盾对，人工裁决"
    
  skill_coverage:
    - constraint: "必须写测试"
      has_skill: true
      skill: "test-driven-development"
      status: pass
      
    - constraint: "API 必须有 OpenAPI spec"
      has_skill: false
      status: fail
      recommendation: "添加 api-versioning Skill"
      
  gate_coverage:
    - constraint: "测试覆盖率 >= 85%"
      has_gate: true
      gate: "coverage.yml"
      status: pass
      
    - constraint: "错误处理统一用 AppError"
      has_gate: false
      status: fail
      recommendation: "添加 error-handling Gate"
```

---

### L2：Skill-Gate 覆盖度分析

生成覆盖矩阵：

```yaml
# L2 覆盖矩阵
l2_matrix:
  constitution_constraints:
    - id: C1
      name: "必须写测试"
      skill: "test-driven-development"
      gate: "coverage.yml"
      hook: "pre-commit"
      status: covered
      
    - id: C2
      name: "API 必须有 OpenAPI spec"
      skill: null
      gate: null
      hook: null
      status: uncovered
      gap: "缺少 Skill 和 Gate"
      
    - id: C3
      name: "错误处理统一用 AppError"
      skill: null
      gate: null
      hook: null
      status: uncovered
      gap: "缺少 Skill 和 Gate"

  summary:
    total_constraints: 15
    covered: 13
    uncovered: 2
    coverage_rate: 86.7%
    status: needs_fix
```

---

### L3：Dry-run 验证

用模拟任务走通完整工作流：

#### 检查项

| 场景 | 预期 | 检查方法 |
|------|------|---------|
| 坏代码拦截 | Gate 阻断 | 写一段不通过 lint 的代码，验证 Gate 拦截 |
| 好代码放行 | Gate 放行 | 写一段规范代码，验证 Gate 放行 |
| Hook 时机 | 正确触发 | 模拟 git commit，验证 Hook 执行 |
| Workflow 端到端 | 可走通 | 模拟 /spec → /plan → /build → /review |

#### 检查逻辑

```yaml
# L3 检查结果
l3_results:
  bad_code_intercept:
    test: "写一段缺少类型注解的 TypeScript 代码"
    expected: "blocked by type-check gate"
    actual: "blocked by type-check gate"
    status: pass
    
  good_code_pass:
    test: "写一段符合规范的 TypeScript 代码"
    expected: "passed by all gates"
    actual: "passed by all gates"
    status: pass
    
  hook_timing:
    test: "模拟 git commit"
    expected: "pre-commit hook 执行"
    actual: "pre-commit hook 执行"
    status: pass
    
  workflow_e2e:
    test: "/spec → /plan → /build → /review"
    expected: "全流程可走通"
    actual: "/plan 阶段缺少模板"
    status: fail
    error: "plan-template.md 不存在"
```

---

### L4：Round-trip 校验

用 Archaeology 模式扫描"按 Harness 生成的模拟代码"：

```yaml
# L4 检查结果
l4_results:
  inferred_from_harness_code:
    - item: "使用 TypeScript"
      matches_constitution: true
      
    - item: "错误处理用 AppError"
      matches_constitution: false
      
    - item: "API 风格 REST"
      matches_constitution: true

  mismatches:
    - inferred: "错误处理用 AppError"
      constitution: "错误处理用 AppError"
      status: match
      
    - inferred: "日志格式 JSON"
      constitution: "日志格式未指定"
      status: uncovered
      recommendation: "Constitution 中添加日志格式规范"

  summary:
    total_inferred: 15
    matched: 14
    mismatched: 1
    uncovered: 1
    status: warning
```

---

### L5：反模式审计

检测已知坏味道：

```yaml
# L5 反模式检查
l5_results:
  anti_patterns:
    - pattern: "Gate 过严"
      check: "pre-commit 耗时 >1s 的检查项数"
      threshold: 3
      actual: 1
      status: pass
      
    - pattern: "Hook 过慢"
      check: "模拟执行 pre-commit hook 计时"
      threshold: 3s
      actual: 2.1s
      status: pass
      
    - pattern: "Skill 过多"
      check: "Skill 总数"
      threshold: 12
      actual: 8
      status: pass
      
    - pattern: "Constitution 过长"
      check: "Constitution 条目数"
      threshold: 30
      actual: 15
      status: pass
      
    - pattern: "孤立 Skill"
      check: "Skill 无对应 Constitution 约束"
      threshold: 0
      actual: 1
      status: warning
      details: "documentation-as-code 无 Constitution 对应"
      
    - pattern: "Dead Gate"
      check: "Gate 检查的条件在 Constitution 中不存在"
      threshold: 0
      actual: 0
      status: pass

  summary:
    total_patterns: 6
    passed: 5
    warnings: 1
    critical: 0
    status: warning
```

---

## 验证报告

5 层验证完成后生成验证报告：

```markdown
# Harness Quality Report — payment-service

## 总评: CONDITIONAL_PASS

## L1: Constitution 一致性
- 条目矛盾: 0
- Skill 覆盖缺口: 1 (API spec 约束无 Skill)
- Gate 覆盖缺口: 1 (API spec 约束无 Gate)
- TBD 条目: 2 (均有最小约束)
- 状态: NEEDS_FIX

## L2: 覆盖度分析
- 总约束: 15
- 已覆盖: 13/15 (86.7%)
- 空缺: ["API spec", "错误处理统一"]
- 状态: NEEDS_FIX

## L3: Dry-run 验证
- 坏代码拦截: PASS (5/5)
- 好代码放行: PASS (5/5)
- Hook 时机: PASS
- Workflow 端到端: FAIL (plan-template.md 不存在)
- 状态: NEEDS_FIX

## L4: Round-trip 校验
- Constitution 一致性: 14/15 条目一致
- 不一致: "日志格式" -> Constitution 未指定
- 状态: WARNING

## L5: 反模式审计
- Gate 过严: OK
- Hook 速度: OK
- Skill 数量: OK
- Constitution 长度: OK
- 孤立 Skill: 1 (documentation-as-code)
- 状态: WARNING

## 修复建议

1. [高优先级] 添加 api-versioning Skill 和对应的 Gate
2. [高优先级] 创建 plan-template.md
3. [中优先级] 确认 documentation-as-code Skill 是否需要
4. [低优先级] Constitution 中添加日志格式规范
```

---

## 验证触发时机

| 时机 | 验证层级 | 说明 |
|------|---------|------|
| Harness 生成后 | 全部 5 层 | 确保生成质量 |
| Constitution 修改后 | L1, L2 | 检查一致性 |
| Skill 添加/删除后 | L2 | 检查覆盖度 |
| Gate 修改后 | L3 | 验证拦截逻辑 |
| 定期检查 | L5 | 检测反模式 |

---

## 自动化检查脚本指引（v2.2 新增）

对于 L3 (Dry-run) 验证，建议配合自动化脚本：

### Python 项目检查脚本

```python
# scripts/check-harness.py
import subprocess
import sys

def check_lint():
    """检查 lint 配置是否可执行"""
    try:
        result = subprocess.run(
            ["ruff", "check", "--version"],
            capture_output=True,
            text=True
        )
        return result.returncode == 0
    except FileNotFoundError:
        return False

def check_type_check():
    """检查类型检查配置是否可执行"""
    try:
        result = subprocess.run(
            ["mypy", "--version"],
            capture_output=True,
            text=True
        )
        return result.returncode == 0
    except FileNotFoundError:
        return False

def check_test():
    """检查测试配置是否可执行"""
    try:
        result = subprocess.run(
            ["pytest", "--version"],
            capture_output=True,
            text=True
        )
        return result.returncode == 0
    except FileNotFoundError:
        return False

def run_dry_run():
    """运行 Dry-run 验证"""
    checks = {
        "lint": check_lint(),
        "type_check": check_type_check(),
        "test": check_test(),
    }
    return all(checks.values()), checks

if __name__ == "__main__":
    passed, results = run_dry_run()
    print("Dry-run 验证结果:")
    for check, result in results.items():
        status = "✅" if result else "❌"
        print(f"  {status} {check}")
    sys.exit(0 if passed else 1)
```

### Node.js 项目检查脚本

```javascript
// scripts/check-harness.js
const { execSync } = require('child_process');

function checkLint() {
  try {
    execSync('npx eslint --version', { stdio: 'pipe' });
    return true;
  } catch {
    return false;
  }
}

function checkTypeCheck() {
  try {
    execSync('npx tsc --version', { stdio: 'pipe' });
    return true;
  } catch {
    return false;
  }
}

function checkTest() {
  try {
    execSync('npx vitest --version || npx jest --version', { stdio: 'pipe' });
    return true;
  } catch {
    return false;
  }
}

const results = {
  lint: checkLint(),
  type_check: checkTypeCheck(),
  test: checkTest(),
};

console.log('Dry-run 验证结果:');
Object.entries(results).forEach(([check, result]) => {
  console.log(`  ${result ? '✅' : '❌'} ${check}`);
});

process.exit(Object.values(results).every(Boolean) ? 0 : 1);
```

### Go 项目检查脚本

```go
// scripts/check_harness.go
package main

import (
	"fmt"
	"os"
	"os/exec"
)

func checkLint() bool {
	cmd := exec.Command("golangci-lint", "version")
	err := cmd.Run()
	return err == nil
}

func checkTest() bool {
	cmd := exec.Command("go", "version")
	err := cmd.Run()
	return err == nil
}

func main() {
	checks := map[string]bool{
		"lint":    checkLint(),
		"test":    checkTest(),
	}
	
	fmt.Println("Dry-run 验证结果:")
	allPassed := true
	for check, result := range checks {
		status := "✅"
		if !result {
			status = "❌"
			allPassed = false
		}
		fmt.Printf("  %s %s\n", status, check)
	}
	
	if !allPassed {
		os.Exit(1)
	}
}
```

---

## 与 SmartMate 验证协议集成（v2.2 新增）

如果项目使用 SmartMate 的验证协议，可以复用其 5 层评审质量检查：

```yaml
# 与 SmartMate 集成
integration:
  # 复用 SmartMate 的评审质量检查
  review_quality_checks:
    - prd-quality
    - acceptance-testability
    - api-compatibility
    - security-risk-coverage
  
  # 复用 SmartMate 的闸门协议
  gate_protocol:
    - G0_CLARIFICATION_REVIEW
    - G1_PRD_REVIEW
    - G2_TASKS_REVIEW
    - G3_SLICE_REVIEW
    - G4_VERIFY_REVIEW
    - G5_REVIEW_REMEDIATION
    - G6_SHIP_APPROVAL
```

---

## 注意事项

1. **验证不是可选项**：生成后必须通过验证，不通过的 Harness 比没有 Harness 更危险

2. **自动修复有限**：验证器可以自动推荐修复方案，但最终决策权在人类

3. **渐进改进**：不要求一次通过所有验证，可以先修复高优先级问题

4. **记录历史**：每次验证结果应记录，便于追踪改进进度

5. **自动化脚本**：L3 验证建议配合自动化脚本，确保检查工具可用

---

## 版本历史

- v2.2.0 (2026-06-15): 增加自动化检查脚本指引（Python/Node.js/Go）、与 SmartMate 验证协议集成
- v2.1.0: 初始版本
