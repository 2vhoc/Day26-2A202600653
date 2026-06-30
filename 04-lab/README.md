# Lab 04 — Weather Agent with Remote MCP Server

A weather agent built with Google ADK that connects to an MCP server via Streamable HTTP transport.

## Architecture

```
┌─────────────────┐   Streamable HTTP    ┌─────────────────┐      REST       ┌─────────────────┐
│   ADK Agent     │ ──────────────────── │   MCP Server    │ ─────────────── │  WeatherAPI.com │
│  (mcp-client)   │   localhost:8085/mcp │  (mcp-server)   │                 │                 │
└─────────────────┘                      └─────────────────┘                 └─────────────────┘
```

## Tools

| Tool | Description |
|------|-------------|
| `get_current_weather(city)` | Get current weather conditions for a city |
| `get_forecast(city, days)` | Get weather forecast (1–3 days) |
| `health_check()` | Verify server is running |

## Setup

### 1. MCP Server

```bash
cd mcp-server
uv sync

# Set your WeatherAPI key (get one free at https://weatherapi.com)
export WEATHERAPI_KEY="your_weatherapi_key"

# Start the server (runs on port 8085 by default)
uv run python weather.py
```

The server will be available at `http://localhost:8085/mcp`.

### 2. ADK Agent (Client)

```bash
cd mcp-client
uv sync

# Create .env file with your Gemini API key
echo "GOOGLE_API_KEY=your_gemini_api_key" > .env

# Start ADK web interface
uv run adk web
```

Open http://localhost:8000 in your browser, select `weather_agent`, and ask about the weather.

## Configuration

| Variable | Where | Description |
|----------|-------|-------------|
| `WEATHERAPI_KEY` | mcp-server | API key from weatherapi.com |
| `GOOGLE_API_KEY` | mcp-client/.env | Gemini API key |
| `PORT` | mcp-server (env) | Override server port (default: 8085) |
