# Skills Builder — Hệ thống học kỹ năng & công nghệ IT

Project này là bộ tài liệu + code mẫu có cấu trúc để học và thực hành các kỹ năng/công nghệ trong lĩnh vực IT và phát triển phần mềm.

## Cách tổ chức

```
skills-builder/
├── CLAUDE.md                    # File này — tổng quan & hướng dẫn
├── ai/                          # Nhóm AI / Machine Learning
│   ├── bedrock-agentcore/
│   ├── strands-agent/
│   └── ...
├── agent/                       # Nhóm Agent Framework
│   ├── agentcore-harness/
│   └── ...
├── cloud/                       # Nhóm Cloud & Infrastructure
│   ├── aws-cdk/
│   └── ...
└── _template/                   # Template chuẩn cho mỗi skill
    ├── docs/
    └── project/
```

## Cấu trúc chuẩn của mỗi skill

```
<group>/<skill-name>/
├── docs/
│   ├── 01-overview.md           # Tổng quan công nghệ
│   ├── 02-case-studies.md       # Case study thực tế
│   ├── 03-alternatives.md       # Công nghệ tương đương / thay thế
│   ├── 04-related-tech.md       # Công nghệ liên quan / ecosystem
│   ├── 05-adoption-steps.md     # Các bước áp dụng vào ứng dụng
│   └── 06-project-structure.md  # Cấu trúc project chuyên nghiệp
└── project/                     # Code mẫu thực hành
    ├── README.md
    ├── src/
    ├── tests/
    └── ...
```

---

## Nhóm AI / Machine Learning

### Bedrock AgentCore

**Mô tả:** Dịch vụ managed của AWS để deploy, quản lý và scale AI agent trong production.

**Đường dẫn:** `ai/bedrock-agentcore/`

**Tài liệu:**
- `docs/01-overview.md` — Kiến trúc AgentCore, các thành phần (Memory, Tools, Gateway, Observability)
- `docs/02-case-studies.md` — Customer service bot, code review agent, data pipeline agent
- `docs/03-alternatives.md` — Azure AI Agent Service, Google Vertex AI Agent Builder, LangGraph Cloud
- `docs/04-related-tech.md` — Amazon Bedrock, Claude models, AWS Lambda, Amazon S3, CloudWatch
- `docs/05-adoption-steps.md` — Setup IAM, tạo agent runtime, định nghĩa tools, deploy và test
- `docs/06-project-structure.md` — Monorepo với CDK stack, agent modules, tool definitions

**Project mẫu:** `project/` — Customer support agent với memory và tool calling
- Stack: Python, AWS CDK, Bedrock AgentCore SDK
- Features: Multi-turn conversation, tool integration (search, calculator), session memory

---

### Strands Agent Framework

**Mô tả:** Framework Python mã nguồn mở của AWS để xây dựng AI agent với model-driven approach — agent tự quyết định khi nào dùng tool nào.

**Đường dẫn:** `ai/strands-agent/`

**Tài liệu:**
- `docs/01-overview.md` — Vòng lặp agent (think → act → observe), tool registry, model providers
- `docs/02-case-studies.md` — Research assistant, DevOps automation agent, document Q&A
- `docs/03-alternatives.md` — LangChain Agents, LlamaIndex Agents, AutoGen, CrewAI
- `docs/04-related-tech.md` — Amazon Bedrock, MCP (Model Context Protocol), FastAPI, Pydantic
- `docs/05-adoption-steps.md` — Cài đặt SDK, định nghĩa tools với @tool decorator, chạy agent loop
- `docs/06-project-structure.md` — Agent package, tools module, config, tests, deployment

**Project mẫu:** `project/` — Research & summarization agent
- Stack: Python, Strands SDK, Claude via Bedrock
- Features: Web search tool, file read/write tool, multi-step reasoning

---

### AgentCore Harness

**Mô tả:** Runtime harness để test và evaluate AI agent locally trước khi deploy lên Bedrock AgentCore — cung cấp môi trường sandbox với mock tools và observability.

**Đường dẫn:** `agent/agentcore-harness/`

**Tài liệu:**
- `docs/01-overview.md` — Kiến trúc harness, test runner, trace viewer, mock tool system
- `docs/02-case-studies.md` — Unit testing agent behavior, regression testing, load testing agent
- `docs/03-alternatives.md` — LangSmith, AgentOps, PromptFlow evaluation, Braintrust
- `docs/04-related-tech.md` — Bedrock AgentCore, pytest, OpenTelemetry, Grafana
- `docs/05-adoption-steps.md` — Cài harness CLI, viết test scenarios, chạy evaluation suite
- `docs/06-project-structure.md` — tests/agent/, fixtures/, evaluation metrics, CI integration

