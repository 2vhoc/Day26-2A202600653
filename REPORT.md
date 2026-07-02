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
timeout: failed to run command ‘uv’: No such file or directory
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
❌ Google Generative AI not installed
❌ MCP not installed
❌ FastMCP not installed
✅ python-dotenv
✅ httpx

   Install with: uv sync
   Or: pip install google-adk google-generativeai mcp fastmcp python-dotenv httpx

🔍 Checking agent structure...
✅ weather_agent/agent.py
✅ weather_agent/__init__.py

🔍 Checking MCP server connectivity...
❌ Cannot reach MCP server: All connection attempts failed

🔍 Checking agent import...
❌ Failed to import agent: No module named 'mcp'

============================================================
❌ Some checks failed

⚠️  Fix the issues above and run this script again
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
- `weather.py` chưa chạy được vì môi trường hiện tại không có `uv`
- `verify_setup.py` đã nhận `.env` nhưng vẫn fail vì thiếu package `mcp`/`fastmcp` và chưa kết nối được MCP server