# Dify Weather Tool

FastAPI weather query service for Dify/tool-calling practice.

## Features

- `POST /weather`
- Bearer token authentication
- City code mapping for common Chinese cities
- Weather data from `t.weather.itboy.net`

## Run

```bash
uv sync
$env:DIFY_WEATHER_TOKEN="your-token"
uv run python main.py
```

Request header:

```text
Authorization: Bearer your-token
```
