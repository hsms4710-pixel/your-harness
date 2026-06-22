---
capability: learning-feedback
name: 学习反馈
version: 1.0.0
description: |
  教 AI 如何从每次生成中学习，持续改进。
  
  核心思想：每次使用都是一次学习机会，知识库越用越丰富。
---

# 元能力 5: 学习反馈

## 核心目标

从每次流程生成中学习，持续改进知识库和推理规则。

## 学习流程

### Step 1: 收集生成结果

```yaml
collection:
  # 在生成完成后收集
  trigger: "flow_generation_completed"
  
  collect:
    # 生成的产物
    generated_artifacts:
      - skill_md: "生成的 SKILL.md"
      - templates: "生成的模板列表"
      - scripts: "生成的脚本列表"
      - constitution: "生成的宪法模型"
      
    # 生成过程信息
    generation_info:
      - project_features: "识别的项目特征"
      - matched_rules: "匹配的推理规则"
      - composition_strategy: "使用的组合策略"
      - knowledge_sources: "引用的知识来源"
      
    # 时间戳
    timestamp: "生成完成时间"
```

### Step 2: 收集用户反馈

```yaml
feedback_collection:
  # 主动询问用户
  questions:
    - question: "生成的流程是否满足您的需求？"
      options: ["完全满足", "基本满足", "部分满足", "不满足"]
      
    - question: "哪些部分需要调整？"
      options: ["阶段划分", "检查项", "闸门设置", "脚本功能", "其他"]
      allow_multiple: true
      
    - question: "是否有遗漏的重要阶段？"
      type: "open_ended"
      
    - question: "是否有多余的阶段？"
      type: "open_ended"
      
    - question: "整体流程的复杂度如何？"
      options: ["太简单", "适中", "太复杂"]
      
  # 记录用户的修改
  modification_tracking:
    if user_modifies_generated_flow:
      record:
        - original_content
        - modified_content
        - modification_reason
```

### Step 3: 分析结果

```yaml
analysis:
  # 识别成功模式
  identify_success_patterns:
    condition: "user_feedback == 'satisfied' OR user_modifications == minimal"
    action: |
      - 记录项目特征 → 流程映射
      - 提取可复用的模式
      - 更新成功案例库
      
  # 识别失败模式
  identify_failure_patterns:
    condition: "user_feedback == 'unsatisfied' OR user_modifications == significant"
    action: |
      - 分析失败原因
      - 识别知识库的不足
      - 标记需要改进的规则
      
  # 提取学习点
  extract_learnings:
    from_success:
      - "哪些推理是准确的"
      - "哪些知识组合是有效的"
      - "用户的偏好设置"
      
    from_failure:
      - "哪些推理需要修正"
      - "知识库缺少什么"
      - "用户期望的是什么"
```

### Step 4: 更新知识库

```yaml
knowledge_update:
  # 添加新的项目特征 → 流程映射
  add_feature_flow_mapping:
    condition: "new_pattern_identified"
    action: |
      添加到 knowledge/feature-to-product-mapping.yaml
      
  # 修正错误的推理规则
  correct_reasoning_rules:
    condition: "rule_produced_wrong_result"
    action: |
      更新 knowledge/reasoning-rules.yaml
      调整规则的触发条件或输出
      
  # 提升成功的模式为最佳实践
  promote_to_best_practice:
    condition: "pattern_used_successfully >= N_times AND user_satisfaction >= threshold"
    action: |
      移动到 knowledge/best-practices/
      标记为推荐模板
      
  # 添加新的成功案例
  add_success_case:
    condition: "flow_completed_successfully AND user_feedback == positive"
    action: |
      添加到 knowledge/success-cases/
      记录项目特征和流程
```

## 反馈数据结构

### 学习记录

```yaml
learning_record:
  id: "LR-2026-06-22-001"
  timestamp: "2026-06-22T15:30:00Z"
  
  # 项目信息
  project:
    name: "my-ai-app"
    path: "/path/to/project"
    
  # 识别的特征
  identified_features:
    - name: "ai-llm-project"
      confidence: "high"
      
    - name: "security-sensitive"
      confidence: "medium"
      
  # 生成结果
  generation_result:
    strategy_used: "feature-decomposition"
    stages_generated: 8
    templates_generated: 3
    scripts_generated: 4
    
  # 用户反馈
  user_feedback:
    overall_satisfaction: "satisfied"
    satisfaction_score: 4  # 1-5
    
    stage_division:
      rating: "good"
      modifications: []
      
    checks_and_gates:
      rating: "good"
      modifications: ["添加了性能检查"]
      
    complexity:
      rating: "appropriate"
      modifications: []
      
    comments:
      - "流程很清晰"
      - "建议添加成本追踪阶段"
      
  # 用户修改
  modifications:
    - type: "add_stage"
      content: "添加成本追踪阶段"
      reason: "需要持续监控 API 成本"
      
    - type: "add_check"
      content: "添加性能检查"
      reason: "需要确保响应时间"
      
  # 分析结果
  analysis:
    success_patterns:
      - "AI 项目特征识别准确"
      - "安全相关阶段完整"
      
    failure_patterns: []
    
    learnings:
      - "AI 项目需要成本追踪阶段"
      - "性能检查是常见需求"
      
  # 知识库更新
  knowledge_updates:
    - type: "add_feature"
      feature: "cost-tracking-required"
      trigger: "has_ai_integration AND has_cost_concerns"
      
    - type: "add_check"
      check: "性能检查"
      applicable_when: "has_performance_requirements"
```

