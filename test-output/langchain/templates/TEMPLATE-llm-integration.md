---
description: LLM 集成开发模板
version: 1.0.0
auto_generated: true
generated_by: your-harness
---

# LLM 集成开发模板

本模板用于 LangChain 项目中的 LLM 集成功能开发。

---

## 阶段 1: 需求分析

### 1.1 明确集成目标

**Input**
- 要集成的模型/工具
- 目标功能

**Process**
1. 确定要集成的模型（OpenAI/Claude/本地模型）
2. 分析模型的 API 特性
3. 确定集成方式（API/SDK/本地）
4. 评估性能要求
5. 确定成本影响

**Output**
- 集成目标
- API 分析
- 集成方案
- 成本估算

### 1.2 依赖分析

**Input**
- 目标模型
- 现有技术栈

**Process**
1. 确定需要的依赖库
2. 检查版本兼容性
3. 评估许可证
4. 测试依赖安装

**Output**
- 依赖清单
- 安装命令
- 兼容性报告

---

## 阶段 2: 设计

### 2.1 模型抽象设计

**Input**
- 集成目标

**Process**
1. 设计统一的模型接口
2. 定义请求/响应格式
3. 规划错误处理
4. 设计重试机制
5. 考虑流式输出

**Output**
- 接口定义
- 数据结构
- 错误码定义

**设计原则**:
- 遵循 LangChain 的 BaseChatModel 抽象
- 实现 _generate 和 _stream 方法
- 使用 ChatResult 和 ChatGenerationChunk

### 2.2 Prompt 设计（如需要）

**Input**
- 功能需求

**Process**
1. 设计 Prompt 模板
2. 定义变量
3. 选择消息格式
4. 测试 Prompt 效果

**Output**
- Prompt 模板
- 变量定义
- 测试结果

### 2.3 集成点设计

**Input**
- 现有架构

**Process**
1. 确定集成点
2. 设计适配器
3. 规划扩展点
4. 考虑缓存策略

**Output**
- 集成架构
- 适配器设计
- 扩展点定义

---

## 阶段 3: 实现

### 3.1 模型客户端实现

**Input**
- 设计文档

**Process**
1. 创建模型类
2. 实现初始化方法
3. 实现 _generate 方法
4. 实现 _stream 方法
5. 添加错误处理

**Run**
```bash
# 创建模型文件
touch libs/langchain/src/langchain_core/chat_models/{{model_name}}.py

# 实现模型类
cat > libs/langchain/src/langchain_core/chat_models/{{model_name}}.py << 'EOF'
from typing import Any, Dict, List, Optional, Sequence, TypeVar

from langchain_core.callbacks import CallbackManagerForLLMRun
from langchain_core.messages import BaseMessage, AIMessage
from langchain_core.outputs import ChatResult, ChatGenerationChunk
from langchain_core.utils import get_from_dict_or_env

from ..base_chat_model import BaseChatModel

T = TypeVar("T", bound="{{ModelName}}")

class {{ModelName}}(BaseChatModel):
    """{{model_description}}"""
    
    # 模型配置
    model_name: str
    api_key: Optional[str] = None
    base_url: Optional[str] = None
    temperature: float = 0.7
    max_tokens: Optional[int] = None
    
    # 类属性
    model_family: str = "{{model_family}}"
    
    @property
    def _llm_type(self) -> str:
        return "{{model_name}}"
    
    def _generate(
        self,
        messages: List[BaseMessage],
        stop: Optional[List[str]] = None,
        run_manager: Optional[CallbackManagerForLLMRun] = None,
        **kwargs: Any,
    ) -> ChatResult:
        """生成响应"""
        # 1. 转换消息格式
        formatted_messages = self._format_messages(messages)
        
        # 2. 调用 API
        response = self._call_api(formatted_messages, stop, **kwargs)
        
        # 3. 解析响应
        return self._parse_response(response)
    
    def _stream(
        self,
        messages: List[BaseMessage],
        stop: Optional[List[str]] = None,
        run_manager: Optional[CallbackManagerForLLMRun] = None,
        **kwargs: Any,
    ) -> ChatGenerationChunk:
        """流式生成"""
        # 1. 转换消息格式
        formatted_messages = self._format_messages(messages)
        
        # 2. 流式调用 API
        for chunk in self._stream_api(formatted_messages, stop, **kwargs):
            yield self._parse_stream_chunk(chunk)
    
    def _format_messages(self, messages: List[BaseMessage]) -> List[Dict[str, str]]:
        """格式化消息"""
        # 实现消息格式转换
        pass
    
    def _call_api(self, messages: List[Dict], stop: Optional[List[str]], **kwargs) -> Dict:
        """调用 API"""
        # 实现 API 调用
        pass
    
    def _parse_response(self, response: Dict) -> ChatResult:
        """解析响应"""
        # 实现响应解析
        pass
    
    def _stream_api(self, messages: List[Dict], stop: Optional[List[str]], **kwargs):
        """流式 API 调用"""
        # 实现流式 API 调用
        pass
    
    def _parse_stream_chunk(self, chunk: Dict) -> ChatGenerationChunk:
        """解析流式块"""
        # 实现流式块解析
        pass
EOF
```

