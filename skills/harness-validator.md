---
name: harness-validator
description: |
  Harness Validator — 5 层质量验证套件。
  确保生成的 Harness 可用、一致、可执行。
  v3.7.2: 增强报告功能，支持多项目对比、自定义模板、自动发送、报告模板库。
  v3.6.1: 添加报告生成功能，支持 HTML/PDF 格式输出、质量评分、趋势分析、定期报告。
version: 3.7.2
author: agent_created
tags: [harness, validator, quality, validation, report, html, pdf, score, trend, comparison, template, auto-send]
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

## 报告生成功能（v3.6.1 新增）

### /harness-report 命令

生成完整的 Harness 质量报告。

#### 用法

```bash
# 生成 Markdown 格式报告
/harness-report

# 生成 HTML 格式报告
/harness-report --format html

# 生成 PDF 格式报告
/harness-report --format pdf

# 保存到指定路径
/harness-report --output ./reports/harness-quality-report.html

# 包含趋势分析
/harness-report --trend

# 指定历史范围
/harness-report --trend --range 30d  # 最近 30 天
```

#### 报告内容结构

```markdown
# Harness Quality Report

## 📊 执行摘要

| 指标 | 值 |
|------|-----|
| 项目名称 | my-fastapi-app |
| 生成时间 | 2026-06-16T20:00:00Z |
| 总体评分 | 85/100 ⭐⭐⭐⭐ |
| 验证状态 | CONDITIONAL_PASS |
| 改进建议 | 3 条 |

## 📈 质量评分

### 总体评分: 85/100

```
████████████████████░░░░░░ 85%
```

### 分项评分

| 维度 | 评分 | 权重 | 加权分 |
|------|------|------|--------|
| L1: Constitution 一致性 | 90/100 | 25% | 22.5 |
| L2: 覆盖度 | 87/100 | 25% | 21.75 |
| L3: Dry-run 验证 | 95/100 | 20% | 19.0 |
| L4: Round-trip 校验 | 80/100 | 15% | 12.0 |
| L5: 反模式审计 | 92/100 | 15% | 13.8 |
| **总计** | - | 100% | **85.05** |

## 📋 详细验证结果

### L1: Constitution 一致性 (90/100)

✅ 条目矛盾: 0
✅ Skill 覆盖率: 95% (19/20)
✅ Gate 覆盖率: 90% (18/20)
⚠️ TBD 条目: 2 (均有最小约束)

**扣分原因**:
- Gate 覆盖缺口: 1 个约束无对应 Gate (-5 分)

### L2: 覆盖度分析 (87/100)

| 约束 | Skill | Gate | Hook | 状态 |
|------|-------|------|------|------|
| 必须写测试 | ✅ | ✅ | ✅ | 完全覆盖 |
| API 必须有 OpenAPI spec | ✅ | ❌ | ❌ | 部分覆盖 |
| 错误处理统一 | ✅ | ✅ | ✅ | 完全覆盖 |
| 代码覆盖率 >= 60% | ❌ | ✅ | ✅ | 完全覆盖 |

**扣分原因**:
- API spec 约束缺少 Gate (-5 分)
- 1 个约束无 Hook 配置 (-8 分)

### L3: Dry-run 验证 (95/100)

✅ 坏代码拦截: 5/5 测试通过
✅ 好代码放行: 5/5 测试通过
✅ Hook 时机: 全部正确
✅ Workflow 端到端: 通过

**扣分原因**:
- pre-commit hook 执行时间略长 (-5 分)

### L4: Round-trip 校验 (80/100)

| 推断项 | Constitution 一致性 | 状态 |
|--------|---------------------|------|
| 使用 Python 3.11+ | ✅ 一致 | 匹配 |
| 使用 FastAPI | ✅ 一致 | 匹配 |
| 使用 PDM 包管理 | ✅ 一致 | 匹配 |
| 日志格式 JSON | ❌ 未指定 | 未覆盖 |
| API 风格 REST | ✅ 一致 | 匹配 |

**扣分原因**:
- 日志格式推断与 Constitution 不匹配 (-20 分)

### L5: 反模式审计 (92/100)

✅ Gate 过严: OK (最严耗时 2.1s)
✅ Hook 速度: OK (平均 1.5s)
✅ Skill 数量: OK (8 个)
✅ Constitution 长度: OK (15 条)
⚠️ 孤立 Skill: 1 个
✅ Dead Gate: 0 个

**扣分原因**:
- 1 个孤立 Skill (-8 分)

## 🎯 改进建议

### 高优先级 (建议立即处理)

1. **添加 API spec Gate**
   - 当前状态: API spec 约束无对应 Gate
   - 建议操作: 创建 `gates/api-spec.yml`，检查 API 是否符合 OpenAPI 规范
   - 预期收益: 提升 L2 覆盖度 5 分
   
2. **添加日志格式规范**
   - 当前状态: Constitution 未指定日志格式
   - 建议操作: 在 Constitution 中添加日志格式规范
   - 预期收益: 提升 L4 一致性 20 分

### 中优先级 (建议本周处理)

3. **优化 pre-commit hook 速度**
   - 当前状态: 平均执行时间 2.1s
   - 建议操作: 考虑并行执行检查，或添加缓存
   - 预期收益: 提升 L3 验证分数

### 低优先级 (可延后处理)

4. **清理孤立 Skill**
   - 当前状态: `documentation-as-code` Skill 无对应 Constitution 约束
   - 建议操作: 删除或添加对应约束
   - 预期收益: 提升 L5 审计分数

## 📊 趋势分析

### 质量评分趋势 (最近 30 天)

```
100|                              ╭─
 95|                         ╭───╯
 90|                    ╭───╯
 85|               ╭───╯
 80|          ╭───╯
 75|     ╭───╯
 70|────╯
    │         │         │         │
    6/16      6/20      6/24      6/28
