# Your Harness 项目优化方案

## 问题分析

当前 Your Harness 的核心问题是：**生成的产物是"通用模板"，而不是"项目定制化产物"**。

### 现状

```
harness-designer 的工作方式：
1. 接收 harness-archaeology 的项目特征
2. 根据特征"选择"预定义的模板
3. 输出的是"模板的组合"，不是"为项目定制的内容"

问题：
- Template 是静态的，没有根据项目特征动态生成
- Reference 是通用的，没有项目特定的内容
- Script 只有框架，没有根据项目特征填充实现
- Constitution 是静态的，没有根据项目特征生成规则
```

### 目标

```
让 Your Harness 能够：
1. 根据项目特征动态生成 Template（不是选择预定义模板）
2. 根据项目特征动态生成 Reference（包含项目特定的规则）
3. 根据项目特征动态生成 Script（填充具体的命令和路径）
4. 根据项目特征动态生成 Constitution（生成项目特定的约束）
```

---

## 核心优化方向

### 1. 建立"特征 → 产物"的映射知识库

**问题**: 当前 harness-designer 没有"什么样的项目特征应该生成什么样的产物"的知识。

**解决方案**: 建立映射知识库，让 AI 能够根据特征推导产物。

```yaml
# knowledge/project-patterns.yaml

patterns:
  
  protocol-driven:
    signals:
      - exists: ["*.proto", ".tad/build/dbsql-*"]
      - has: ["cgi", "dbproxy", "yunapi"]
    generates:
      templates:
        - TEMPLATE-protocol-change.md
        - TEMPLATE-dbproxy-sync.md
        - TEMPLATE-yunapi-registration.md
      references:
        - gate-protocol.md (with protocol-chain gates)
        - dbproxy-mapping-rules.md
      scripts:
        - check-dbproxy-mapping.py
        - check-api-compatibility.py
      constitution:
        - protocol-chain-must-follow-order
        - yunapi-registration-required
        - tag-sync-after-proto-change
    
  ai-llm-project:
    signals:
      - exists: ["*.env.example", "llm_config.*", "prompts/"]
      - has: ["openai", "anthropic", "langchain", "llama"]
    generates:
      templates:
        - TEMPLATE-model-integration.md
        - TEMPLATE-prompt-engineering.md
        - TEMPLATE-api-key-management.md
      references:
        - ai-safety-checklist.md
        - cost-estimation-protocol.md
      scripts:
        - check-api-key-rotation.py
        - check-cost-estimation.py
      constitution:
        - api-key-must-be-encrypted
        - prompt-must-have-safety-guards
        - cost-must-be-estimated-before-deploy
        
  multi-repo:
    signals:
      - multiple git roots
      - has: ["go.mod", "package.json", "pyproject.toml"] (multiple)
    generates:
      templates:
        - TEMPLATE-cross-repo-change.md
      references:
        - cross-repo-coordination.md
        - dependency-sync-protocol.md
      scripts:
        - check-cross-repo-dependency.py
      constitution:
        - cross-repo-change-must-sync
        - version-must-be-aligned
```

### 2. 建立"产物生成模板"（元模板）

**问题**: 当前的 Template 是"最终产物"，而不是"生成 Template 的模板"。

**解决方案**: 建立"元模板"——用于生成 Template 的模板。

```markdown
# meta-template/TEMPLATE-protocol-change.md.tmpl

# {{feature_name}} 协议变更 Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use `executing-plans` to implement this plan task-by-task.

**Goal:** {{goal_description}}

**Architecture:** 遵循 {{project_name}} 的 {{protocol_chain_stages}} 阶段变更链路。

## 变更范围自检清单

{{#if has_database_change}}
- [ ] 是否需要改数据库结构？
{{/if}}

{{#if has_proto_change}}
- [ ] 是否新增/修改 RPC 或 Message？
{{/if}}

{{#if has_downstream_sync}}
- [ ] 下游服务是否需要同步？
{{/if}}

---

{{#each protocol_chain_stages}}
## Task {{@index}}: {{this.name}}

**Files:**
{{#each this.files}}
- 查看/修改: `{{this}}`
{{/each}}

**Step 1: {{this.step_1_name}}**

Run:
```bash
{{this.step_1_command}}
```

Expected: {{this.step_1_expected}}

{{#if this.has_mcp_call}}
**Step 2: {{this.step_2_name}}（{{this.mcp_tool}} MCP）**

调用 `{{this.mcp_tool}}.{{this.mcp_method}}`：
```json
{{this.mcp_payload}}
```
{{/if}}

{{#if this.has_manual_step}}
**Step 3: {{this.step_3_name}}（人工操作）**

{{this.manual_instructions}}
{{/if}}

---
{{/each}}

## 完工检查清单

{{#each completion_checklist}}
- [ ] {{this}}
{{/each}}
```

