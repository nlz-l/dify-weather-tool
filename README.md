# Dify 天气查询工具

[![Python](https://img.shields.io/badge/python-3.13+-blue)](https://www.python.org/)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.115+-009688)](https://fastapi.tiangolo.com/)
[![Dify](https://img.shields.io/badge/Dify-Platform-1677FF)](https://dify.ai)

> 为 Dify AI 平台开发的天气查询工具微服务。展示 FastAPI 微服务设计、第三方 API 封装、LLM 工具集成能力。

---

## 🏗️ 架构

```text
┌──────────┐     HTTP POST      ┌──────────────┐     HTTP GET     ┌──────────────┐
│   Dify    │ ─────────────────→ │  Dify 天气工具 │ ──────────────→ │  天气数据源    │
│  AI 平台  │ ←───────────────── │  FastAPI 微服务│ ←────────────── │  itboy.net   │
└──────────┘   自然语言天气结果    └──────────────┘    JSON 天气数据   └──────────────┘
```

**数据流**：Dify Agent 识别用户"查天气"意图 → 调用本服务 `/weather` → 查询天气 API → 格式化自然语言 → 返回 LLM 转述

---

## 📂 项目结构

```text
Dify/
├── main.py            # 应用入口：路由 + 城市映射 + 天气 API 调用
├── pyproject.toml     # 依赖配置（仅 fastapi、requests、uvicorn）
└── .env.example       # Token 配置
```

---

## 🔧 技术选型

| 决策点 | 选择 | 理由 |
|---|---|---|
| **框架** | FastAPI | 轻量、自动文档、类型校验 |
| **HTTP 客户端** | requests | 同步调用外部 API（业务简单，无需异步） |
| **鉴权** | Bearer Token | Dify 平台原生支持的方式 |
| **输出格式** | 自然语言字符串 | LLM 擅长理解自然语言，降低 Dify 端 Prompt 复杂度 |

---

## 🚀 快速开始

```bash
git clone https://github.com/your/dify-weather.git
cd dify-weather

uv sync
$env:DIFY_WEATHER_TOKEN="your-token"    # PowerShell
# export DIFY_WEATHER_TOKEN="your-token"  # bash

uv run python main.py     # → http://localhost:8081
```

---

## 📡 接口

**POST /weather**

```json
// Request
{ "location": "北京" }

// Response (text/plain)
"北京今天是晴，温度30℃/20℃"
```

支持 38 个中国主要城市。

---

## 🔗 Dify 平台接入

1. Dify 工作台 → 工具 → 创建自定义工具
2. 选择 OpenAPI / Swagger 导入，或手动配置：
   - 方法：`POST`
   - URL：`http://<server>:8081/weather`
   - 请求头：`Authorization: Bearer <token>`
   - 请求体：`{ "location": "string" }`
3. 在 Agent 应用中关联该工具

---