```

### 改进项追踪

| 改进项 | 创建日期 | 目标日期 | 状态 |
|--------|----------|----------|------|
| 添加 API spec Gate | 6/16 | 6/20 | 进行中 |
| 添加日志格式规范 | 6/16 | 6/23 | 待开始 |
| 优化 hook 速度 | 6/16 | 6/27 | 待开始 |

## 📁 附件

- [完整验证日志](./logs/2026-06-16-validation.log)
- [L2 覆盖矩阵详情](./reports/coverage-matrix-2026-06-16.md)
- [L5 反模式详细报告](./reports/anti-patterns-2026-06-16.md)
```

### 报告格式支持

#### HTML 格式

生成交互式 HTML 报告，支持：
- 图表可视化
- 折叠/展开详情
- 导出功能
- 打印友好

```html
<!-- .harness/reports/harness-report-2026-06-16.html -->
<!DOCTYPE html>
<html>
<head>
    <title>Harness Quality Report - my-fastapi-app</title>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
</head>
<body>
    <h1>Harness Quality Report</h1>
    
    <!-- 评分图表 -->
    <canvas id="scoreChart"></canvas>
    
    <script>
        new Chart(document.getElementById('scoreChart'), {
            type: 'bar',
            data: {
                labels: ['L1', 'L2', 'L3', 'L4', 'L5'],
                datasets: [{
                    label: 'Score',
                    data: [90, 87, 95, 80, 92],
                    backgroundColor: 'rgba(75, 192, 192, 0.2)',
                    borderColor: 'rgba(75, 192, 192, 1)',
                    borderWidth: 1
                }]
            }
        });
    </script>
</body>
</html>
```

#### PDF 格式

生成可打印的 PDF 报告，支持：
- 专业排版
- 页码和目录
- 图表嵌入
- 适合归档

### 定期报告功能

#### 自动发送配置

```yaml
# .harness/report-config.yaml

schedule:
  # 每周一上午 9 点
  cron: "0 9 * * 1"
  
  # 或每周发送
  weekly: true
  weekday: 1  # Monday
  
  # 或每月发送
  monthly: true
  day: 1

format: html
output: .harness/reports/weekly-report-{date}.{format}

recipients:
  - email: "team-lead@example.com"
    role: team_lead
  - email: "dev-team@example.com"
    role: team

notifications:
  # 评分下降超过 10 分时发送警告
  score_drop_threshold: 10
  # 出现 critical 问题时立即通知
  critical_issues: true
  # 改进建议超过 5 条时提醒
  suggestions_threshold: 5
```

#### 报告发送脚本

