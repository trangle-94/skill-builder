# Claude — Cấu trúc Project Chuyên nghiệp

## Cấu trúc thư mục chuẩn

```
my-claude-app/
├── src/
│   └── claude_app/
│       ├── __init__.py
│       ├── main.py              # Entry point (FastAPI app hoặc CLI)
│       ├── config.py            # Settings từ environment variables
│       ├── client.py            # Wrapper quanh Anthropic SDK
│       ├── prompts/
│       │   ├── __init__.py
│       │   ├── system.py        # System prompts (constants)
│       │   └── templates.py     # Prompt templates
│       ├── tools/
│       │   ├── __init__.py
│       │   └── definitions.py   # Tool schemas cho tool use
│       └── utils/
│           ├── logging.py
│           └── retry.py
├── tests/
│   ├── unit/
│   │   ├── test_client.py
│   │   └── test_prompts.py
│   └── integration/
│       └── test_api.py          # Test thật với API (cần key)
├── .env.example
├── .gitignore
├── Dockerfile
├── pyproject.toml
└── README.md
```

---

## Vai trò từng thành phần

### `src/claude_app/config.py`
Tập trung tất cả configuration, load từ environment variables.

```python
from pydantic_settings import BaseSettings

class Settings(BaseSettings):
    anthropic_api_key: str
    model: str = "claude-sonnet-4-20250514"
    max_tokens: int = 1024
    temperature: float = 0.0  # 0 = deterministic cho production

    class Config:
        env_file = ".env"

settings = Settings()
```

### `src/claude_app/client.py`
Wrapper quanh SDK — xử lý retry, logging, error handling tập trung.

```python
import logging
from anthropic import Anthropic, RateLimitError
from .config import settings

logger = logging.getLogger(__name__)

class ClaudeClient:
    def __init__(self):
        self._client = Anthropic(api_key=settings.anthropic_api_key)

    def chat(self, messages: list, system: str = None) -> str:
        kwargs = {
            "model": settings.model,
            "max_tokens": settings.max_tokens,
            "messages": messages
        }
        if system:
            kwargs["system"] = system

        logger.info("Calling Claude", extra={"message_count": len(messages)})
        response = self._client.messages.create(**kwargs)
        logger.info("Response received", extra={
            "input_tokens": response.usage.input_tokens,
            "output_tokens": response.usage.output_tokens
        })
        return response.content[0].text
```

### `src/claude_app/prompts/system.py`
Quản lý system prompts như constants — tránh hardcode rải rác trong code.

```python
CUSTOMER_SUPPORT = """Bạn là trợ lý hỗ trợ khách hàng của Công ty XYZ.
- Luôn lịch sự, thân thiện và ngắn gọn
- Chỉ trả lời câu hỏi liên quan đến sản phẩm của chúng tôi
- Nếu không biết, hãy nói "Tôi sẽ chuyển câu hỏi này cho team hỗ trợ"
- Ngôn ngữ: tiếng Việt"""

CODE_REVIEWER = """Bạn là senior developer đang review code.
Review theo thứ tự ưu tiên: Security > Performance > Correctness > Style.
Trả về danh sách issue theo format: [SEVERITY] line X: mô tả ngắn."""
```

### `src/claude_app/tools/definitions.py`
Định nghĩa tool schemas tập trung.

```python
SEARCH_TOOL = {
    "name": "search_knowledge_base",
    "description": "Tìm kiếm thông tin trong knowledge base của công ty",
    "input_schema": {
        "type": "object",
        "properties": {
            "query": {
                "type": "string",
                "description": "Câu hỏi hoặc keyword cần tìm"
            }
        },
        "required": ["query"]
    }
}

ALL_TOOLS = [SEARCH_TOOL]
```

---

## Naming conventions

| Loại | Convention | Ví dụ |
|------|------------|-------|
| Package | snake_case | `claude_app` |
| Module | snake_case | `client.py`, `retry.py` |
| Class | PascalCase | `ClaudeClient`, `Settings` |
| Function | snake_case | `chat()`, `call_with_retry()` |
| Constant (prompt) | UPPER_SNAKE | `CUSTOMER_SUPPORT`, `CODE_REVIEWER` |
| Env var | UPPER_SNAKE | `ANTHROPIC_API_KEY`, `MODEL` |

---

## Patterns quan trọng

### Không hardcode model name

```python
# KHÔNG làm thế này
response = client.messages.create(model="claude-sonnet-4-20250514", ...)

# Làm thế này — dễ upgrade model sau
response = client.messages.create(model=settings.model, ...)
```

### Tách prompt ra khỏi logic

```python
# KHÔNG — prompt nằm rải rác trong business logic
def handle_question(q):
    return claude.chat([{"role": "user", "content": f"Answer: {q}"}],
                       system="You are helpful...")  # Hardcoded ở đây

# Làm thế này — prompt tập trung
from .prompts.system import CUSTOMER_SUPPORT

def handle_question(q):
    return claude.chat([{"role": "user", "content": q}],
                       system=CUSTOMER_SUPPORT)
```

### Token budget awareness

```python
# Log token usage để monitor cost
def chat_with_tracking(messages, system=None):
    response = client.messages.create(...)
    
    # Log để Grafana/CloudWatch theo dõi
    logger.info("token_usage", extra={
        "input_tokens": response.usage.input_tokens,
        "output_tokens": response.usage.output_tokens,
        "estimated_cost_usd": (
            response.usage.input_tokens * 0.000003 +
            response.usage.output_tokens * 0.000015
        )
    })
    return response.content[0].text
```

---

## .env.example

```bash
# Anthropic
ANTHROPIC_API_KEY=sk-ant-your-key-here

# Model config
MODEL=claude-sonnet-4-20250514
MAX_TOKENS=1024

# App config
LOG_LEVEL=INFO
ENVIRONMENT=development
```

---

## CI/CD pipeline

```yaml
# .github/workflows/ci.yml
steps:
  - name: Lint
    run: ruff check src/

  - name: Type check
    run: mypy src/

  - name: Unit tests (không cần API key)
    run: pytest tests/unit/ --mock-anthropic

  - name: Integration tests (cần API key từ CI secret)
    run: pytest tests/integration/
    env:
      ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}

  - name: Build Docker image
    run: docker build -t my-claude-app .
```