**Expected**
- 模型文件创建成功
- 继承 BaseChatModel
- 实现 _generate 和 _stream 方法
- 代码通过类型检查

### 3.2 集成测试

**Input**
- 模型实现
- 测试用例

**Process**
1. 编写单元测试
2. 编写集成测试
3. 测试 Prompt 效果
4. 验证流式输出

**Run**
```bash
# 运行测试
cd libs/langchain && pytest tests/unit_tests/chat_models/test_{{model_name}}.py -v

# 测试覆盖率
cd libs/langchain && pytest --cov=src/langchain_core/chat_models/{{model_name}} tests/unit_tests/chat_models/test_{{model_name}}.py
```

**Expected**
- 所有测试通过
- 覆盖率 > 80%

---

## 阶段 4: 验证

### 4.1 功能验证

**Input**
- 模型实现
- 测试场景

**Process**
1. 执行端到端测试
2. 验证输出正确性
3. 测试边界条件
4. 验证错误处理

**Run**
```bash
# 端到端测试
cd libs/langchain && pytest tests/integration_tests/test_{{model_name}}.py -v
```

**Expected**
- 所有场景通过
- 输出符合预期格式

### 4.2 兼容性验证

**Input**
- 模型实现

**Process**
1. 测试不同模型版本
2. 测试不同输入格式
3. 测试不同参数组合
4. 验证与现有代码兼容

**Run**
```bash
# 兼容性测试
cd libs/langchain && pytest tests/compatibility/test_{{model_name}}.py -v
```

**Expected**
- 所有版本兼容
- 无破坏性变更

### 4.3 性能验证

**Input**
- 模型实现

**Process**
1. 测试响应时间
2. 测试并发处理
3. 验证资源使用
4. 优化瓶颈

**Run**
```bash
# 性能测试
cd libs/langchain && pytest tests/performance/test_{{model_name}}.py -v
```

**Expected**
- 响应时间 < 阈值
- 无内存泄漏

---

## 阶段 5: 文档

### 5.1 API 文档

**Input**
- 模型实现

**Process**
1. 更新 API 文档
2. 添加使用示例
3. 说明参数和返回值
4. 记录错误码

**Output**
- API 文档
- 示例代码

### 5.2 集成文档

**Input**
- 集成方式

**Process**
1. 编写集成指南
2. 添加配置说明
3. 说明依赖要求
4. 记录常见问题

**Output**
- 集成指南
- 配置说明
- FAQ

---

## 闸门检查点

### G0: 需求确认
- [ ] 集成目标明确
- [ ] API 分析完成
- [ ] 依赖清单确认

### G1: 设计确认
- [ ] 接口设计完成
- [ ] 集成点确定
- [ ] 设计评审通过

### G2: 实现确认
- [ ] 模型类实现完成
- [ ] 测试通过
- [ ] 代码审查通过

### G3: 验证确认
- [ ] 功能验证通过
- [ ] 兼容性验证通过
- [ ] 性能达标

### G4: 文档确认
- [ ] API 文档完成
- [ ] 集成文档完成
- [ ] 示例代码完成

---

## 注意事项

1. **API Key 安全**:
   - 使用环境变量
   - 不在代码中硬编码
   - 支持多种配置方式

2. **成本控制**:
   - 监控 API 调用成本
   - 设置用量上限
   - 优化 Prompt 减少 token 消耗

3. **错误处理**:
   - 处理 API 限流
   - 实现重试机制
   - 记录错误日志

4. **版本兼容**:
   - 锁定依赖版本
   - 测试模型更新
   - 提供降级方案

5. **LangChain 规范**:
   - 遵循 BaseChatModel 接口
   - 使用标准输出格式
   - 支持回调机制
