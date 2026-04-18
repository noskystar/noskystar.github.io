---
layout: post
title: "国内AI控制手机开源框架调研"
date: 2026-04-18 10:00:00 +0800
categories: 技术调研
tags: [AI, 移动自动化, 国产框架, Android, 手机控制]
---

> 在AI Agent浪潮下，让大模型直接控制手机操作成为热门方向。相比国际框架，国内开发者也贡献了不少优秀的开源工具。本文聚焦国产Android自动化与AI控制框架，从技术原理到应用场景进行深度调研。

## 一、为什么关注国产框架？

相比Appium等国际老牌框架，国产方案有以下优势：

- 🇨🇳 **本土化支持**：中文文档、国内社区、QQ/微信群支持
- 🔧 **更轻量级**：针对国内Android生态优化（如MIUI、ColorOS适配）
- 🚀 **迭代快速**：国内开发者响应速度快，新特性跟进及时
- 💡 **创新场景**：结合国内流行的AI模型（文心一言、通义千问、GLM等）

---

## 二、主流国产自动化框架

### 1. uiautomator2 —— 国产Android自动化之王 👑

| 属性 | 详情 |
|------|------|
| **GitHub** | https://github.com/openatx/uiautomator2 |
| **作者** | openatx 社区（国内团队） |
| **Star** | 6.5k+ |
| **定位** | Python封装的Android UI自动化 |
| **底层** | Google UiAutomator2 |
| **协议** | HTTP JSON |

**核心特点：**
- **极简API**：几行Python代码即可完成自动化
- **HTTP通信**：设备端运行服务，PC端通过HTTP控制
- **中文支持**：完整的README_CN.md，QQ群技术支持（815453846）
- **多定位方式**：支持text、resource-id、XPath、坐标等
- **生态丰富**：配套weditor元素查看器

**示例代码：**
```python
import uiautomator2 as u2

# 连接设备
d = u2.connect()

# 启动应用
d.app_start("com.example.app")

# 点击元素（通过文本）
d(text="登录").click()

# 输入内容
d.send_keys("username")

# 截图给AI分析
screenshot = d.screenshot()
```

**与AI结合：**
```python
# 结合LLM实现智能操作
import openai

def ai_control_phone(d, instruction):
    # 获取屏幕截图
    img = d.screenshot()
    
    # 发给AI分析
    response = openai.ChatCompletion.create(
        model="gpt-4-vision",
        messages=[{
            "role": "user",
            "content": f"根据截图，执行指令：{instruction}"
        }],
        image=img
    )
    
    # 解析AI返回的操作，执行
    action = parse_ai_response(response)
    execute_action(d, action)
```

---

### 2. SoloPi —— 支付宝出品的测试神器 🎯

| 属性 | 详情 |
|------|------|
| **GitHub** | https://github.com/alipay/SoloPi |
| **出品** | 支付宝（蚂蚁集团） |
| **定位** | 无线化、非侵入式Android自动化测试工具 |
| **特色** | 录制回放 + 性能测试 + 一机多控 |
| **支持** | Android、鸿蒙（solopi-harmony分支） |

**核心功能：**

**① 录制回放**
- 手机端直接录制操作步骤
- 自动生成JSON格式的用例
- 支持跨设备回放
- 无需电脑参与，手机独立完成

**② 性能测试**
- 实时悬浮窗显示性能指标
- 支持CPU、内存、帧率、网络流量监控
- 响应耗时计算
- 性能加压测试

**③ 一机多控**
- 同时控制多台设备
- 适合兼容性测试

**AI结合场景：**
```
用户操作 → SoloPi录制 → 生成JSON → 
LLM分析用例 → 优化测试路径 → 批量回放
```

**社区资源：**
- TesterHome开源项目专区
- 支持导出为Appium/Macaca脚本

---

### 3. Auto.js / AutoX.js —— JavaScript自动化 📱

| 属性 | 详情 |
|------|------|
| **GitHub** | https://github.com/kkevsekk1/AutoX |
| **前身** | Auto.js（因故停止维护） |
| **定位** | Android上的JavaScript自动化 |
| **特点** | 无需root、基于无障碍服务 |

**核心能力：**
- **JavaScript编程**：前端开发者友好
- **无障碍服务**：模拟点击、滑动、输入
- **界面分析**：控件查找、文字识别（OCR）
- **脚本市场**：丰富的现成脚本

**示例脚本：**
```javascript
// 启动微信
launchApp("微信");

// 等待特定界面
text("通讯录").waitFor();

// 点击并输入
click("搜索");
setText("张三");

// 截图保存
captureScreen("/sdcard/screenshot.png");
```

**AI增强方向：**
- 结合OCR实现智能元素定位
- 通过LLM生成自动化脚本
- 图像识别辅助复杂场景

---

## 三、AI + 手机控制的新兴方案

### 4. 大模型驱动的Agent框架

随着多模态大模型（GPT-4V、GLM-4V、Qwen-VL）的发展，**纯视觉驱动**的手机控制成为趋势：

**国内可用的多模态模型：**

| 模型 | 厂商 | 能力 |
|------|------|------|
| **GLM-4V** | 智谱AI | 图像理解、视觉问答 |
| **Qwen-VL** | 阿里云 | 图文混合输入、目标检测 |
| **文心一言-4** | 百度 | 多模态理解 |
| **豆包** | 字节跳动 | 视觉理解、中文优化 |

