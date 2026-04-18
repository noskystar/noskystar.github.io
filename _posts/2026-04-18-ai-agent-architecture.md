---
layout: post
title: "AI Agent 架构设计模式：手机控制系统的工程实践"
date: 2026-04-18 08:00:00 +0800
categories: 技术架构
tags: [AI Agent, 架构设计, 系统工程, 手机自动化, 设计模式]
---

> 当大模型遇上手机控制，如何设计一个可扩展、可维护、高可用的 AI Agent 系统？本文从架构视角深入剖析手机控制 Agent 的设计模式、核心组件与工程实践，提供可直接落地的架构方案。

## 一、从脚本到 Agent：架构演进之路

### 1.1 传统自动化的局限

早期的手机自动化（如 Appium、uiautomator2）采用**脚本驱动**模式：

```python
# 传统脚本式自动化
d.click(100, 200)           # 坐标点击
d(text="登录").click()      # 元素定位点击
d.send_keys("username")     # 文本输入
```

**核心问题：**
- **脆弱性**：UI 变化导致脚本失效
- **维护成本**：每个流程都要硬编码
- **泛化能力**：无法处理未知场景

### 1.2 AI Agent 的范式转变

AI Agent 引入**感知-决策-执行**三层架构：

```
┌─────────────────────────────────────────────────────────┐
│                    AI Agent Architecture                 │
├─────────────────────────────────────────────────────────┤
│  ┌──────────────┐    ┌──────────────┐    ┌──────────────┐ │
│  │  感知层      │ →  │  决策层      │ →  │  执行层      │ │
│  │  Perception  │    │  Planning    │    │  Action      │ │
│  └──────────────┘    └──────────────┘    └──────────────┘ │
│         │                   │                   │          │
│    屏幕截图/OCR        LLM/多模态模型       uiauto2/      │
│    传感器数据          推理决策            Midscene/      │
│    应用状态            任务分解            Accessibility  │
└─────────────────────────────────────────────────────────┘
```

**范式对比：**

| 维度 | 脚本自动化 | AI Agent |
|------|-----------|----------|
| **控制方式** | 硬编码坐标/元素 | 自然语言指令 |
| **适应性** | UI变化即失效 | 视觉理解，自适应 |
| **维护性** | 高维护成本 | 低维护，自描述 |
| **扩展性** | 线性扩展 | 组件化扩展 |
| **智能化** | 无 | 推理、规划、学习 |

---

## 二、核心架构组件设计

### 2.1 感知层（Perception Layer）

感知层负责**理解当前环境状态**，是 Agent 的"眼睛"。

#### 2.1.1 多模态感知架构

```python
class PerceptionModule:
    """
    多模态感知模块
    整合视觉、文本、传感器数据
    """
    
    def __init__(self, config):
        self.vision_model = config.vision_model  # 视觉模型
        self.ocr_engine = config.ocr_engine      # OCR引擎
        self.sensor_reader = config.sensor       # 传感器读取
        
    async def perceive(self, device) -> WorldState:
        """
        感知当前世界状态
        """
        # 1. 获取屏幕截图
        screenshot = await device.screenshot()
        
        # 2. 视觉理解
        visual_info = await self.vision_model.analyze(screenshot)
        
        # 3. OCR文本提取
        text_elements = self.ocr_engine.extract(screenshot)
        
        # 4. 传感器数据
        sensors = self.sensor_reader.get_state()
        
        # 5. 构建世界状态
        return WorldState(
            screenshot=screenshot,
            ui_elements=visual_info.elements,
            text_content=text_elements,
            device_info=sensors,
            timestamp=datetime.now()
        )
```

#### 2.1.2 感知层设计要点

**1. 多分辨率适配**
```python
class AdaptivePerception:
    """适配不同屏幕分辨率"""
    
    def normalize_coordinates(self, x, y, screen_size):
        """
        将绝对坐标归一化为相对坐标（0-1范围）
        适配不同分辨率设备
        """
        return {
            'x': x / screen_size.width,
            'y': y / screen_size.height
        }
    
    def denormalize(self, rel_x, rel_y, target_size):
        """将相对坐标转换为目标设备绝对坐标"""
        return {
            'x': int(rel_x * target_size.width),
            'y': int(rel_y * target_size.height)
        }
```

**2. 增量感知优化**
```python
class IncrementalPerception:
    """增量感知，减少重复计算"""
    
    def __init__(self):
        self.last_screenshot = None
        self.last_hash = None
        
    async def perceive_if_changed(self, device):
        """仅当屏幕变化时才重新感知"""
        current = await device.screenshot()
        current_hash = self.compute_hash(current)
        
        if current_hash == self.last_hash:
            return None  # 无变化，返回缓存
        
        self.last_hash = current_hash
        self.last_screenshot = current
        
        return await self.full_perception(current)
```