**Project mẫu:** `project/` — Test suite cho customer support agent
- Stack: Python, AgentCore Harness, pytest
- Features: Scenario-based testing, tool mock injection, trace assertion

---

## Nhóm Agent Framework

### LangGraph

**Mô tả:** Framework để xây dựng stateful, multi-actor AI agent dưới dạng directed graph — mỗi node là một bước xử lý, edge là luồng điều kiện.

**Đường dẫn:** `agent/langgraph/`

**Tài liệu:**
- `docs/01-overview.md` — StateGraph, nodes, edges, conditional routing, checkpointing
- `docs/02-case-studies.md` — Multi-agent coding assistant, document review pipeline, RAG agent
- `docs/03-alternatives.md` — Strands Agent, AutoGen, CrewAI, Semantic Kernel
- `docs/04-related-tech.md` — LangChain, LangSmith, Redis (state store), FastAPI
- `docs/05-adoption-steps.md` — Define state schema, add nodes, wire edges, add memory/persistence
- `docs/06-project-structure.md` — graph/, nodes/, tools/, state/, tests/, server/

**Project mẫu:** `project/` — Code review agent với human-in-the-loop
- Stack: Python, LangGraph, LangSmith, FastAPI
- Features: Interrupt for human approval, multi-step review, structured output

---

### CrewAI

**Mô tả:** Framework để điều phối nhiều AI agent làm việc theo role — mỗi agent có vai trò, mục tiêu, backstory riêng và cộng tác để hoàn thành task phức tạp.

**Đường dẫn:** `agent/crewai/`

**Tài liệu:**
- `docs/01-overview.md` — Crew, Agent, Task, Process (sequential/hierarchical), Tools
- `docs/02-case-studies.md` — Content marketing pipeline, market research team, software dev crew
- `docs/03-alternatives.md` — AutoGen, Swarm, LangGraph multi-agent, Semantic Kernel
- `docs/04-related-tech.md` — LangChain tools, OpenAI / Anthropic API, Composio
- `docs/05-adoption-steps.md` — Định nghĩa agents + roles, tạo tasks, cấu hình crew, kickoff
- `docs/06-project-structure.md` — agents/, tasks/, tools/, crews/, config/agents.yaml

**Project mẫu:** `project/` — Content creation crew (researcher + writer + editor)
- Stack: Python, CrewAI, Claude API
- Features: Sequential process, tool use (web search), structured output

---

## Nhóm Cloud & Infrastructure

### AWS CDK (Cloud Development Kit)

**Mô tả:** Framework IaC (Infrastructure as Code) cho phép định nghĩa AWS infrastructure bằng ngôn ngữ lập trình (Python, TypeScript) thay vì YAML/JSON.

**Đường dẫn:** `cloud/aws-cdk/`

**Tài liệu:**
- `docs/01-overview.md` — Constructs (L1/L2/L3), Stacks, Apps, CDK CLI, synthesize → deploy
- `docs/02-case-studies.md` — Serverless API, ECS microservice, data lake infrastructure
- `docs/03-alternatives.md` — Terraform, Pulumi, AWS SAM, CloudFormation (native)
- `docs/04-related-tech.md` — AWS CloudFormation, AWS CLI, LocalStack (local dev)
- `docs/05-adoption-steps.md` — Bootstrap CDK, tạo stack, định nghĩa constructs, cdk deploy
- `docs/06-project-structure.md` — stacks/, constructs/, bin/app.py, cdk.json, tests/

**Project mẫu:** `project/` — Serverless API với Lambda + API Gateway + DynamoDB
- Stack: Python CDK, Lambda, DynamoDB, API Gateway, CloudWatch
- Features: Environment configs, unit tests cho constructs, snapshot testing

---

### Model Context Protocol (MCP)

**Mô tả:** Giao thức mã nguồn mở (Anthropic) để kết nối AI model với external tools và data sources theo chuẩn thống nhất — thay thế custom tool integration.

**Đường dẫn:** `cloud/mcp/`

