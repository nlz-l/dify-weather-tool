# Dify 天气查询工具服务

这是一个用于 Dify 工具调用练习的 FastAPI 小服务。服务提供 `/weather` 接口，接收城市名称后查询天气数据，并返回适合大模型直接转述的自然语言结果。

## 功能说明

- 提供 `POST /weather` 天气查询接口
- 使用 `Authorization: Bearer <token>` 做简单鉴权
- 内置常见中国城市与天气城市编码映射
- 调用 `t.weather.itboy.net` 的城市天气接口获取数据
- 返回今日天气、天气类型和高低温信息

## 环境要求

- Python 3.13 或更高版本
- uv

## 安装依赖

```bash
uv sync
```

## 配置鉴权 Token

PowerShell 示例：

```powershell
$env:DIFY_WEATHER_TOKEN="your-token"
```

如果不设置该环境变量，服务会使用默认值 `change-me`，仅适合本地临时测试。

## 启动服务

```bash
uv run python main.py
```

默认监听：

```text
http://localhost:8081
```

## 请求示例

```http
POST /weather HTTP/1.1
Host: localhost:8081
Authorization: Bearer your-token
Content-Type: application/json

{
  "location": "北京"
}
```

返回示例：

```text
北京今天是晴，温度30℃/20℃
```

## 接入 Dify 的思路

在 Dify 中添加自定义工具或 API 工具时，将接口地址配置为该服务的 `/weather`，请求方式选择 `POST`，请求头中加入 `Authorization`，请求体传入 `location` 字段即可。

## 注意事项

- 天气接口依赖外部站点，网络不可用或接口变更时会查询失败
- 当前城市列表为代码内置，如需支持更多城市，需要补充城市编码
- 该鉴权方式适合学习和本地演示，不适合作为生产级安全方案
