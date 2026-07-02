# Báo cáo hoàn thành bài Day 26 - MCP Tools Integration

- Họ tên: Vũ Văn Học
- MSSV: 2A202600653
- Chủ đề: Phân biệt Function Calling và MCP, thực hành weather agent với MCP server

## Nội dung đã hoàn thành

1. Phần 01 - Function Calling thuần với Google Gemini SDK
   - Định nghĩa tool `get_weather` bằng schema thủ công
   - Thực thi tool ngay trong app
   - Cập nhật để đọc API key từ `.env` qua `GEMINI_API_KEY`

2. Phần 04 - Lab weather agent với MCP server từ xa
   - Kiểm tra và đồng bộ setup client ADK với MCP server local `http://localhost:8085/mcp`
   - Cập nhật script kiểm tra môi trường để phù hợp với README local

## Kết quả chạy thực tế trong môi trường hiện tại

### 1. Chạy `01-function-calling/weather_function_calling.py`

```text
User: Thời tiết Hà Nội và Đà Nẵng hôm nay thế nào?

 [fallback] Gemini unavailable: 403 PERMISSION_DENIED. {...}
Trả lời: Chào bạn! Hệ thống đã chuyển sang chế độ dự phòng vì không gọi được Gemini.

**Tại Hà Nội:** 🌧️
*   Nhiệt độ: 29°C
*   Thời tiết: Trời mưa nhẹ
*   Độ ẩm: 82%
*   Gió: Đông Nam, 12 km/h
*   **Lời khuyên:** Hôm nay trời có mưa nhẹ và độ ẩm cao, bạn nhớ mang theo ô hoặc áo mưa khi ra ngoài nhé!

**Tại Đà Nẵng:** ☁️
*   Nhiệt độ: 30°C
*   Thời tiết: Nhiều mây
*   Độ ẩm: 78%
*   Gió: Đông, 10 km/h
*   **Lời khuyên:** Trời nhiều mây nhưng vẫn khá ấm áp, bạn nên mặc quần áo thoáng mát nhé!
Chúc bạn một ngày tốt lành! 😊
```

### 2. Chạy `04-lab/mcp-server/weather.py`

```text
✅ MCP server initialized with Streamable HTTP transport
🔧 Available tools: get_current_weather, get_forecast, health_check
🚀 Starting MCP server on http://0.0.0.0:8085/mcp
INFO:     Started server process [18728]
INFO:     Waiting for application startup.
[07/02/26 11:20:55] INFO     StreamableHTTP       streamable_http_manager.py:131
                             session manager                                    
                             started                                            
INFO:     Application startup complete.
INFO:     Uvicorn running on http://0.0.0.0:8085 (Press CTRL+C to quit)
```

### 3. Chạy `04-lab/mcp-client/verify_setup.py`

```text
============================================================
Weather Agent Setup Verification
============================================================

🔍 Checking environment configuration...
✅ GOOGLE_API_KEY configured (AQ.Ab8RN6J...)

🔍 Checking dependencies...
✅ Google ADK
✅ python-dotenv
✅ httpx

🔍 Checking agent structure...
✅ weather_agent/agent.py
✅ weather_agent/__init__.py

🔍 Checking MCP server connectivity...
✅ MCP server reachable at http://localhost:8085/mcp

🔍 Checking agent import...
INFO:weather_agent.agent:🌐 Initializing weather agent with remote MCP server
INFO:weather_agent.agent:📡 MCP Server: http://localhost:8085/mcp
INFO:weather_agent.agent:🔌 Connecting to MCP server...
INFO:weather_agent.agent:✅ MCP toolset created successfully
INFO:weather_agent.agent:✅ Weather agent initialized with remote MCP tools:
INFO:weather_agent.agent:   - get_current_weather(city)
INFO:weather_agent.agent:   - get_forecast(city, days)
INFO:weather_agent.agent:   - health_check()
INFO:weather_agent.agent:🎉 Remote MCP connection successful!
✅ Agent imported successfully: weather_agent
   Model: gemini-2.5-flash

============================================================
✅ All checks passed!

🚀 Ready to start!
   Or:  uv run adk web

📍 Then open: http://localhost:8000
```

## Cách chạy

### 01 - Function Calling

```bash
cd 01-function-calling
pip install -r ../requirements.txt
python weather_function_calling.py
```

### 04 - Lab MCP

```bash
cd 04-lab/mcp-server
export WEATHERAPI_KEY="your_weatherapi_key"
uv run python weather.py

cd ../mcp-client
echo "GOOGLE_API_KEY=your_gemini_api_key" > .env
uv sync
uv run adk web
```

## Ghi chú

- `.env` trong repo đang được dùng cho API key của Gemini ở bài Function Calling
- `weather_function_calling.py` đã chạy được tới output cuối nhờ fallback local, nhưng vẫn báo `403 PERMISSION_DENIED` từ Gemini vì API key bị ràng buộc IP
- `weather.py` đã chạy lên MCP server thành công bằng `python3`
- `verify_setup.py` đã pass toàn bộ sau khi cài dependency và bật MCP server local