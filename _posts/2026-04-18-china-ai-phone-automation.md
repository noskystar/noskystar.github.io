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

## 三、AI驱动的下一代自动化框架

### 1. Midscene —— 字节跳动的AI自动化明星 🌟

| 属性 | 详情 |
|------|------|
| **官网** | https://midscenejs.com/zh/ |
| **GitHub** | https://github.com/web-infra-dev/midscene |
| **出品** | 字节跳动（ByteDance）web-infra团队 |
| **Star** | 12k+ ⭐ |
| **趋势榜** | GitHub Trending 第2名 |
| **定位** | AI驱动、视觉感知的全平台UI自动化 |
| **支持** | Web、PC、Mobile（Android/iOS/鸿蒙） |

**核心优势：**

**① 自然语言控制**
无需编写复杂的定位代码，直接用自然语言描述操作：
```javascript
// 使用自然语言控制手机
await agent.aiAction('点击"设置"按钮');
await agent.aiAction('在搜索框输入"天气"');
await agent.aiAction('向上滑动查看更多信息');
```

**② 多模型支持**
Midscene 支持多种大模型，可根据场景灵活选择：

| 模型 | 厂商 | 特点 |
|------|------|------|
| **豆包 Seed Vision** | 字节跳动 | 针对UI元素识别优化，国内首选 |
| **Qwen3-VL** | 阿里云 | 性价比高，中文理解强 |
| **Gemini-3-Pro** | Google | 视觉能力强，全面支持 |

**③ 多端统一**
一套API控制多个平台：
- **Web**: 集成 Puppeteer / Playwright
- **PC**: 控制 macOS、Windows、Linux 桌面应用
- **Mobile**: Android、iOS、鸿蒙设备自动化

**④ 丰富的工具链**
- **可视化报告**: 自动化流程回溯
- **Playground**: 交互式调试环境
- **Skills & MCP**: 支持AI编程工具和MCP Server
- **YAML脚本**: 用YAML编写自动化流程

**Mobile端快速上手：**
```javascript
import { Agent } from '@midscene/android';

const agent = new Agent({
  model: 'doubao-seed',  // 使用豆包模型
});

// 连接设备
await agent.connect();

// 自然语言操作
await agent.aiAction('打开微信');
await agent.aiAction('点击通讯录');
await agent.aiAction('找到张三并发送"你好"');

// 断言验证
await agent.aiAssert('看到聊天界面中的"你好"消息');
```

**与uiautomator2对比：**

| 特性 | Midscene | uiautomator2 |
|------|----------|--------------|
| **控制方式** | 自然语言 | 代码定位 |
| **学习曲线** | 极低 | 中等 |
| **AI原生** | ✅ 天生为AI设计 | 需要二次封装 |
| **多模型** | ✅ 支持 | 需自行集成 |
| **跨平台** | ✅ Web/PC/Mobile | 仅Android |
| **可视化** | ✅ 报告+Playground | 需搭配其他工具 |

---

## 四、AI + 手机控制的新兴方案

### 2. 大模型驱动的Agent框架

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

| 框架 | 技术栈 | 学习曲线 | 适用场景 | AI友好度 | 跨平台 |
|------|--------|----------|----------|----------|--------|
| **Midscene** | JavaScript/TS | ⭐ | AI自动化/测试 | ⭐⭐⭐⭐⭐ | ✅ Web/PC/Mobile |
| uiautomator2 | Python | ⭐⭐ | 开发/测试 | ⭐⭐⭐⭐ | ❌ 仅Android |
| SoloPi | App(录制) | ⭐ | 测试/运维 | ⭐⭐⭐ | ❌ 仅Android |
| AutoX.js | JavaScript | ⭐⭐ | 个人自动化 | ⭐⭐⭐ | ❌ 仅Android |
| Appium | 多语言 | ⭐⭐⭐ | 企业级测试 | ⭐⭐⭐ | ✅ iOS/Android |

### 选型建议

**情况1：AI Agent开发 / 自然语言控制**
→ 选 **Midscene** ⭐ **强烈推荐**
- 字节跳动出品，GitHub 12k+ Stars
- 自然语言控制，无需学习复杂API
- 原生支持多模型（豆包/Qwen/Gemini）
- 跨平台：一套代码控制Web/PC/Mobile
- 可视化报告和Playground调试

**情况2：Python开发者 + 传统自动化**
→ 选 **uiautomator2**
- 生态丰富，与Python AI生态无缝衔接
- 文档完善，社区活跃

**情况3：测试工程师 + 录制回放**
→ 选 **SoloPi** 或 **Midscene**
- SoloPi：零代码录制，性能测试一站解决
- Midscene：自然语言描述测试步骤，AI自动执行

**情况4：前端开发者 + 轻量需求**
→ 选 **AutoX.js**
- JavaScript语法熟悉
- 脚本分享方便

---

## 五、国产框架的未来趋势

### 1. 与国产大模型深度集成

预计2025年，将出现：
- **Midscene + 豆包** 官方深度集成，端侧模型部署
- **GLM + uiautomator2** 官方集成方案
- **通义千问** 推出手机Agent SDK
- **文心一言** 支持自动化工作流

### 2. 端侧AI部署

随着手机算力提升：
- **Midscene** 支持端侧小模型（Qwen-1.8B、豆包端侧版）
- 本地视觉模型识别UI元素
- 无需联网即可实现AI控制
- 隐私数据不出设备

### 3. 低代码/无代码化

- **Midscene YAML**：自然语言直接生成自动化流程
- SoloPi式的录制增强
- "帮我订外卖" → AI自动完成全部操作

---

## 七、总结

国内AI手机控制框架已进入**2.0时代**：

### 🌟 新一代：AI原生框架
**Midscene**（字节跳动）代表了未来方向：
- ✅ 自然语言控制，零学习成本
- ✅ 原生多模型支持（豆包/Qwen/Gemini）
- ✅ 跨平台：Web/PC/Mobile一套代码
- ✅ GitHub 12k+ Stars，国内最火AI自动化框架

### 🛠️ 传统框架：稳定可靠
- **uiautomator2**：开发者首选，Python生态完善
- **SoloPi**：测试工程师利器，录制回放零代码
- **AutoX.js**：个人自动化，JavaScript轻量方案

### 🚀 选型建议

| 场景 | 推荐框架 |
|------|----------|
| **AI Agent开发** | **Midscene** ⭐ |
| **自然语言控制** | **Midscene** ⭐ |
| **跨平台自动化** | **Midscene** ⭐ |
| **Python开发者** | uiautomator2 |
| **录制回放测试** | SoloPi / Midscene |
| **前端轻量需求** | AutoX.js |

### 💡 未来已来
随着**Midscene**等AI原生框架的成熟，"用自然语言控制手机"已从实验室走向实用。字节跳动等大厂的入场，标志着这个领域的爆发期已经到来。

对于开发者来说，**现在学习Midscene，就是提前布局AI自动化时代**。

---

**参考链接：**
- **Midscene**（强烈推荐）: https://midscenejs.com/zh/
- Midscene GitHub: https://github.com/web-infra-dev/midscene
- uiautomator2: https://github.com/openatx/uiautomator2
- SoloPi: https://github.com/alipay/SoloPi
- AutoX.js: https://github.com/kkevsekk1/AutoX
- 豆包模型: https://www.volcengine.com/product/doubao
- Qwen-VL: https://github.com/QwenLM/Qwen-VL

---

*写于 2026 年 4 月 18 日*  
*没有夜空的星星*
