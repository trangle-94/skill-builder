# Claude — Công nghệ liên quan & Ecosystem

## Sơ đồ ecosystem

```
Claude
├── Depends on: Anthropic API, Python/JS SDK
├── Integrates with: LangChain, LlamaIndex, FastAPI, MCP
├── Often used with: Vector DB (Pinecone, pgvector), Redis, PostgreSQL
└── Competes with: GPT-4, Gemini (xem docs/03-alternatives.md)
```

---

## Công nghệ phụ thuộc (Dependencies)

### Anthropic Python SDK

**Vai trò:** Thư viện chính thức để gọi Claude API từ Python  
**Cần biết:** Python cơ bản, khái niệm async/await

```bash
pip install anthropic
```

```python
from anthropic import Anthropic

client = Anthropic()  # Tự động đọc ANTHROPIC_API_KEY từ env
message = client.messages.create(
    model="claude-sonnet-4-20250514",
    max_tokens=1024,
    messages=[{"role": "user", "content": "Hello!"}]
)
```

### Anthropic TypeScript/JS SDK

**Vai trò:** SDK cho Node.js và browser  
**Cần biết:** TypeScript, async/await, Node.js

```bash
npm install @anthropic-ai/sdk
```

---

## Công nghệ tích hợp thường gặp

### LangChain — Orchestration framework

**Pattern tích hợp:** Dùng Claude như LLM backend trong chain hoặc agent

```python
from langchain_anthropic import ChatAnthropic

llm = ChatAnthropic(model="claude-sonnet-4-20250514")
response = llm.invoke("Giải thích RAG là gì?")
```

**Dùng khi:** Cần chain phức tạp, tích hợp nhiều tool, team đã quen LangChain ecosystem

### LlamaIndex — Data framework cho RAG

**Pattern tích hợp:** Claude làm LLM, LlamaIndex lo việc index và retrieve

```python
from llama_index.llms.anthropic import Anthropic

llm = Anthropic(model="claude-sonnet-4-20250514")
# Dùng trong RAG pipeline để query documents
```

**Dùng khi:** Cần build RAG trên knowledge base lớn (PDF, database, website)

### FastAPI — Backend API framework

**Pattern tích hợp:** Wrap Claude API thành REST endpoint của riêng mình

```python
from fastapi import FastAPI
from anthropic import Anthropic

app = FastAPI()
client = Anthropic()

@app.post("/chat")
async def chat(message: str):
    response = client.messages.create(
        model="claude-sonnet-4-20250514",
        max_tokens=1000,
        messages=[{"role": "user", "content": message}]
    )
    return {"reply": response.content[0].text}
```

### Model Context Protocol (MCP)

**Pattern tích hợp:** Chuẩn giao tiếp giữa Claude và external tools/data sources

```python
# Claude tự động gọi MCP server khi cần
# Developer chỉ cần định nghĩa MCP server với tools và resources
```

**Dùng khi:** Cần Claude tương tác với database, filesystem, API bên ngoài theo chuẩn thống nhất

### Pinecone / pgvector — Vector Database

**Pattern tích hợp:** Lưu embeddings của documents, retrieve context cho Claude

```python
# Flow RAG:
# 1. Embed query bằng embedding model
# 2. Search vector DB tìm docs liên quan  
# 3. Inject docs vào prompt của Claude
# 4. Claude trả lời dựa trên context
```

---

## Stack phổ biến

**Stack 1: Chatbot đơn giản**
```
Claude API + FastAPI + React
→ Dùng cho: Internal tool, demo, MVP chatbot
```

**Stack 2: RAG Application**
```
Claude + LlamaIndex + pgvector + PostgreSQL + FastAPI
→ Dùng cho: Document Q&A, knowledge base assistant
```

**Stack 3: Agentic Workflow**
```
Claude + MCP + LangGraph + Redis (state) + FastAPI
→ Dùng cho: Tự động hóa workflow phức tạp, multi-step task
```

**Stack 4: Production Scale**
```
Claude API + AWS Lambda + API Gateway + DynamoDB + CloudWatch
→ Dùng cho: High-traffic, serverless, pay-per-use
```

---

## Tài nguyên học công nghệ liên quan

- Anthropic SDK docs: https://docs.anthropic.com/en/api
- LangChain + Claude: https://python.langchain.com/docs/integrations/llms/anthropic
- LlamaIndex + Claude: https://docs.llamaindex.ai/en/stable/examples/llm/anthropic/
- MCP Protocol: https://modelcontextprotocol.io
- Anthropic Cookbook: https://github.com/anthropics/anthropic-cookbook