### 3. 建立"脚本生成模板"（元模板）

**问题**: 当前的 Script 只有框架，没有实现。

**解决方案**: 建立"脚本生成元模板"，根据项目特征填充具体实现。

```python
# meta-template/scripts/check-gates.py.tmpl

#!/usr/bin/env python3
"""
闸门状态检查脚本
项目: {{project_name}}
检查特定闸门是否可以通过
"""
import json
import sys
from pathlib import Path

# 项目特定的闸门配置
GATES = {
    "G0_CLARIFICATION": {
        "stage": "需求澄清",
        "artifacts": [
            {{#if has_spec_dir}}
            "docs/spec/<feature>/analyze/00-clarifications.md",
            {{else}}
            "docs/<feature>/clarifications.md",
            {{/if}}
        ],
        "checks": [
            "需求已澄清",
            {{#if has_security_surface}}
            "安全影响已评估",
            {{/if}}
        ]
    },
    {{#each additional_gates}}
    "{{this.id}}": {
        "stage": "{{this.stage}}",
        "artifacts": {{this.artifacts}},
        "checks": {{this.checks}}
    },
    {{/each}}
}

def check_gate(feature: str, gate_id: str) -> dict:
    """检查闸门状态"""
    gate_file = Path(".harness/workflow-gates/{{gate_state_filename}}")
    
    # 项目特定的检查逻辑
    {{#if has_custom_gate_check}}
    {{custom_gate_check_logic}}
    {{else}}
    # 默认检查逻辑
    if not gate_file.exists():
        return {"status": "BLOCKED", "reason": "Gate state file not found"}
    {{/if}}
    
    with open(gate_file) as f:
        gate_state = json.load(f)
    
    gate = gate_state.get(gate_id)
    if not gate:
        return {"status": "BLOCKED", "reason": f"Gate {gate_id} not found"}
    
    if gate.get("status") != "approved":
        return {"status": "BLOCKED", "reason": "Gate not approved", "current": gate.get("status")}
    
    return {"status": "PASS", "approved_by": gate.get("approved_by")}

def main():
    import argparse
    parser = argparse.ArgumentParser()
    parser.add_argument("action", choices=["can-advance", "status"])
    parser.add_argument("--feature", required=True)
    parser.add_argument("--gate", required=True)
    args = parser.parse_args()
    
    result = check_gate(args.feature, args.gate)
    
    if result["status"] == "PASS":
        print(f"✅ Gate {args.gate} passed")
        sys.exit(0)
    else:
        print(f"❌ Gate {args.gate} blocked: {result['reason']}")
        sys.exit(1)

if __name__ == "__main__":
    main()
```

### 4. 建立"Reference 生成模板"（元模板）

**问题**: 当前的 Reference 是静态的，没有项目特定内容。

**解决方案**: 建立"Reference 生成元模板"，根据项目特征填充具体内容。

```markdown
# meta-template/references/gate-protocol.md.tmpl

# Gate Protocol

## Purpose

定义 {{project_name}} 的闸门协议，确保关键节点有人工确认。

## 闸门定义

基于项目特征，本项目需要以下闸门：

{{#if has_protocol_chain}}
### 协议链路专用闸门

| Gate | 阶段 | 必须产物 | 继续条件 |
|------|------|----------|----------|
| `G1_PROTO` | Proto 更新后 | proto/*.proto | 编译通过 |
| `G2_TAG` | Tag 同步后 | tag-sync-log.md | 工蜂 tag 已创建 |
| `G3_CGI` | CGI 更新后 | cgi/apis/*.go | 编译通过 |
| `G4_YUNAPI` | 云 API 注册后 | yunapi-sync-log.md | API 已注册 |
{{/if}}

{{#if has_ai_pipeline}}
### AI Pipeline 专用闸门

| Gate | 阶段 | 必须产物 | 继续条件 |
|------|------|----------|----------|
| `G1_MODEL` | 模型集成后 | model-config.yaml | 连接测试通过 |
| `G2_PROMPT` | Prompt 设计后 | prompts/*.txt | 安全检查通过 |
| `G3_COST` | 成本估算后 | cost-estimation.md | 预算审批通过 |
{{/if}}

### 通用闸门

| Gate | 阶段 | 必须产物 | 继续条件 |
|------|------|----------|----------|
| `G0_CLARIFICATION` | 需求澄清后 | clarifications.md | 用户确认需求 |
| `G1_PRD` | PRD 完成后 | prd.md | 用户审批 PRD |
| `G2_TASKS` | 任务拆解后 | tasks.md | 用户审批任务 |
| `G3_SLICE` | 每个代码切片后 | develop/3X-*.md | 用户确认切片 |
| `G4_VERIFY` | 验证完成后 | test-report.md | 测试通过 |
| `G5_REVIEW` | 审查后 | review/*.md | 无阻塞问题 |
| `G6_SHIP` | 上线前 | ship-checklist.md | 用户审批上线 |

## Universal Gate Output

每个闸门必须使用以下格式：

```markdown
## Gate: <GATE_ID>