```python
# scripts/send-weekly-report.py

import smtplib
from email.mime.multipart import MIMEMultipart
from email.mime.text import MIMEText
from datetime import datetime, timedelta
import os


def generate_report(project_path: str) -> str:
    """生成本周报告"""
    # 调用 /harness-report 命令
    # 收集验证结果
    # 生成报告内容
    pass


def send_email(recipient: str, subject: str, body: str, attachment_path: str = None):
    """发送邮件"""
    msg = MIMEMultipart()
    msg['From'] = os.environ.get('SMTP_USER')
    msg['To'] = recipient
    msg['Subject'] = subject
    msg.attach(MIMEText(body, 'html'))
    
    # 添加附件
    if attachment_path:
        with open(attachment_path, 'rb') as f:
            attachment = MIMEText(f.read(), _subtype='html')
            attachment.add_header('Content-Disposition', 'attachment', filename=os.path.basename(attachment_path))
            msg.attach(attachment)
    
    # 发送
    with smtplib.SMTP_SSL('smtp.gmail.com', 465) as server:
        server.login(os.environ.get('SMTP_USER'), os.environ.get('SMTP_PASSWORD'))
        server.sendmail(os.environ.get('SMTP_USER'), recipient, msg.as_string())


def main():
    # 读取配置
    config_path = os.path.join(os.getcwd(), '.harness', 'report-config.yaml')
    
    # 生成报告
    report = generate_report(os.getcwd())
    
    # 发送报告
    with open(config_path) as f:
        config = yaml.safe_load(f)
    
    for recipient in config['recipients']:
        send_email(
            recipient['email'],
            f"Harness Quality Report - {datetime.now().strftime('%Y-%m-%d')}",
            report
        )


if __name__ == "__main__":
    main()
```

### 质量评分算法

```yaml
# 评分权重配置
scoring_weights:
  L1_constitution_consistency: 25
  L2_coverage_analysis: 25
  L3_dry_run: 20
  L4_round_trip: 15
  L5_anti_patterns: 15

# 评分规则
scoring_rules:
  L1:
    contradictions: 0分/个矛盾
    skill_gap: 5分/个缺口
    gate_gap: 5分/个缺口
    
  L2:
    coverage: "线性插值 (0% = 0分, 100% = 100分)"
    uncovered_penalty: 10分/个未覆盖约束
    
  L3:
    test_pass: 20分/类
    hook_timing: "超过3s扣20分"
    
  L4:
    consistency: "线性插值"
    uncovered: 10分/个
    
  L5:
    anti_pattern: 10分/个

# 等级映射
grade_mapping:
  A: [90, 100]
  B: [80, 90)
  C: [70, 80)
  D: [60, 70)
  F: [0, 60)
```

### 趋势数据存储

```yaml
# .harness/metrics/history.yaml

records:
  - date: "2026-06-16"
    total_score: 85
    scores:
      L1: 90
      L2: 87
      L3: 95
      L4: 80
      L5: 92
    issues:
      - type: "gate_gap"
        severity: "warning"
        count: 1
      - type: "uncovered_constraint"
        severity: "warning"
        count: 1
    suggestions_count: 3
    
  - date: "2026-06-09"
    total_score: 82
    scores:
      L1: 88
      L2: 85
      L3: 95
      L4: 78
      L5: 92
    issues:
      - type: "gate_gap"
        severity: "warning"
        count: 2
      - type: "uncovered_constraint"
        severity: "warning"
        count: 2
    suggestions_count: 4
```

---

## 报告增强功能（v3.7.2 新增）

### 多项目对比报告

支持生成多个项目的对比分析报告：

```bash
/harness-report --compare project-a,project-b,project-c
```

#### 对比报告内容

```markdown
# 多项目对比报告

## 项目概览

| 项目 | 语言 | 框架 | 质量评分 | 趋势 |
|------|------|------|----------|------|
| project-a | Python | FastAPI | 92 | ↑ +5% |
| project-b | Go | Gin | 88 | → 持平 |
| project-c | TypeScript | Next.js | 95 | ↓ -2% |

## 质量维度对比

### 测试覆盖率

| 项目 | 当前 | 目标 | 达成率 |
|------|------|------|--------|
| project-a | 85% | 80% | 106% |
| project-b | 72% | 80% | 90% |
| project-c | 92% | 85% | 108% |

### 代码质量

| 项目 | Lint 错误 | Type 错误 | 技术债 |
|------|----------|----------|--------|
| project-a | 0 | 0 | 12 |
| project-b | 5 | 2 | 28 |
| project-c | 0 | 1 | 8 |

## 改进建议

### project-b 需要关注

1. **测试覆盖率不足**：当前 72%，目标 80%，建议补充核心模块测试
2. **Lint 错误较多**：5 个错误需要修复

### project-c 需要关注

1. **Type 错误**：1 个 Type 错误需要修复
2. **技术债上升**：较上周增加 3 个，需要关注
```

### 自定义报告模板