## 知识提升机制

### 提升规则

```yaml
promotion_rules:
  # 规则 1: 使用频率
  rule_1_usage_frequency:
    condition:
      - learning_record.count(feature) >= 5
      - average_satisfaction >= 4
    action: "promote_to_best_practice"
    
  # 规则 2: 质量评分
  rule_2_quality_score:
    condition:
      - average_satisfaction >= 4.5
      - no_user_modifications
    action: "promote_to_best_practice"
    
  # 规则 3: 用户推荐
  rule_3_user_recommendation:
    condition:
      - user_marked_as_recommendation == true
    action: "promote_to_best_practice"
    
  # 规则 4: 时间衰减
  rule_4_time_decay:
    condition:
      - last_used > 30_days_ago
      - no_recent_modifications
    action: "demote_or_archive"
```

### 提升流程

```yaml
promotion_process:
  # 从学习记录中筛选候选
  select_candidates:
    from: all_learning_records
    where:
      - satisfaction >= 4
      - modification_count <= 1
      
  # 聚类相似的记录
  cluster_similar_records:
    method: "feature_vector_similarity"
    threshold: 0.8
    
  # 为每个聚类生成最佳实践
  generate_best_practice:
    for cluster in clusters:
      best_practice = {
        id: "BP-{cluster_id}"
        name: "从 {cluster.size} 个项目中提取"
        features: cluster.common_features
        flow: cluster.most_common_flow
        satisfaction: cluster.average_satisfaction
        source_records: cluster.record_ids
        }
        
  # 添加到最佳实践库
  add_to_best_practices:
    path: "knowledge/best-practices/BP-{id}.yaml"
    content: best_practice
```

## 知识库更新脚本

### 反馈收集脚本

```python
#!/usr/bin/env python3
"""
反馈收集脚本
在流程生成完成后运行，收集用户反馈
"""
import json
import yaml
from pathlib import Path
from datetime import datetime

class FeedbackCollector:
    def __init__(self, project_path: str):
        self.project_path = project_path
        self.feedback_file = Path(f"{project_path}/.harness/feedback.json")
        
    def collect(self) -> dict:
        """收集用户反馈"""
        feedback = {
            "timestamp": datetime.now().isoformat(),
            "overall_satisfaction": self._ask_satisfaction(),
            "stage_division": self._ask_stage_division(),
            "checks_and_gates": self._ask_checks_and_gates(),
            "complexity": self._ask_complexity(),
            "comments": self._ask_comments(),
            "modifications": self._get_modifications(),
        }
        
        return feedback
        
    def save(self, feedback: dict):
        """保存反馈"""
        self.feedback_file.parent.mkdir(parents=True, exist_ok=True)
        with open(self.feedback_file, "w") as f:
            json.dump(feedback, f, indent=2)
            
    def _ask_satisfaction(self) -> str:
        """询问满意度"""
        print("生成的流程是否满足您的需求？")
        options = ["1. 完全满足", "2. 基本满足", "3. 部分满足", "4. 不满足"]
        for opt in options:
            print(opt)
        choice = input("> ")
        return options[int(choice)-1].split(". ")[1]
        
    # ... 其他询问方法
```

### 知识库更新脚本