**3. 多模态融合策略**
```python
class MultimodalFusion:
    """融合多种感知模态"""
    
    def fuse(self, vision_data, ocr_data, sensor_data):
        """
        多模态数据融合
        例如：视觉检测到按钮 + OCR识别文字 = 语义化元素
        """
        fused_elements = []
        
        for v_elem in vision_data:
            # 匹配OCR文本
            text = self.find_matching_text(v_elem.bbox, ocr_data)
            
            # 匹配传感器上下文
            context = self.get_sensor_context(v_elem, sensor_data)
            
            fused_elements.append(FusedElement(
                bbox=v_elem.bbox,
                type=v_elem.type,
                text=text,
                context=context,
                confidence=self.compute_fusion_confidence(v_elem, text)
            ))
        
        return fused_elements
```

---

## 三、决策层（Planning Layer）

决策层是 Agent 的"大脑"，负责**推理、规划、决策**。

#### 2.2.1 LLM 驱动的决策架构

```python
class LLMPlanner:
    """
    基于大语言模型的任务规划器
    """
    
    def __init__(self, model_config):
        self.llm = LLMClient(model_config)
        self.memory = ConversationMemory()
        self.tool_registry = ToolRegistry()
        
    async def plan(self, goal: str, world_state: WorldState) -> ActionPlan:
        """
        根据目标和当前状态生成行动计划
        """
        # 构建提示
        prompt = self.build_planning_prompt(goal, world_state)
        
        # 调用LLM进行规划
        response = await self.llm.chat(
            messages=[
                {"role": "system", "content": self.get_system_prompt()},
                {"role": "user", "content": prompt}
            ],
            temperature=0.3,  # 低温度，更确定性
            response_format="json"  # 结构化输出
        )
        
        # 解析行动计划
        plan = self.parse_action_plan(response)
        
        # 验证计划可行性
        validated_plan = self.validate_plan(plan, world_state)
        
        return validated_plan
```

---

## 四、工程实践要点

### 4.1 错误处理与容错设计

```python
class FaultTolerantExecutor:
    """容错执行器"""
    
    def __init__(self, max_retries=3, retry_delay=1.0):
        self.max_retries = max_retries
        self.retry_delay = retry_delay
        self.error_classifier = ErrorClassifier()
    
    async def execute_with_resilience(self, action):
        """带容错的动作执行"""
        
        for attempt in range(self.max_retries):
            try:
                result = await self.execute(action)
                
                if result.success:
                    return result
                
                # 分析错误类型
                error_type = self.error_classifier.classify(result.error)
                
                if error_type == ErrorType.TRANSIENT:
                    # 瞬态错误，重试
                    await asyncio.sleep(self.retry_delay * (attempt + 1))
                    continue
                    
                elif error_type == ErrorType.RECOVERABLE:
                    # 可恢复错误，尝试恢复
                    recovery_action = await self.generate_recovery(action, result.error)
                    if recovery_action:
                        await self.execute(recovery_action)
                        continue
                    
                else:
                    # 致命错误，放弃
                    return result
                    
            except Exception as e:
                logger.exception(f"Execution failed: {e}")
                if attempt == self.max_retries - 1:
                    return ActionResult.fail(str(e))
        
        return ActionResult.fail("Max retries exceeded")
```

---

## 五、架构选型决策树

```
选择AI Agent架构
│
├─ 是否需要实时响应？
│  ├─ 是 → 选择边缘计算架构
│  └─ 否 → 继续
│
├─ 任务是否结构化？
│  ├─ 是 → 状态机 + 规则引擎
│  └─ 否 → 继续
│
├─ 是否需要复杂推理？
│  ├─ 是 → LLM + ReAct模式
│  └─ 否 → 简单反射架构
│
├─ 是否需要多设备协同？
│  ├─ 是 → 分布式Agent架构
│  └─ 否 → 单机Agent架构
│
└─ 选择具体实现
   ├─ 快速原型 → Midscene + 豆包
   ├─ 企业级 → 自研 + uiautomator2
   └─ 端侧部署 → MLC-LLM + 本地模型
```

---

## 六、总结

AI Agent 架构设计的核心在于**分层解耦、模块化扩展、容错设计**：

1. **感知层**：多模态融合，增量优化
2. **决策层**：分层规划，反思机制
3. **执行层**：动作原语，容错执行

**工程实践原则：**
- 分层设计，单一职责
- 插件化扩展，避免硬编码
- 状态机管理复杂流程
- 全面的错误处理与恢复
- 性能优化与缓存策略

**推荐技术栈：**
- 快速验证：Midscene + 豆包
- 深度定制：uiautomator2 + 自研Agent
- 端侧部署：MLC-LLM + Qwen-1.8B

---

**参考链接：**
- Midscene: https://midscenejs.com/zh/
- ReAct Pattern: https://arxiv.org/abs/2210.03629
- MLC-LLM: https://llm.mlc.ai/
- uiautomator2: https://github.com/openatx/uiautomator2

---

*写于 2026 年 4 月 18 日*  
*没有夜空的星星*