支持使用自定义模板生成报告：

```bash
/harness-report --template custom-report
```

#### 内置模板库

| 模板名称 | 用途 | 输出格式 |
|----------|------|----------|
| `default` | 默认综合报告 | HTML/PDF |
| `quality-only` | 仅质量相关 | HTML |
| `security-only` | 仅安全相关 | HTML/PDF |
| `trend` | 趋势分析 | HTML |
| `comparison` | 多项目对比 | HTML |
| `executive` | 管理层摘要 | PDF |

#### 自定义模板格式

```yaml
# .harness/report-templates/custom-report.yaml
name: custom-report
description: "自定义综合报告"
version: "1.0"

output:
  format: html
  filename: "custom-report-{date}.html"

sections:
  - type: header
    title: "项目质量报告"
    include: [project_name, timestamp]
    
  - type: score
    include: [overall, by_category]
    
  - type: issues
    include: [critical, warning]
    limit: 10
    
  - type: suggestions
    include: [all]
    limit: 5
    
  - type: footer
    include: [generated_by, contact_info]

css:
  theme: professional
  colors:
    primary: "#1a73e8"
    success: "#34a853"
    warning: "#fbbc04"
    danger: "#ea4335"

filters:
  issues:
    - severity: critical
    - severity: warning
  suggestions:
    - priority: high
```

### 报告自动发送

支持将报告自动发送到指定渠道：

```bash
/harness-report --auto-send email
/harness-report --auto-send slack
/harness-report --auto-send wechat
```

#### 配置自动发送

```yaml
# .harness/report-config.yaml
auto_send:
  enabled: true
  
  # 发送频率
  schedule:
    type: weekly
    day: friday
    time: "17:00"
    
  # 发送渠道
  channels:
    - type: email
      recipients:
        - "team@example.com"
        - "manager@example.com"
      subject: "项目质量周报 - {date}"
      
    - type: slack
      webhook: "https://hooks.slack.com/services/xxx"
      channel: "#quality-reports"
      
    - type: wechat
      webhook: "https://qyapi.weixin.qq.com/cgi-bin/webhook/send?key=xxx"
      chat_id: "xxx"
  
  # 报告内容
  report:
    template: default
    format: html
    include_attachments: true
```

#### 定时报告

```bash
# 每周五 17:00 自动生成并发送报告
/harness-report --schedule weekly --day friday --time 17:00

# 每天 09:00 生成日报
/harness-report --schedule daily --time 09:00

# 每月 1 号生成月报
/harness-report --schedule monthly --day 1 --time 09:00
```

### 报告存储和历史

```yaml
# .harness/reports/
# ├── 2026-06/
# │   ├── 2026-06-17/
# │   │   ├── daily-report-2026-06-17.html
# │   │   └── daily-report-2026-06-17.json
# │   └── 2026-06-30/
# │       ├── weekly-report-2026-06-30.html
# │       └── weekly-report-2026-06-30.json
# └── 2026-07/

# 报告索引
reports-index.yaml:
  - id: "rpt-20260617-001"
    type: daily
    date: "2026-06-17"
    filename: "daily-report-2026-06-17.html"
    score: 92
    issues_count: 3
    
  - id: "rpt-20260630-001"
    type: weekly
    date: "2026-06-30"
    filename: "weekly-report-2026-06-30.html"
    score: 90
    issues_count: 5
```

### 完整命令语法

```bash
/harness-report [options]

选项:
  --format <format>       输出格式: html|pdf|markdown|json (默认: html)
  --output <path>         输出路径
  --template <name>       模板名称或路径
  --compare <projects>    对比项目列表（逗号分隔）
  --trend                 包含趋势分析
  --auto-send <channel>   自动发送到指定渠道
  --schedule <type>       设置定时报告: daily|weekly|monthly
  --day <day>             定时报告日期（weekly: 1-7, monthly: 1-31）
  --time <time>           定时报告时间（HH:MM）
  --history               查看报告历史
  --latest                生成最新报告（使用上次配置）
```

---

## 版本历史

- v3.7.2 (2026-06-17): 增强报告功能，支持多项目对比、自定义模板、自动发送、报告模板库
- v3.6.1 (2026-06-16): 添加报告生成功能（/harness-report、HTML/PDF 格式、质量评分、趋势分析、定期报告）
- v2.2.0 (2026-06-15): 增加自动化检查脚本指引（Python/Node.js/Go）、与 SmartMate 验证协议集成
- v2.1.0: 初始版本
