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

## Điểm đã kiểm tra

- File `01-function-calling/weather_function_calling.py` không còn lỗi cú pháp
- File `04-lab/mcp-client/verify_setup.py` không còn lỗi cú pháp
- File `04-lab/mcp-server/weather.py` không có lỗi cú pháp
- File `04-lab/mcp-client/weather_agent/agent.py` không có lỗi cú pháp

## Output kiểm tra thực tế

### 1. Kiểm tra cú pháp file Function Calling

```bash
python3 -m py_compile 01-function-calling/weather_function_calling.py && echo OK
```

Kết quả:

```text
OK
```

### 2. Kiểm tra cú pháp file verify setup của lab

```bash
python3 -m py_compile 04-lab/mcp-client/verify_setup.py && echo OK
```

Kết quả:

```text
OK
```

### 3. Kiểm tra trạng thái repository

```bash
git remote -v
git branch --show-current
```

Kết quả:

```text
origin  https://github.com/VinUni-AI20k/Day26-MCP-Tools-Integration.git (fetch)
origin  https://github.com/VinUni-AI20k/Day26-MCP-Tools-Integration.git (push)
main
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
- Cảnh báo unresolved import trong editor là do môi trường Python hiện tại chưa cài dependency, không phải lỗi source