# FastAIE 轻量级AI工具执行桌面应用

<div align="center">
  
[![Version](https://img.shields.io/badge/version-0.1.0-green.svg)](package.json)
[![Tauri](https://img.shields.io/badge/Tauri-2.5.0-orange.svg)](https://tauri.app/)
[![React](https://img.shields.io/badge/React-18.2.0-blue.svg)](https://reactjs.org/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.6.3-blue.svg)](https://www.typescriptlang.org/)

</div>


## 项目概述

###  简介

FastAIE（Fast AI Execute）是一款基于 **Tauri 2** 框架的**超轻量级** AI 工具调用执行客户端，开启工具调用能力后，可以自定义添加各种命令行工具让AI自由选择，高效完成任务。

**核心设计理念**：让 AI 不仅能"回答"，更能"执行"——下载后即可让大模型直接帮你执行命令、请求接口、修改文件，零配置、无服务端、纯本地运行。

> ⚠️ **系统依赖**：Windows 用户需安装 [WebView2 Runtime](https://developer.microsoft.com/en-us/microsoft-edge/webview2/)（Win10/11 自带）

###  核心特点

- ✅ **轻量快速**：单文件，无需安装，软件仅 15MB，启动秒开
- ✅ **工具调用**：AI 可执行系统命令、文件操作、网络请求、扫描端口
- ✅ **支持本地运行**：本地模型所有数据本地存储，无隐私泄露风险
- ✅ **高度可定制**：自定义 AI 角色、工作流、工具配置

###  核心原理

```
1. 启动时注入系统提示词 → 告诉 AI 可用工具、参数格式、安全边界
2. 用户发消息 → AI 判断是否需要调用工具
3. 需要工具 → 生成调用指令 → FastAIE 解析 → 本地执行
4. 执行结果 → 返回给 AI → AI 整理后以自然语言回复用户
```

 - 目的是通过通过命令执行、文件操作、网络请求函数，**打通AI连接外部所有命令行工具的通道**。


###  使用示例
- 首次使用，先配置自定义AI服务，或直接使用内置免费AI助手
<img width="1200" height="800" alt="image" src="https://github.com/user-attachments/assets/b69d68e9-4be6-49b3-bb5a-2747148fe494" />

- 开启工具调用能力，让AI可以自由调用内置工具
  <img width="1216" height="843" alt="image" src="https://github.com/user-attachments/assets/9e49f9c5-b129-4707-b069-fe65e5ae6c87" />

- 提示词中配置命令行功能，让AI了解目前可以使用的工具
<img width="986" height="456" alt="image" src="https://github.com/user-attachments/assets/3d3c31cd-84e7-4cde-aa38-7212413051c3" />

- 输入 “帮我扫描127.0.0.1端口” 自动调用工具，完成后返回结果
<img width="1200" height="976" alt="image" src="https://github.com/user-attachments/assets/1c0e65f1-61f8-4496-9d5c-be238d8ccce8" />

- 网络在线主机探测
<img width="1200" height="859" alt="image" src="https://github.com/user-attachments/assets/449f7357-6e3c-40e8-a3c6-b1bb7a9f7f03" />

<img width="1295" height="887" alt="image" src="https://github.com/user-attachments/assets/d317a53c-dfe9-4559-acd9-574059cf6026" />

- 利用nmap工具进行远程操作系统识别
  
<img width="1200" height="953" alt="image" src="https://github.com/user-attachments/assets/2aae8cb6-1d9d-4a26-8283-eae5ed10dbed" />


---

##  技术栈

###  前端技术

| 技术           | 版本   | 用途             | 说明          |
| -------------- | ------ | ---------------- | ------------- |
| React          | 18.2.0 | UI 框架          | 使用 Hooks    |
| TypeScript     | 5.6.3  | 类型系统         | 严格模式      |
| Vite           | 6.0.3  | 构建工具         | 快速 HMR      |
| Ant Design     | 5.15.5 | UI 组件库        | 企业级组件    |
| Zustand        | 4.4.7  | 状态管理         | 轻量级 Store  |
| React Router   | 6.30.0 | 路由管理         | v6 新特性     |
| React Markdown | 10.1.0 | Markdown 渲染    | 支持 GFM      |
| TailwindCSS    | 3.4.17 | CSS 框架         | 实用优先      |

###  后端技术

| 技术      | 用途           | 说明           |
| -------   | -------------- | -------------- |
| Tauri    | 桌面应用框架   | Rust 驱动      |
| Rust     | 系统编程语言   | 内存安全       |
| Serde     | 序列化/反序列化 | JSON 处理      |
| Tokio      | 异步运行时     | 高性能异步 I/O |
| Reqwest    | HTTP 客户端    | 异步请求       |

---

##  系统架构

###  整体架构图

```
┌─────────────────────────────────────────────────────────┐
│                    Frontend (React)                      │
│  ┌────────────┐  ┌────────────┐  ┌────────────┐        │
│  │   Pages    │  │ Components │  │  Services  │        │
│  └─────┬──────┘  └─────┬──────┘  └─────┬──────┘        │
│        │                │                │               │
│        └────────────────┼────────────────┘               │
│                         │                                │
│                    ┌────▼────┐                          │
│                    │  Store  │ (Zustand)               │
│                    └────┬────┘                          │
└─────────────────────────┼──────────────────────────────┘
                          │ Tauri IPC
┌─────────────────────────▼──────────────────────────────┐
│                  Backend (Rust + Tauri)                 │
│  ┌────────────┐  ┌────────────┐  ┌────────────┐        │
│  │  Commands  │  │   Models   │  │  Storage   │        │
│  └─────┬──────┘  └─────┬──────┘  └─────┬──────┘        │
│        │                │                │               │
│        └────────────────┼────────────────┘               │
│                    ┌────▼────┐                          │
│                    │ File I/O│                          │
│                    └─────────┘                          │
└──────────────────────────────────────────────────────────┘
                          │
                   ┌──────▼──────┐
                   │Local Storage│
                   │ (JSON Files)│
                   └─────────────┘
```


---

##  核心功能

###  多模型支持

- 支持 OpenAI、DeepSeek、Ollama、智谱 AI 等主流 AI 服务
- 流式输出，实时显示 AI 回复
- 对话中随时切换模型
- 支持自定义 API 端点和密钥

###  智能对话系统

- 完整的对话历史记录和会话管理
- 支持长对话上下文理解
- 内置多种预设角色（包括"暴躁助手"等特色角色）
- 支持自定义角色系统提示词

###  附件支持

- 支持同时上传多个文件（最多 5 个）
- 支持格式：PDF、Word (.docx)、Excel、图片、文本
- AI 自动分析文件内容并回答相关问题

###  用户体验

- 暗黑/明亮主题自动适配系统，支持手动切换
- Markdown 完整语法支持，代码语法高亮
- 全局快捷键快速唤起应用

---

##  工具调用系统

###  内置三大核心工具

| 工具             | 作用                     | 示例                                                 |
| ---------------- | ------------------------ | ---------------------------------------------------- |
| `execute_command` | 执行本地命令行           | `dir`, `ipconfig`, `nmap -p 1-1000 127.0.0.1`       |
| `http_request`    | 发送 HTTP/HTTPS 请求     | GET 公网 API、POST 数据到服务器                      |
| `file_operation`  | 文件读写删移             | 批量改后缀、读取日志、生成 JSON 报表                 |

###  工具调用灵敏度模式

FastAIE 支持三种解析灵敏度，兼容不同能力层级的 AI 模型：

| 模式       | 格式要求                            | 说明                           | 适用模型           |
| ---------- | ----------------------------------- | ------------------------------ | ------------------ |
| 高灵敏度   | 严格 JSON-RPC 2.0 格式              | 结构化强，格式稳定             | GPT-4、Claude 等   |
| 中灵敏度   | `[TOOL_CALL:工具名]{JSON参数}`      | 兼顾结构与容错                 | 大部分主流模型     |
| 低灵敏度   | `工具名{参数JSON}`                  | 适配自然语言模型，容错性最高   | 所有模型           |

**示例对比**：

```javascript
// 低灵敏度
execute_command{"command": "dir"}

// 中灵敏度
[TOOL_CALL:execute_command]{
  "command": "ipconfig",
  "args": ["/all"]
}

// 高灵敏度
{
  "jsonrpc": "2.0",
  "method": "tools/call",
  "params": {
    "name": "execute_command",
    "arguments": {
      "command": "ping",
      "args": ["bing.com"]
    }
  },
  "id": 1
}
```

###  内置 Nmap 网络扫描工具提示词

FastAIE 内置了 Nmap 网络扫描工具提示词，安装nmap后无需额外配置，即可指挥AI使用nmap工具。

**常用 Nmap 参数**：
- `-p <端口范围>`：指定扫描端口（如 `-p 1-1000`）
- `-sV`：探测服务版本
- `-O`：探测操作系统
- `-A`：启用全面扫描（版本检测 + 脚本扫描）
- `-sS`：TCP SYN 扫描（半开放扫描）
- `-Pn`：跳过主机发现

**使用示例**：

```
用户：使用 nmap 扫描本地端口
AI：好的，我将扫描 127.0.0.1 的 Top 1000 端口

[工具调用: execute_command]
命令: nmap -p  127.0.0.1

[结果展示]
PORT     STATE SERVICE
80/tcp   open  http
443/tcp  open  https
3306/tcp open  mysql

AI：扫描完成！发现了 3 个开放端口...
```

---

##  工作流引擎

###  工作流概述

工作流系统允许用户创建可重复执行的任务序列，支持条件分支和错误处理。

###  工作流特性

- **预定义工作流**：创建可重复执行的任务流程
- **条件分支**：支持基于结果的条件判断和分支执行
- **定时执行**：设置工作流定时自动运行
- **步骤管理**：可视化编辑工作流步骤
- **错误恢复**：支持重试、跳过、替代步骤等错误处理策略

###  实用场景示例

**场景 ：系统信息收集**
```
步骤 1: 执行 systeminfo 获取系统信息
步骤 2: 执行 wmic cpu 获取 CPU 信息
步骤 3: 执行 wmic memorychip 获取内存信息
步骤 4: 汇总生成报告
```

---

##  安全机制

###  工具调用安全

- ✅ 默认关闭工具调用功能
- ✅ 首次启用时显示风险警告
- ✅ 可配置命令白名单
- ✅ 建议在虚拟机中运行
- ✅ 记录所有工具调用历史


---

##  使用指南

###  基础对话

1. 选择 AI 角色（或使用默认角色）
2. 在输入框输入问题
3. 按 Enter 发送，Shift+Enter 换行
4. AI 将流式返回回复内容

###  工具调用

启用工具调用后，AI 可执行实际任务：

```
用户：帮我查看当前目录下的文件
AI：好的，让我执行 dir 命令
[工具调用: execute_command]
[结果展示: 文件列表]
```

⚠️ **安全提示**：工具调用功能默认关闭，需在"工具调用管理"页面手动启用。

###  附件上传

1. 点击输入框左侧 📎 图标
2. 选择文件（支持 PDF、Word、Excel、图片）
3. AI 自动分析文件内容并回答问题

###  自定义 AI 角色

1. 进入"角色管理"页面
2. 点击"添加新角色"
3. 设置角色名称、图标、描述
4. 编写系统提示词（定义角色行为）
5. 在聊天页面选择该角色使用

---

##  配置说明

###  AI 服务配置

在"AI 设置"页面配置：

**支持的 API 提供商**：
- OpenAI (GPT-3.5, GPT-4)
- DeepSeek
- Ollama（本地模型）
- 智谱 AI (GLM)
- 自定义提供商

**配置项**：
- API Key
- API Endpoint（可选）
- 模型选择
- 请求参数（温度、最大 tokens 等）

###  工具调用配置

在"工具调用管理"页面配置：

1. **全局开关**：启用/禁用工具调用功能
2. **解析灵敏度**：
   - 低（推荐）：支持所有格式
   - 中：支持 [MCP_CALL] 格式和 JSON-RPC
   - 高：仅支持严格的 JSON-RPC 格式
3. **自定义提示词**：自定义工具调用的提示词模板
4. **命令行工具**：管理自定义命令行工具列表

---


## 🙏 致谢

感谢以下开源项目：

- [Tauri](https://tauri.app/) - 跨平台桌面应用框架
- [React](https://reactjs.org/) - 用户界面库
- [Ant Design](https://ant.design/) - 企业级 UI 组件库
- [Zustand](https://github.com/pmndrs/zustand) - 状态管理库
- [Vite](https://vitejs.dev/) - 现代化构建工具

---

<div align="center">

**如果这个项目对你有帮助，请给一个 ⭐️ Star 支持一下！**


</div>
