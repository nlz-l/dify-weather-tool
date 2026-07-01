# Dify 天气查询工具服务

![Python](https://img.shields.io/badge/python-3.13+-blue)
![FastAPI](https://img.shields.io/badge/FastAPI-0.115+-009688)
![License](https://img.shields.io/badge/license-MIT-green)
![Status](https://img.shields.io/badge/status-learning-orange)

为 [Dify](https://dify.ai) AI 应用平台定制的天气查询工具服务。提供标准化的 `/weather` API 接口，返回自然语言格式的天气结果供 LLM 直接使用。

## 功能特性

- 🌤️ 城市天气实时查询（覆盖 38 个中国主要城市）
- 🔐 Bearer Token 鉴权机制
- 📡 调用 `t.weather.itboy.net` 天气数据源
- 🗣️ 返回自然语言结果，适合大模型直接转述

## 项目结构

```text
Dify/
├── main.py            # FastAPI 应用入口，包含路由和逻辑
├── pyproject.toml     # 项目依赖配置 (uv)
├── .env.example       # 环境变量模板
└── README.md
```

## 快速开始

### 环境要求

- Python 3.13+
- [uv](https://docs.astral.sh/uv/)

### 安装与运行

```bash
# 安装依赖
uv sync

# 配置鉴权 Token（PowerShell）
$env:DIFY_WEATHER_TOKEN="your-secret-token"

# 启动服务
uv run python main.py
```

服务默认监听 `http://localhost:8081`。

## API 文档

### POST /weather

查询指定城市的天气信息。

**请求头：**

```http
Authorization: Bearer <token>
Content-Type: application/json
```

**请求体：**

```json
{
  "location": "北京"
}
```

**支持的城市（部分）：**

北京、上海、广州、深圳、杭州、南京、成都、重庆、武汉、西安、天津、苏州、长沙、郑州、青岛、大连、厦门、福州、合肥、济南、哈尔滨、长春、沈阳、昆明、贵阳、南宁、海口、兰州、乌鲁木齐、呼和浩特、拉萨、银川、西宁、石家庄、太原、宁波、无锡、珠海

**返回示例：**

```text
北京今天是晴，温度30℃/20℃
```

## 接入 Dify 平台

1. 进入 Dify 工作台 → **工具** → **创建自定义工具**
2. 选择 **OpenAPI Swagger** 导入或手动配置：
   - 请求方法：`POST`
   - 接口地址：`http://<your-server>:8081/weather`
   - 请求头：`Authorization: Bearer <your-token>`
   - 请求体字段：`location`（string）
3. 在 Agent 应用中关联该工具即可使用

## 注意事项

| 项目 | 说明 |
|---|---|
| 天气数据源 | 依赖 `t.weather.itboy.net`，网络异常时查询失败 |
| 城市支持 | 当前硬编码 38 个城市，扩展需修改源码中的映射表 |
| 安全鉴权 | Bearer Token 适合学习/本地演示，生产环境建议升级为 API Key + HTTPS |
| 端口 | 默认 8081，可通过代码修改 |