**Tài liệu:**
- `docs/01-overview.md` — MCP Server, MCP Client, Resources, Tools, Prompts, Transport
- `docs/02-case-studies.md` — Database query tool, GitHub integration, file system access, Slack bot
- `docs/03-alternatives.md` — LangChain tools, OpenAI function calling, custom API integration
- `docs/04-related-tech.md` — Claude API, FastMCP, Anthropic SDK, JSON-RPC
- `docs/05-adoption-steps.md` — Cài MCP SDK, định nghĩa tools/resources, expose server, connect client
- `docs/06-project-structure.md` — server/, tools/, resources/, tests/, Dockerfile

**Project mẫu:** `project/` — MCP server cho PostgreSQL database
- Stack: Python, FastMCP, asyncpg, Claude Desktop (client)
- Features: Query execution, schema inspection, safe parameterized queries

---

## Nhóm Backend & API

### FastAPI

**Mô tả:** Framework Python hiện đại để build REST API với type hints, async support và auto-generated OpenAPI docs.

**Đường dẫn:** `backend/fastapi/`

**Tài liệu:**
- `docs/01-overview.md` — Router, dependency injection, Pydantic models, middleware, lifespan
- `docs/02-case-studies.md` — Microservice API, ML model serving, real-time chat backend
- `docs/03-alternatives.md` — Django REST Framework, Flask, Litestar, Starlette
- `docs/04-related-tech.md` — Pydantic, SQLAlchemy, Alembic, uvicorn, pytest-asyncio
- `docs/05-adoption-steps.md` — Project setup, define models, routers, auth, tests, containerize
- `docs/06-project-structure.md` — app/, routers/, models/, services/, core/, tests/, Dockerfile

**Project mẫu:** `project/` — Task management API với auth và pagination
- Stack: Python, FastAPI, PostgreSQL, SQLAlchemy, JWT
- Features: CRUD operations, JWT auth, pagination, async DB, pytest test suite

---

## Template chuẩn

Khi thêm skill mới, sao chép `_template/` và điền nội dung theo cấu trúc sau.

### `docs/01-overview.md`
- Skill/công nghệ là gì, ra đời khi nào, ai tạo ra
- Kiến trúc tổng quan (diagram text/mermaid)
- Các khái niệm cốt lõi (glossary)
- Khi nào nên dùng / không nên dùng

### `docs/02-case-studies.md`
- Tối thiểu 3 case study thực tế hoặc realistic scenario
- Mỗi case: bối cảnh → vấn đề → giải pháp với skill này → kết quả

### `docs/03-alternatives.md`
- Bảng so sánh các công nghệ tương đương
- Trade-off: ưu/nhược từng lựa chọn
- Hướng dẫn chọn cái nào trong trường hợp nào

### `docs/04-related-tech.md`
- Các công nghệ trong cùng ecosystem
- Dependency và integration patterns
- Công nghệ thường được dùng kết hợp

### `docs/05-adoption-steps.md`
- Prerequisite (kiến thức, tool cần có trước)
- Step-by-step từ zero đến working prototype
- Checklist để đưa vào production

### `docs/06-project-structure.md`
- Cấu trúc thư mục chuẩn
- Giải thích vai trò từng file/folder quan trọng
- Naming conventions và best practices

### `project/README.md`
- Mô tả project mẫu
- Prerequisite để chạy
- Hướng dẫn setup và chạy
- Các tính năng được demo

---

## Quy ước đặt tên

| Loại | Quy ước | Ví dụ |
|------|---------|-------|
| Thư mục group | kebab-case | `ai/`, `agent/`, `cloud/` |
| Thư mục skill | kebab-case | `bedrock-agentcore/`, `langgraph/` |
| File doc | `NN-kebab-case.md` | `01-overview.md` |
| File Python | snake_case | `agent_runner.py` |
| File TypeScript | camelCase | `agentRunner.ts` |

## Thứ tự học đề xuất

```
1. Nền tảng API & Backend
   fastapi → mcp

2. Cloud Infrastructure
   aws-cdk

3. AI Agent - cơ bản
   strands-agent → langgraph

4. AI Agent - nâng cao
   crewai → bedrock-agentcore → agentcore-harness
```

## Thêm skill mới

```bash
# 1. Tạo cấu trúc thư mục
mkdir -p <group>/<skill-name>/{docs,project/src,project/tests}

# 2. Copy template
cp _template/docs/*.md <group>/<skill-name>/docs/
cp _template/project/README.md <group>/<skill-name>/project/

# 3. Điền nội dung theo cấu trúc chuẩn ở trên

# 4. Cập nhật CLAUDE.md — thêm entry vào nhóm tương ứng
```
