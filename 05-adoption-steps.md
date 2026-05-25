# Claude — Các bước áp dụng vào ứng dụng

## Prerequisites

**Kiến thức cần có:**
- [ ] Python cơ bản (hoặc TypeScript nếu dùng JS SDK)
- [ ] Hiểu khái niệm REST API và HTTP request
- [ ] Biết dùng environment variables và `.env` file

**Tool cần cài:**
- [ ] Python >= 3.9
- [ ] pip hoặc uv (package manager)
- [ ] Tài khoản Anthropic — đăng ký tại https://console.anthropic.com
- [ ] API Key từ Anthropic Console

---

## Giai đoạn 1: Setup môi trường

### Bước 1.1 — Tạo API Key

1. Vào https://console.anthropic.com
2. Vào **API Keys** → **Create Key**
3. Copy key (chỉ hiển thị 1 lần)

### Bước 1.2 — Cài đặt SDK

```bash
# Tạo virtual environment
python -m venv .venv
source .venv/bin/activate  # Windows: .venv\Scripts\activate

# Cài Anthropic SDK
pip install anthropic
```

### Bước 1.3 — Cấu hình API Key

```bash
# Cách 1: Export trực tiếp (chỉ dùng khi dev)
export ANTHROPIC_API_KEY="sk-ant-..."

# Cách 2: Dùng .env file (khuyến nghị)
echo "ANTHROPIC_API_KEY=sk-ant-..." > .env
```

**Kiểm tra:** Chạy lệnh sau để verify setup:
```bash
python -c "from anthropic import Anthropic; print('OK')"
```

---

## Giai đoạn 2: Hello World

### Bước 2.1 — Gọi API lần đầu

```python
# hello_claude.py
from anthropic import Anthropic

client = Anthropic()  # Tự đọc ANTHROPIC_API_KEY từ env

message = client.messages.create(
    model="claude-sonnet-4-20250514",
    max_tokens=1024,
    messages=[
        {"role": "user", "content": "Xin chào Claude! Bạn là ai?"}
    ]
)

print(message.content[0].text)
```

### Bước 2.2 — Thêm System Prompt

```python
# with_system_prompt.py
message = client.messages.create(
    model="claude-sonnet-4-20250514",
    max_tokens=1024,
    system="Bạn là trợ lý hỗ trợ kỹ thuật. Luôn trả lời ngắn gọn, chính xác bằng tiếng Việt.",
    messages=[
        {"role": "user", "content": "Giải thích API là gì trong 2 câu."}
    ]
)
print(message.content[0].text)
```

### Bước 2.3 — Multi-turn Conversation

```python
# multi_turn.py
messages = []

def chat(user_input: str) -> str:
    messages.append({"role": "user", "content": user_input})
    
    response = client.messages.create(
        model="claude-sonnet-4-20250514",
        max_tokens=1024,
        system="Bạn là trợ lý hữu ích.",
        messages=messages
    )
    
    assistant_reply = response.content[0].text
    messages.append({"role": "assistant", "content": assistant_reply})
    return assistant_reply

# Test
print(chat("Tên tôi là Nam"))
print(chat("Tên tôi là gì?"))  # Claude nhớ context
```

---

## Giai đoạn 3: Tích hợp nâng cao

### Bước 3.1 — Streaming Response

```python
# streaming.py — Hiển thị response dần dần như ChatGPT
with client.messages.stream(
    model="claude-sonnet-4-20250514",
    max_tokens=1024,
    messages=[{"role": "user", "content": "Viết essay về AI trong 200 từ"}]
) as stream:
    for text in stream.text_stream:
        print(text, end="", flush=True)
```

### Bước 3.2 — Tool Use (Function Calling)

```python
# tool_use.py
import json

tools = [
    {
        "name": "get_weather",
        "description": "Lấy thông tin thời tiết của một thành phố",
        "input_schema": {
            "type": "object",
            "properties": {
                "city": {"type": "string", "description": "Tên thành phố"}
            },
            "required": ["city"]
        }
    }
]

def get_weather(city: str) -> dict:
    # Mock — thực tế gọi weather API
    return {"city": city, "temperature": 32, "condition": "Sunny"}

response = client.messages.create(
    model="claude-sonnet-4-20250514",
    max_tokens=1024,
    tools=tools,
    messages=[{"role": "user", "content": "Thời tiết Hà Nội hôm nay thế nào?"}]
)

# Xử lý tool call
if response.stop_reason == "tool_use":
    tool_use = next(b for b in response.content if b.type == "tool_use")
    tool_result = get_weather(**tool_use.input)
    
    # Gửi lại kết quả cho Claude
    final = client.messages.create(
        model="claude-sonnet-4-20250514",
        max_tokens=1024,
        tools=tools,
        messages=[
            {"role": "user", "content": "Thời tiết Hà Nội hôm nay thế nào?"},
            {"role": "assistant", "content": response.content},
            {"role": "user", "content": [
                {"type": "tool_result", "tool_use_id": tool_use.id,
                 "content": json.dumps(tool_result)}
            ]}
        ]
    )
    print(final.content[0].text)
```

### Bước 3.3 — Structured Output (JSON)

```python
# structured_output.py
import json

response = client.messages.create(
    model="claude-sonnet-4-20250514",
    max_tokens=1024,
    system="""Bạn là API. Luôn trả về JSON hợp lệ, không có text thêm.
Format: {"sentiment": "positive|negative|neutral", "score": 0.0-1.0, "reason": "string"}""",
    messages=[{"role": "user", "content": "Phân tích: 'Sản phẩm này tuyệt vời, tôi rất hài lòng!'"}]
)

result = json.loads(response.content[0].text)
print(result)  # {"sentiment": "positive", "score": 0.95, "reason": "..."}
```

---

## Giai đoạn 4: Production Readiness

### Checklist trước khi go-live

- [ ] API key lưu trong secrets manager (AWS Secrets Manager, Vault), không trong code
- [ ] Implement retry với exponential backoff khi gặp rate limit (429)
- [ ] Set timeout hợp lý (30-60s cho non-streaming)
- [ ] Logging: log input token count, output token count, latency, errors
- [ ] Cost monitoring: theo dõi token usage, set budget alert trên Anthropic Console
- [ ] Input validation: sanitize user input trước khi đưa vào prompt
- [ ] Output validation: kiểm tra format output trước khi trả về user
- [ ] Graceful degradation: fallback khi API down
- [ ] Rate limiting phía ứng dụng để tránh user abuse

### Xử lý Rate Limit

```python
import time
from anthropic import RateLimitError

def call_with_retry(client, **kwargs, max_retries=3):
    for attempt in range(max_retries):
        try:
            return client.messages.create(**kwargs)
        except RateLimitError:
            if attempt < max_retries - 1:
                time.sleep(2 ** attempt)  # 1s, 2s, 4s
            else:
                raise
```

---

## Các lỗi phổ biến và cách xử lý

| Lỗi | Nguyên nhân | Cách fix |
|-----|-------------|----------|
| `AuthenticationError` | API key sai hoặc hết hạn | Kiểm tra lại key trên console |
| `RateLimitError (429)` | Vượt quá rate limit | Implement retry với backoff |
| `OverloadedError (529)` | Server Anthropic bận | Retry sau vài giây |
| `InvalidRequestError` | Prompt quá dài, vượt context window | Cắt bớt conversation history |
| Output không phải JSON | Model không tuân thủ format | Tăng cường system prompt, dùng regex extract |