**通用AI控制架构：**

```
┌─────────────────────────────────────────┐
│           AI Agent Architecture          │
├─────────────────────────────────────────┤
│  视觉感知层 → 决策规划层 → 执行控制层   │
├─────────────────────────────────────────┤
│                                         │
│  [截图/OCR] → [LLM分析] → [自动化执行] │
│      ↓            ↓            ↓         │
│  屏幕理解      操作决策      uiauto2     │
│  元素识别      路径规划      SoloPi      │
│  状态判断      异常处理      AutoX       │
│                                         │
└─────────────────────────────────────────┘
```

**典型应用场景：**

**场景1：智能客服助手**
```python
# 自动处理淘宝客服消息
while True:
    screenshot = capture_screen()
    new_message = detect_new_message(screenshot)
    
    if new_message:
        # 发给大模型分析
        reply = glm4v_chat(screenshot, "如何回复这条消息？")
        
        # 自动输入回复
        input_text(reply)
        click("发送")
```

**场景2：自动化测试增强**
```python
# 结合LLM的测试用例生成
from uiautomator2 import Device

def ai_exploratory_test(d: Device):
    """AI驱动的探索性测试"""
    for step in range(100):
        # 截图理解当前状态
        state = analyze_screen(d.screenshot())
        
        # LLM决策下一步操作
        action = llm_decide(state, test_goal="验证购物车功能")
        
        # 执行并记录
        execute(d, action)
        log_step(state, action)
```

---

## 四、技术对比与选型建议

### 框架对比表

| 框架 | 技术栈 | 学习曲线 | 适用场景 | AI友好度 |
|------|--------|----------|----------|----------|
| uiautomator2 | Python | ⭐⭐ | 开发/测试 | ⭐⭐⭐⭐ |
| SoloPi | App(录制) | ⭐ | 测试/运维 | ⭐⭐⭐ |
| AutoX.js | JavaScript | ⭐⭐ | 个人自动化 | ⭐⭐⭐ |
| Appium | 多语言 | ⭐⭐⭐ | 企业级测试 | ⭐⭐⭐ |

### 选型建议

**情况1：Python开发者 + AI项目**
→ 选 **uiautomator2**
- 生态丰富，与Python AI生态无缝衔接
- 文档完善，社区活跃

**情况2：测试工程师 + 录制回放**
→ 选 **SoloPi**
- 无需编程，手机端直接操作
- 性能测试一站解决

**情况3：前端开发者 + 轻量需求**
→ 选 **AutoX.js**
- JavaScript语法熟悉
- 脚本分享方便

**情况4：AI Agent开发**
→ **uiautomator2 + 多模态LLM**
- 截图获取最方便
- API设计最符合AI调用习惯

---

## 五、国产框架的未来趋势

### 1. 与国产大模型深度集成

预计2025年，将出现：
- **GLM + uiautomator2** 官方集成方案
- **通义千问** 推出手机Agent SDK
- **文心一言** 支持自动化工作流

### 2. 端侧AI部署

随着手机算力提升：
- 小型LLM（Qwen-1.8B、ChatGLM3-6B）端侧运行
- 本地视觉模型识别UI元素
- 无需联网即可实现AI控制

### 3. 低代码/无代码化

- SoloPi式的录制增强
- 自然语言直接生成自动化流程
- "帮我订外卖" → AI自动完成全部操作

---

## 六、实践建议

### 快速上手路径

**Day 1-2：环境搭建**
```bash
# 安装uiautomator2
pip install uiautomator2

# 连接手机测试
python -c "import uiautomator2 as u2; d = u2.connect(); print(d.info)"
```

**Day 3-5：基础自动化**
- 实现自动打开App
- 完成登录流程
- 截图获取

**Day 6-7：接入AI**
- 集成大模型API
- 实现截图→理解→操作的闭环
- 处理异常情况

### 注意事项

⚠️ **权限问题**
- 国内Android系统（MIUI、ColorOS等）可能需要额外权限
- 无障碍服务需要手动开启

⚠️ **稳定性**
- 微信等App有反自动化机制
- 需要模拟人类操作间隔

⚠️ **合规性**
- 遵守目标App的使用协议
- 不要用于恶意刷单、薅羊毛

---

## 七、总结

国产Android自动化框架在**易用性**和**本土化**方面表现出色：

- **uiautomator2** 是开发者首选，生态最完善
- **SoloPi** 是测试工程师利器，零代码上手
- **AutoX.js** 满足个人自动化需求

结合国内多模态大模型（GLM-4V、Qwen-VL等），**AI控制手机**的场景正在快速成熟。对于开发者来说，现在正是入场的好时机。

---

**参考链接：**
- uiautomator2: https://github.com/openatx/uiautomator2
- SoloPi: https://github.com/alipay/SoloPi
- AutoX.js: https://github.com/kkevsekk1/AutoX
- GLM-4V: https://github.com/THUDM/GLM-4
- Qwen-VL: https://github.com/QwenLM/Qwen-VL

---

*写于 2026 年 4 月 18 日*  
*没有夜空的星星*
