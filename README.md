# Dify 天气查询工具

[![Python](https://img.shields.io/badge/python-3.13+-blue)](https://www.python.org/)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.115+-009688)](https://fastapi.tiangolo.com/)
[![License](https://img.shields.io/badge/license-MIT-green)](./LICENSE)

为 [Dify](https://dify.ai) AI 平台定制的天气查询微服务，返回自然语言格式的天气结果供大模型直接使用。

---

## ✨ 功能特性

| 功能 | 说明 |
|---|---|
| 🌤️ 天气查询 | 38 个中国主要城市实时天气 |
| 🔐 鉴权 | Bearer Token 认证 |
| 🗣️ 自然语言输出 | 结果直接适合大模型转述 |
| 📡 数据源 | `t.weather.itboy.net` API |

---

## 📂 项目结构

```text
Dify/
├── main.py            # FastAPI 应用入口
├── pyproject.toml     # 依赖配置（uv）
├── .env.example       # 环境变量模板
└── README.md
```

---

## 🚀 快速开始

环境要求：Python 3.13+、[uv](https://docs.astral.sh/uv/)

```bash
uv sync
$env:DIFY_WEATHER_TOKEN="your-secret-token"   # PowerShell
uv run python main.py                          # → http://localhost:8081
```

---

## 📡 接口文档

### POST /weather

查询指定城市天气。

**请求：**

```http
POST /weather HTTP/1.1
Authorization: Bearer <token>
Content-Type: application/json

{ "location": "北京" }
```

**响应：**

```text
北京今天是晴，温度30℃/20℃
```

**支持城市（38 个）：**

北京、上海、广州、深圳、杭州、南京、成都、重庆、武汉、西安、天津、苏州、长沙、郑州、青岛、大连、厦门、福州、合肥、济南、哈尔滨、长春、沈阳、昆明、贵阳、南宁、海口、兰州、乌鲁木齐、呼和浩特、拉萨、银川、西宁、石家庄、太原、宁波、无锡、珠海

---

## 🔗 接入 Dify

1. Dify 工作台 → **工具** → **创建自定义工具**
2. 请求方法：`POST`，地址：`http://<your-server>:8081/weather`
3. 请求头：`Authorization: Bearer <your-token>`
4. 请求体字段：`location`（string）
5. 在 Agent 应用中关联该工具

---

## ⚠️ 注意事项

| 项目 | 说明 |
|---|---|
| 数据源 | 依赖 `t.weather.itboy.net`，网络异常时查询失败 |
| 城市列表 | 当前硬编码 38 个城市，扩展需修改源码 |
| 安全 | Bearer Token 适合学习/本地演示，生产环境建议升级为 API Key + HTTPS |
| 端口 | 默认 8081，可在代码中修改 |