**状态**：WAITING_USER_APPROVAL

**本阶段产物**
{{#each required_artifacts}}
- {{this}}
{{/each}}

**通过标准**
{{#each checks}}
- [ ] {{this}}
{{/each}}

**需要用户确认**
1. 通过，进入下一阶段
2. 有问题，按反馈修改
3. 暂停，不继续
```

## Stop Rules

- 不得在没有显式用户批准的情况下跨越阶段闸门
{{#if has_strict_security_requirements}}
- 安全敏感操作必须经过安全审查
{{/if}}
{{#if has_multi_repo}}
- 跨仓库变更必须同步所有相关仓库
{{/if}}
```

### 5. 建立"知识积累"机制

**问题**: 每次生成都是从零开始，没有学习能力。

**解决方案**: 建立知识积累机制，让 Your Harness 能够从历史生成中学习。

```yaml
# knowledge/learnings.yaml

learnings:
  
  smartmate-project:
    date: "2026-06-18"
    project: smartmate
    what_worked:
      - protocol-chain 识别准确
      - 闸门定义完整
    what_didnt_work:
      - 脚本只有框架，没有实现
      - Reference 不够详细
    improvements:
      - 增加元模板系统
      - 建立特征-产物映射知识库
      
  nextjs-project:
    date: "2026-06-15"
    project: nextjs-app
    what_worked:
      - 正确识别为纯前端项目
      - 生成了轻量级 Workflow
    what_didnt_work:
      - 没有识别到 API 调用后端
      - 缺少前后端协同流程
    improvements:
      - 增加前后端协同检测
```

---

## 具体实施计划

### Phase 1: 建立元模板系统（1 周）

**目标**: 让 harness-designer 能够根据项目特征填充模板内容。

**任务**:

1. **创建 meta-template 目录结构**

```bash
your-harness/
├── skills/
├── meta-templates/
│   ├── templates/
│   │   ├── TEMPLATE-protocol-change.md.tmpl
│   │   ├── TEMPLATE-dbproxy-sync.md.tmpl
│   │   └── ...
│   ├── scripts/
│   │   ├── check-gates.py.tmpl
│   │   ├── workflow-preflight.py.tmpl
│   │   └── ...
│   ├── references/
│   │   ├── gate-protocol.md.tmpl
│   │   ├── clarification-loop.md.tmpl
│   │   └── ...
│   └── skills/
│       └── SKILL.md.tmpl
└── knowledge/
    ├── project-patterns.yaml
    └── feature-to-product-mapping.yaml
```

2. **定义模板变量规范**

```yaml
# 模板变量规范
variables:
  project_name: string
  protocol_chain_stages: array
  has_database_change: boolean
  has_proto_change: boolean
  has_security_surface: boolean
  gate_definitions: array
  script_commands: array
  # ...
```

3. **更新 harness-designer**

在 harness-designer 中添加模板渲染逻辑：

```markdown
## 模板渲染流程

1. 从 harness-archaeology 获取项目特征
2. 根据特征从 knowledge/project-patterns.yaml 匹配模式
3. 加载对应的元模板
4. 填充模板变量
5. 输出最终产物
```

### Phase 2: 建立特征-产物映射知识库（1 周）

**目标**: 让 harness-designer 知道"什么样的项目特征应该生成什么样的产物"。

**任务**:

1. **分析 SmartMate 项目的特征和产物**

```yaml
# 从 SmartMate 项目提取的映射
smartmate:
  features:
    - protocol-chain
    - multi-repo
    - ai-pipeline
  products:
    templates:
      - TEMPLATE-agent-skills.md
      - TEMPLATE-dbproxy-change.md
      - TEMPLATE-agent-skills-reverse.md
    scripts:
      - workflow-preflight.py
      - check-dbproxy-mapping.py
      - check-api-compatibility.py
    references:
      - gate-protocol.md
      - dbproxy-change-template-contract.md
      - hard-constraints-constitution.md
```

2. **建立通用映射规则**

```yaml
# 从 SmartMate 映射推导出的通用规则
rules:
  - if: has_protocol_chain
    generates:
      - TEMPLATE-protocol-change.md
      - check-dbproxy-mapping.py
      - dbproxy-change-template-contract.md
  
  - if: has_multi_repo
    generates:
      - TEMPLATE-cross-repo-change.md
      - cross-repo-coordination.md
      - check-cross-repo-dependency.py
  
  - if: has_ai_pipeline
    generates:
      - TEMPLATE-model-integration.md
      - ai-safety-checklist.md
      - check-api-key-rotation.py
```

3. **创建 knowledge/project-patterns.yaml**

将映射规则整理成知识库文件。

### Phase 3: 实现动态生成逻辑（2 周）

**目标**: 让 harness-designer 能够动态生成产物，而不是选择预定义模板。

**任务**:

1. **更新 harness-designer.md**

添加动态生成的详细流程：

```markdown
## 动态生成流程

### Step 1: 特征收集

从 harness-archaeology 获取：
- 技术栈特征
- 业务特征
- 特殊流程
- 质量要求

### Step 2: 模式匹配

在 knowledge/project-patterns.yaml 中查找匹配的模式：
- 精确匹配：所有信号都符合
- 部分匹配：主要信号符合
- 无匹配：使用默认模式

### Step 3: 产物规划

根据匹配的模式，确定要生成的产物：
- Templates
- References
- Scripts
- Constitution

### Step 4: 模板渲染

对每个产物：
1. 加载对应的元模板
2. 从项目特征提取变量值
3. 渲染模板
4. 输出最终产物

### Step 5: 质量检查

验证生成的产物：
- 没有未填充的变量
- 产物结构完整
- 产物之间引用正确
```

2. **实现模板渲染器**

创建一个简单的模板渲染器（可以用 Python 的 string.Template 或 Jinja2）：

```python
# utils/template-renderer.py
def render_template(template_path: str, variables: dict) -> str:
    """渲染模板"""
    with open(template_path) as f:
        template = f.read()
    
    # 简单的变量替换
    for key, value in variables.items():
        if isinstance(value, str):
            template = template.replace("{{" + key + "}}", value)
        elif isinstance(value, list):
            # 处理循环
            template = render_loop(template, key, value)
        elif isinstance(value, dict):
            # 处理条件
            template = render_condition(template, key, value)
    
    return template
```

### Phase 4: 建立知识积累机制（1 周）

**目标**: 让 Your Harness 能够从历史生成中学习，持续改进。

**任务**:

1. **添加生成反馈收集**

在 harness-designer 中添加：

```markdown
## 生成后反馈

生成完成后，询问用户：

1. 生成的产物是否符合预期？
2. 有哪些需要改进的地方？
3. 是否需要补充其他产物？

将反馈记录到 knowledge/learnings.yaml
```

2. **实现知识更新逻辑**

```python
# utils/knowledge-updater.py
def update_knowledge(project_name: str, feedback: dict):
    """更新知识库"""
    learnings = load_learnings()
    
    learnings[project_name] = {
        "date": datetime.now().isoformat(),
        "feedback": feedback,
        "improvements": derive_improvements(feedback)
    }
    
    save_learnings(learnings)
```

3. **实现持续改进**

在下次生成时，参考历史学习：

```markdown
## 知识应用

生成前，检查 knowledge/learnings.yaml：
- 是否有类似项目的生成经验？
- 有哪些已知的问题和改进？
- 应用这些知识到当前生成中
```

---

## 成功指标

### Phase 1 成功指标

- [ ] meta-template 目录结构创建完成
- [ ] 至少 3 个元模板定义完成
- [ ] harness-designer 能够加载并渲染元模板

### Phase 2 成功指标

- [ ] knowledge/project-patterns.yaml 创建完成
- [ ] 至少 5 个项目模式定义完成
- [ ] 模式匹配逻辑实现完成

### Phase 3 成功指标

- [ ] harness-designer 能够动态生成产物
- [ ] 生成的产物包含项目特定内容（不是通用模板）
- [ ] 用 SmartMate 项目测试，生成质量提升 30%

### Phase 4 成功指标

- [ ] 知识积累机制实现完成
- [ ] 能够从用户反馈中学习
- [ ] 生成质量持续改进

---

## 总结

本次优化的核心思路是：

1. **从"选择模板"到"生成模板"**
   - 建立元模板系统
   - 根据项目特征动态填充

2. **从"静态知识"到"动态知识"**
   - 建立特征-产物映射知识库
   - 建立知识积累机制

3. **从"通用产物"到"定制产物"**
   - 每个产物都包含项目特定内容
   - 不是简单地复制预定义模板

通过这三步优化，Your Harness 将能够真正地"根据项目生成契合的 Template、流程约束和 Reference"，而不是输出"通用模板的组合"。