```python
#!/usr/bin/env python3
"""
知识库更新脚本
从学习记录中提取知识，更新知识库
"""
import yaml
from pathlib import Path
from collections import defaultdict

class KnowledgeUpdater:
    def __init__(self, harness_path: str):
        self.harness_path = harness_path
        self.learnings_dir = Path(f"{harness_path}/knowledge/learnings")
        self.best_practices_dir = Path(f"{harness_path}/knowledge/best-practices")
        
    def analyze_learnings(self) -> dict:
        """分析所有学习记录"""
        learnings = []
        for file in self.learnings_dir.glob("*.yaml"):
            with open(file) as f:
                learnings.append(yaml.safe_load(f))
                
        # 按特征分组
        by_feature = defaultdict(list)
        for learning in learnings:
            for feature in learning["identified_features"]:
                by_feature[feature["name"]].append(learning)
                
        # 计算每个特征的成功率
        success_rates = {}
        for feature, records in by_feature.items():
            satisfied = sum(1 for r in records if r["user_feedback"]["overall_satisfaction"] == "satisfied")
            success_rates[feature] = satisfied / len(records)
            
        return {
            "total_records": len(learnings),
            "by_feature": dict(by_feature),
            "success_rates": success_rates,
        }
        
    def identify_candidates(self, analysis: dict) -> list:
        """识别可以提升为最佳实践的候选"""
        candidates = []
        
        for feature, records in analysis["by_feature"].items():
            if len(records) >= 5:  # 至少使用 5 次
                satisfaction = analysis["success_rates"].get(feature, 0)
                if satisfaction >= 0.8:  # 满意度 >= 80%
                    candidates.append({
                        "feature": feature,
                        "record_count": len(records),
                        "average_satisfaction": satisfaction,
                        "records": records,
                    })
                    
        return candidates
        
    def promote_to_best_practice(self, candidate: dict):
        """将候选提升为最佳实践"""
        # 从记录中提取最常见的流程
        flows = [r["generation_result"]["stages_generated"] for r in candidate["records"]]
        most_common_flow = max(set(flows), key=flows.count)
        
        best_practice = {
            "id": f"BP-{candidate['feature']}",
            "name": candidate["feature"],
            "description": f"从 {candidate['record_count']} 个项目中提取的最佳实践",
            "features": [candidate["feature"]],
            "recommended_stages": most_common_flow,
            "success_rate": candidate["average_satisfaction"],
            "source_record_count": candidate["record_count"],
        }
        
        # 保存
        self.best_practices_dir.mkdir(parents=True, exist_ok=True)
        bp_file = self.best_practices_dir / f"{best_practice['id']}.yaml"
        with open(bp_file, "w") as f:
            yaml.dump(best_practice, f)
            
        print(f"已提升为最佳实践: {best_practice['id']}")
```

## 与其他元能力的关系

```yaml
input_from:
  - capability: "project-analysis"
    provides: ["all_analysis_results"]
    
  - capability: "stage-reasoning"
    provides: ["all_reasoning_results"]
    
  - capability: "flow-generation"
    provides: ["generated_flows", "quality_metrics"]
    
  - capability: "knowledge-composition"
    provides: ["knowledge_usage_stats"]
    
output_to:
  - target: "knowledge/"
    provides: ["updated_knowledge_base", "new_best_practices"]
    
  - target: "meta-capabilities/"
    provides: ["improved_reasoning_rules"]
```

## 学习效果评估

### 评估指标

```yaml
evaluation_metrics:
  # 知识库增长
  knowledge_growth:
    - new_feature_mappings_added
    - new_best_practices_added
    - new_success_cases_added
    
  # 生成质量
  generation_quality:
    - average_user_satisfaction
    - average_modification_count
    - first_try_success_rate
    
  # 知识应用
  knowledge_application:
    - best_practice_usage_count
    - rule_accuracy_improvement
    - pattern_match_accuracy
```

### 趋势分析

```yaml
trend_analysis:
  # 按时间追踪
  track_over_time:
    metrics: ["satisfaction", "modifications", "knowledge_size"]
    intervals: ["weekly", "monthly"]
    
  # 识别改进
  identify_improvements:
    - "满意度是否提升"
    - "修改次数是否减少"
    - "知识库是否增长"
    
  # 生成报告
  generate_report:
    frequency: "monthly"
    content:
      - summary_statistics
      - trend_charts
      - recommendations
```

## 示例

### 输入（生成结果 + 用户反馈）

```yaml
generation_result:
  project: "my-ai-app"
  features: ["ai-llm-project", "security-sensitive"]
  stages: ["API分析", "安全设计", "实现", "安全审查"]
  satisfaction: 4
  
user_feedback:
  satisfaction: "satisfied"
  modifications: [
    { type: "add_stage", content: "添加成本追踪" }
  ]
  comments: ["流程很清晰", "建议添加成本监控"]
```

### 学习记录

```yaml
learning_record:
  id: "LR-2026-06-22-001"
  
  analysis:
    success_patterns:
      - "AI 项目特征识别准确"
      - "安全阶段完整"
      
    failure_patterns: []
    
    learnings:
      - "AI 项目通常需要成本追踪"
      
  knowledge_updates:
    - type: "add_feature"
      feature: "cost-tracking"
      trigger: "has_ai_integration"
```

### 知识库更新

```yaml
# 更新 knowledge/feature-to-product-mapping.yaml
new_entry:
  feature: "cost-tracking"
  trigger:
    conditions:
      - "has_ai_integration"
      - "has_api_key_config"
    confidence: "medium"
  generates:
    stages:
      - "成本追踪"
    checks:
      - "API 成本已估算"
      - "成本预警已设置"
```
