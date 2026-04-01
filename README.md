# OpenX API

### Real-time X/Twitter Data for Your AI Applications

### 为你的 AI 应用提供实时 X/Twitter 数据

[![OpenAI Compatible](https://img.shields.io/badge/OpenAI-Compatible-10a37f?style=flat-square&logo=openai&logoColor=white)](https://api.gugou.ai)
[![Streaming](https://img.shields.io/badge/SSE-Streaming-blue?style=flat-square)](https://api.gugou.ai)
[![API Status](https://img.shields.io/badge/Status-Live-brightgreen?style=flat-square)](https://api.gugou.ai)

**[English](#english)** | **[中文](#中文)**

---

<a id="english"></a>

## English

OpenX API turns X/Twitter into a structured, real-time data source that works with any OpenAI-compatible client. Search tweets, discover users, pull follower graphs — all through the standard `/v1/chat/completions` interface your AI stack already speaks.

**No new SDK. No learning curve. Just swap the base URL.**

---

### Why OpenX

| | |
|---|---|
| **OpenAI Compatible** | Drop-in replacement — works with any OpenAI SDK, LangChain, or HTTP client |
| **Real-time Data** | Live X/Twitter data, not cached or stale datasets |
| **Structured Output** | Clean JSON with full tweet metadata, user profiles, media links |
| **Streaming Support** | Server-Sent Events (SSE) for progressive result delivery |
| **Production Ready** | High-availability infrastructure with enterprise-grade uptime and auto-scaling |

---

### How It Works

```
Your Application                  OpenX API                        X / Twitter
      │                               │                               │
      │  POST /v1/chat/completions     │                               │
      │  model: "x-search-tweets"      │                               │
      │  ─────────────────────────▶    │                               │
      │                               │   Real-time data query          │
      │                               │   ─────────────────────────▶   │
      │                               │                               │
      │                               │   ◀───── Raw response ──────  │
      │                               │                               │
      │                               │   Parse & structure data       │
      │                               │   Wrap in OpenAI format        │
      │                               │                               │
      │   ◀── OpenAI-format JSON ──── │                               │
      │   (or SSE stream chunks)       │                               │
```

1. **You send a standard OpenAI request** — same format you already use for GPT/Claude, just change the `model` and `base_url`
2. **OpenX routes the request** through its distributed resource layer with smart load balancing, automatically avoiding throttled paths
3. **Queries X in real-time** with proper authentication and anti-detection measures
4. **Parses and structures** the raw response into clean, typed JSON objects
5. **Returns data in OpenAI format** — compatible with every tool in the ecosystem, including billing metadata

You never touch the X API directly. No API keys from X, no OAuth flows, no rate limit headaches.

---

### Security

| Aspect | Detail |
|--------|--------|
| **Transport** | All API traffic encrypted via HTTPS / TLS 1.3 |
| **Authentication** | Bearer token authentication on every request |
| **Key Isolation** | Each user gets an independent API key with separate quota tracking |
| **No Data Storage** | OpenX does not store or cache your query results — data flows through in real-time |
| **Credential Security** | All internal credentials are encrypted and managed server-side; never exposed to users |
| **Rate Protection** | Built-in per-key rate limiting prevents abuse and protects service stability |

---

### Stability & Reliability

| Feature | Description |
|---------|-------------|
| **Distributed Resource Pool** | Large-scale resource rotation ensures continuous availability even under heavy load |
| **Auto Recovery** | Throttled or degraded resources are automatically detected and rotated out; healthy nodes fill in seamlessly |
| **Smart Rotation** | Intelligent load balancing distributes requests evenly, preventing resource exhaustion |
| **Multi-Worker** | Uvicorn multi-process architecture handles concurrent requests without blocking |
| **Health Monitoring** | Built-in health check endpoint with real-time account pool statistics |
| **Retry Logic** | Automatic multi-layer retries per request — if one path fails, the next picks up instantly |
| **99.5%+ Uptime** | Production SLA-grade infrastructure with auto-restart and container orchestration |

---

### Compatibility

OpenX API follows the OpenAI Chat Completions specification. Any tool, framework, or library that speaks OpenAI can use OpenX with zero code changes — just set your `base_url` and `api_key`.

#### Verified Compatible Tools & Frameworks

| Category | Tools |
|----------|-------|
| **Official SDKs** | OpenAI Python SDK, OpenAI Node.js SDK |
| **LLM Frameworks** | LangChain, LlamaIndex, Semantic Kernel, Haystack |
| **Agent Frameworks** | AutoGPT, CrewAI, MetaGPT, BabyAGI |
| **Chat UIs** | ChatBox, LobeChat, NextChat (ChatGPT-Next-Web), Open WebUI |
| **API Platforms** | New API, One API, FastGPT, Dify |
| **HTTP Clients** | cURL, Postman, Insomnia, HTTPie |
| **Custom Code** | Any language with HTTP support (Python requests, Node fetch, Go net/http, etc.) |

#### Integration Examples

**LangChain (Python)**

```python
from langchain_openai import ChatOpenAI

llm = ChatOpenAI(
    model="x-search-tweets",
    api_key="YOUR_API_KEY",
    base_url="https://api.gugou.ai/v1"
)

result = llm.invoke('{"query": "AI startup funding", "limit": 10, "product": "Latest"}')
print(result.content)
```

**Dify / FastGPT**

Add a custom model provider:
- **Base URL**: `https://api.gugou.ai/v1`
- **API Key**: Your OpenX API key
- **Model**: `x-search-tweets` (or any other model)

That's it. OpenX models will appear in your model selector alongside GPT, Claude, etc.

**LobeChat / NextChat**

Settings → Model Provider → Custom:
- API Endpoint: `https://api.gugou.ai`
- API Key: `YOUR_API_KEY`
- Add models: `x-search-tweets`, `x-search-users`, `x-user-tweets`, `x-user-followers`, `x-user-following`

---

### Available Models

| Model | Description | Key Parameters |
|-------|-------------|----------------|
| `x-search-tweets` | Search tweets by keyword or query expression | `query`, `limit`, `product` (Top/Latest/Photos/Videos) |
| `x-search-users` | Discover X users by keyword | `query`, `limit` |
| `x-user-tweets` | Fetch tweets from a specific user | `user_id`, `limit` |
| `x-user-followers` | Get a user's follower list | `user_id`, `limit` |
| `x-user-following` | Get accounts a user follows | `user_id`, `limit` |

---

### Quick Start

#### 1. Get your API key

Sign up at [api.gugou.ai](https://api.gugou.ai) to get your API key.

#### 2. Make your first request

**cURL**

```bash
curl -X POST https://api.gugou.ai/v1/chat/completions \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "x-search-tweets",
    "messages": [
      {
        "role": "user",
        "content": "{\"query\": \"bitcoin\", \"limit\": 5, \"product\": \"Latest\"}"
      }
    ]
  }'
```

**Python (OpenAI SDK)**

```python
from openai import OpenAI

client = OpenAI(
    api_key="YOUR_API_KEY",
    base_url="https://api.gugou.ai/v1"
)

response = client.chat.completions.create(
    model="x-search-tweets",
    messages=[
        {
            "role": "user",
            "content": '{"query": "AI agents", "limit": 10, "product": "Latest"}'
        }
    ]
)

tweets = response.choices[0].message.content
print(tweets)
```

**Node.js (OpenAI SDK)**

```javascript
import OpenAI from "openai";

const client = new OpenAI({
  apiKey: "YOUR_API_KEY",
  baseURL: "https://api.gugou.ai/v1",
});

const response = await client.chat.completions.create({
  model: "x-search-tweets",
  messages: [
    {
      role: "user",
      content: '{"query": "Web3", "limit": 10, "product": "Latest"}',
    },
  ],
});

console.log(response.choices[0].message.content);
```

**Streaming (Python)**

```python
stream = client.chat.completions.create(
    model="x-search-tweets",
    messages=[
        {"role": "user", "content": '{"query": "breaking news", "limit": 20}'}
    ],
    stream=True
)

for chunk in stream:
    if chunk.choices[0].delta.content:
        print(chunk.choices[0].delta.content, end="")
```

---

### Response Example

Each tweet is returned as a structured JSON object with rich metadata:

```json
{
  "id": 2038663014098899416,
  "url": "https://x.com/OpenAI/status/2038663014098899416",
  "date": "Mon Mar 30 17:01:53 +0000 2026",
  "user": {
    "username": "OpenAI",
    "displayname": "OpenAI",
    "followersCount": 4701924,
    "blue": true
  },
  "rawContent": "We're releasing a new eval to measure expert-level scientific reasoning...",
  "replyCount": 109,
  "retweetCount": 235,
  "likeCount": 2041,
  "viewCount": 948536,
  "media": {
    "photos": [],
    "videos": [{"thumbnailUrl": "...", "variants": [...]}]
  },
  "hashtags": [],
  "mentionedUsers": [],
  "quotedTweet": null
}
```

**Response fields include:** tweet ID, URL, timestamp, full user profile (username, display name, followers, verified status), raw content text, engagement metrics (replies, retweets, likes, views, bookmarks), media attachments (photos, videos, GIFs), hashtags, mentioned users, quoted/replied tweets, source app, and more.

---

### Use Cases

| Scenario | How OpenX Helps |
|----------|----------------|
| **AI Agents** | Give your agent real-time awareness of social media trends, breaking news, and public sentiment |
| **RAG Systems** | Enrich your retrieval pipeline with live X data for up-to-the-minute context |
| **Crypto / DeFi** | Monitor KOL opinions, token mentions, whale wallet alerts, and market sentiment in real-time |
| **Brand Monitoring** | Track brand mentions, competitor activity, campaign performance, and audience reactions |
| **Research & Analytics** | Collect structured social data for NLP training, sentiment analysis, and trend research |
| **Influencer Analysis** | Map follower graphs, discover key opinion leaders, and analyze influence networks |
| **News & Media** | Aggregate breaking news, verify sources, and track story evolution across social media |
| **Recruitment & HR** | Discover talent, track industry thought leaders, and monitor employer brand perception |

---

### API Endpoints

| Method | Path | Description |
|--------|------|-------------|
| `POST` | `/v1/chat/completions` | Main data retrieval endpoint (OpenAI compatible) |
| `GET` | `/v1/models` | List available models |
| `GET` | `/health` | Service health check |

---

### Request Parameters

The `content` field in your message accepts a JSON string with these parameters:

**Search models** (`x-search-tweets`, `x-search-users`):

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `query` | string | Yes | Search keyword or X advanced search expression |
| `limit` | integer | No | Number of results (1-500, default: 20) |
| `product` | string | No | Search type: `Top`, `Latest`, `People`, `Photos`, `Videos` (default: `Top`) |

**User models** (`x-user-tweets`, `x-user-followers`, `x-user-following`):

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `user_id` | integer | Yes | X user numeric ID |
| `limit` | integer | No | Number of results (1-500, default: 20) |

> **Tip:** You can also send plain text as the content — it will be treated as a `query` for search models.

---

### FAQ

<details>
<summary><b>Do I need an X/Twitter developer account?</b></summary>
No. OpenX handles all X/Twitter data access internally. You only need an OpenX API key.
</details>

<details>
<summary><b>What's the rate limit?</b></summary>
Rate limits depend on your plan tier. The service handles X's internal throttling automatically — if one path is limited, the system reroutes seamlessly with zero downtime.
</details>

<details>
<summary><b>Is the data real-time?</b></summary>
Yes. Every request queries X/Twitter live. There is no caching layer — you always get the latest data.
</details>

<details>
<summary><b>Can I use this with my existing OpenAI code?</b></summary>
Absolutely. Just change the <code>base_url</code> to <code>https://api.gugou.ai/v1</code> and use an OpenX model name. Everything else stays the same.
</details>

<details>
<summary><b>What happens if all accounts are rate-limited?</b></summary>
With our large-scale distributed resource pool, this is extremely rare. If it happens, the API returns a clear error with retry-after timing. Resources automatically recover when X's throttling window resets.
</details>

<details>
<summary><b>Do you support webhooks / callbacks?</b></summary>
Not yet. Currently the API is request-response based (synchronous and streaming). Webhook-based push notifications for monitored keywords or users is on the roadmap.
</details>

---

### Documentation

Full API documentation is available at [docs.gugou.ai](https://docs.gugou.ai).

---

---

<a id="中文"></a>

## 中文

OpenX API 将 X/Twitter 的海量实时数据转化为结构化输出，完全兼容 OpenAI 接口协议。搜索推文、发现用户、获取粉丝关系图谱 —— 全部通过标准的 `/v1/chat/completions` 接口完成，你现有的 AI 技术栈无需任何改动。

**无需新 SDK，零学习成本，只需替换 Base URL。**

---

### 为什么选择 OpenX

| | |
|---|---|
| **OpenAI 协议兼容** | 即插即用 —— 支持所有 OpenAI SDK、LangChain 及任意 HTTP 客户端 |
| **实时数据** | 直连 X/Twitter 实时数据，非缓存或过期数据集 |
| **结构化输出** | 干净的 JSON 格式，包含完整推文元数据、用户资料、媒体链接 |
| **流式传输** | 支持 Server-Sent Events (SSE) 逐步返回结果 |
| **生产可用** | 高可用基础设施，企业级稳定性与弹性扩展 |

---

### 工作原理

```
你的应用程序                       OpenX API                        X / Twitter
      │                               │                               │
      │  POST /v1/chat/completions     │                               │
      │  model: "x-search-tweets"      │                               │
      │  ─────────────────────────▶    │                               │
      │                               │   实时数据查询                   │
      │                               │   ─────────────────────────▶   │
      │                               │                               │
      │                               │   ◀───── 原始响应数据 ──────   │
      │                               │                               │
      │                               │   解析 & 结构化处理             │
      │                               │   封装为 OpenAI 格式            │
      │                               │                               │
      │   ◀── OpenAI 格式 JSON ────   │                               │
      │   (或 SSE 流式推送)            │                               │
```

1. **你发送标准 OpenAI 请求** —— 和调用 GPT/Claude 完全一样，只需更换 `model` 和 `base_url`
2. **OpenX 通过分布式资源层路由请求**，智能负载均衡，自动绕过被限速的路径
3. **实时查询 X 数据**，自动处理身份认证与反检测策略
4. **解析并结构化**原始响应，转换为干净的、带类型的 JSON 对象
5. **以 OpenAI 格式返回数据** —— 兼容生态中的所有工具，包含计费元数据

你无需直接接触 X API。不需要 X 的 API 密钥，不需要 OAuth 授权流程，不需要处理速率限制。

---

### 安全性

| 方面 | 说明 |
|------|------|
| **传输加密** | 全部 API 流量通过 HTTPS / TLS 1.3 加密传输 |
| **身份认证** | 每个请求都需要 Bearer Token 认证 |
| **密钥隔离** | 每个用户拥有独立 API Key，配额独立追踪 |
| **无数据存储** | OpenX 不存储或缓存你的查询结果 —— 数据实时流转 |
| **凭证安全** | 所有内部凭证在服务端加密管理，永远不会暴露给用户 |
| **速率保护** | 内置按 Key 的速率限制，防止滥用并保护服务稳定性 |

---

### 稳定性与可靠性

| 特性 | 说明 |
|------|------|
| **分布式资源池** | 大规模资源轮转，即使在高负载下也能保持持续可用 |
| **自动恢复** | 被限速或降级的资源自动检测并下线，健康节点无缝补位 |
| **智能轮转** | 智能负载均衡策略，均匀分配请求，防止单点资源过度消耗 |
| **多进程架构** | Uvicorn 多 Worker 架构，并发请求互不阻塞 |
| **健康监控** | 内置健康检查端点，实时账号池状态统计 |
| **重试机制** | 每个请求自动多层重试 —— 一条路径失败，下一条立即接管 |
| **99.5%+ 可用性** | 生产级 SLA 基础设施，自动重启与容器编排 |

---

### 兼容性

OpenX API 遵循 OpenAI Chat Completions 规范。任何支持 OpenAI 的工具、框架或库都可以零代码改动使用 OpenX —— 只需设置 `base_url` 和 `api_key`。

#### 已验证兼容的工具与框架

| 类别 | 工具 |
|------|------|
| **官方 SDK** | OpenAI Python SDK、OpenAI Node.js SDK |
| **LLM 框架** | LangChain、LlamaIndex、Semantic Kernel、Haystack |
| **Agent 框架** | AutoGPT、CrewAI、MetaGPT、BabyAGI |
| **聊天 UI** | ChatBox、LobeChat、NextChat (ChatGPT-Next-Web)、Open WebUI |
| **API 管理平台** | New API、One API、FastGPT、Dify |
| **HTTP 客户端** | cURL、Postman、Insomnia、HTTPie |
| **自定义代码** | 任何支持 HTTP 的语言（Python requests、Node fetch、Go net/http 等） |

#### 集成示例

**LangChain (Python)**

```python
from langchain_openai import ChatOpenAI

llm = ChatOpenAI(
    model="x-search-tweets",
    api_key="YOUR_API_KEY",
    base_url="https://api.gugou.ai/v1"
)

result = llm.invoke('{"query": "AI 创业融资", "limit": 10, "product": "Latest"}')
print(result.content)
```

**Dify / FastGPT**

添加自定义模型供应商：
- **Base URL**：`https://api.gugou.ai/v1`
- **API Key**：你的 OpenX API Key
- **模型名称**：`x-search-tweets`（或其他任意模型）

配置完成后，OpenX 模型会与 GPT、Claude 等一起出现在模型选择器中。

**LobeChat / NextChat**

设置 → 模型供应商 → 自定义：
- API 端点：`https://api.gugou.ai`
- API Key：`YOUR_API_KEY`
- 添加模型：`x-search-tweets`、`x-search-users`、`x-user-tweets`、`x-user-followers`、`x-user-following`

---

### 可用模型

| 模型 | 功能说明 | 关键参数 |
|------|---------|---------|
| `x-search-tweets` | 按关键词或查询语句搜索推文 | `query`、`limit`、`product`（Top/Latest/Photos/Videos） |
| `x-search-users` | 按关键词搜索 X 用户 | `query`、`limit` |
| `x-user-tweets` | 获取指定用户发布的推文 | `user_id`、`limit` |
| `x-user-followers` | 获取用户的粉丝列表 | `user_id`、`limit` |
| `x-user-following` | 获取用户的关注列表 | `user_id`、`limit` |

---

### 快速上手

#### 1. 获取 API Key

访问 [api.gugou.ai](https://api.gugou.ai) 注册并获取你的 API Key。

#### 2. 发送第一个请求

**cURL**

```bash
curl -X POST https://api.gugou.ai/v1/chat/completions \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "x-search-tweets",
    "messages": [
      {
        "role": "user",
        "content": "{\"query\": \"比特币\", \"limit\": 5, \"product\": \"Latest\"}"
      }
    ]
  }'
```

**Python (OpenAI SDK)**

```python
from openai import OpenAI

client = OpenAI(
    api_key="YOUR_API_KEY",
    base_url="https://api.gugou.ai/v1"
)

response = client.chat.completions.create(
    model="x-search-tweets",
    messages=[
        {
            "role": "user",
            "content": '{"query": "AI 智能体", "limit": 10, "product": "Latest"}'
        }
    ]
)

tweets = response.choices[0].message.content
print(tweets)
```

**Node.js (OpenAI SDK)**

```javascript
import OpenAI from "openai";

const client = new OpenAI({
  apiKey: "YOUR_API_KEY",
  baseURL: "https://api.gugou.ai/v1",
});

const response = await client.chat.completions.create({
  model: "x-search-tweets",
  messages: [
    {
      role: "user",
      content: '{"query": "Web3", "limit": 10, "product": "Latest"}',
    },
  ],
});

console.log(response.choices[0].message.content);
```

**流式请求 (Python)**

```python
stream = client.chat.completions.create(
    model="x-search-tweets",
    messages=[
        {"role": "user", "content": '{"query": "突发新闻", "limit": 20}'}
    ],
    stream=True
)

for chunk in stream:
    if chunk.choices[0].delta.content:
        print(chunk.choices[0].delta.content, end="")
```

---

### 返回数据示例

每条推文以结构化 JSON 对象返回，包含丰富的元数据：

```json
{
  "id": 2038663014098899416,
  "url": "https://x.com/OpenAI/status/2038663014098899416",
  "date": "Mon Mar 30 17:01:53 +0000 2026",
  "user": {
    "username": "OpenAI",
    "displayname": "OpenAI",
    "followersCount": 4701924,
    "blue": true
  },
  "rawContent": "We're releasing a new eval to measure expert-level scientific reasoning...",
  "replyCount": 109,
  "retweetCount": 235,
  "likeCount": 2041,
  "viewCount": 948536,
  "media": {
    "photos": [],
    "videos": [{"thumbnailUrl": "...", "variants": [...]}]
  },
  "hashtags": [],
  "mentionedUsers": [],
  "quotedTweet": null
}
```

**返回字段包括：** 推文 ID、URL、发布时间、完整用户资料（用户名、昵称、粉丝数、认证状态）、正文内容、互动数据（回复、转发、点赞、浏览、收藏）、媒体附件（图片、视频、GIF）、话题标签、提及用户、引用/回复推文、来源应用等。

---

### 应用场景

| 场景 | OpenX 如何帮助 |
|------|--------------|
| **AI Agent** | 赋予智能体实时感知社交媒体动态、突发新闻和公众情绪的能力 |
| **RAG 系统** | 用实时 X 数据增强检索管线，获取最新上下文 |
| **加密货币 / DeFi** | 实时监控 KOL 观点、代币提及、巨鲸钱包动态和市场情绪 |
| **品牌监控** | 追踪品牌提及、竞品动态、营销活动效果和用户反馈 |
| **学术研究** | 采集结构化社交数据用于 NLP 训练、情感分析和趋势研究 |
| **KOL 分析** | 绘制粉丝关系图谱，发现关键意见领袖，分析影响力网络 |
| **新闻媒体** | 聚合突发新闻、验证信源、追踪社交媒体上的事件演变 |
| **人才招聘** | 发现人才、追踪行业领袖、监测雇主品牌口碑 |

---

### API 接口

| 方法 | 路径 | 说明 |
|------|------|------|
| `POST` | `/v1/chat/completions` | 主数据检索接口（OpenAI 兼容） |
| `GET` | `/v1/models` | 获取可用模型列表 |
| `GET` | `/health` | 服务健康检查 |

---

### 请求参数说明

`messages` 中 `content` 字段接受 JSON 字符串，支持以下参数：

**搜索类模型**（`x-search-tweets`、`x-search-users`）：

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `query` | string | 是 | 搜索关键词或 X 高级搜索表达式 |
| `limit` | integer | 否 | 返回结果数量（1-500，默认 20） |
| `product` | string | 否 | 搜索类型：`Top`、`Latest`、`People`、`Photos`、`Videos`（默认 `Top`） |

**用户类模型**（`x-user-tweets`、`x-user-followers`、`x-user-following`）：

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `user_id` | integer | 是 | X 用户数字 ID |
| `limit` | integer | 否 | 返回结果数量（1-500，默认 20） |

> **提示：** 你也可以直接发送纯文本作为 content，搜索类模型会将其作为 `query` 处理。

---

### 常见问题

<details>
<summary><b>需要 X/Twitter 开发者账号吗？</b></summary>
不需要。OpenX 在内部处理所有 X/Twitter 数据访问，你只需要一个 OpenX API Key。
</details>

<details>
<summary><b>速率限制是多少？</b></summary>
速率限制取决于你的套餐等级。服务内部自动处理 X 的限速机制 —— 如果一条路径受限，系统会自动重新路由，零停机时间。
</details>

<details>
<summary><b>数据是实时的吗？</b></summary>
是的。每个请求都直接查询 X/Twitter 实时数据，没有缓存层 —— 你始终获取最新数据。
</details>

<details>
<summary><b>可以用现有的 OpenAI 代码吗？</b></summary>
完全可以。只需将 <code>base_url</code> 改为 <code>https://api.gugou.ai/v1</code>，使用 OpenX 模型名称即可，其他代码无需任何修改。
</details>

<details>
<summary><b>所有账号都被限速了怎么办？</b></summary>
在我们大规模分布式资源池下，这种情况极为罕见。如果发生，API 会返回明确的错误信息和重试时间。资源会在 X 的限速窗口重置后自动恢复。
</details>

<details>
<summary><b>支持 Webhook / 回调吗？</b></summary>
暂不支持。目前 API 基于请求-响应模式（同步和流式）。基于 Webhook 的关键词和用户监控推送功能已在开发路线图中。
</details>

---

### 文档

完整 API 文档请访问 [docs.gugou.ai](https://docs.gugou.ai)。

---

## Contact / 联系我们

- Email: [feiqijie598@gmail.com](mailto:feiqijie598@gmail.com)
- Website: [api.gugou.ai](https://api.gugou.ai)

---

<p align="center">
  <sub>Empowering AI & LLM with Real-Time Social Data</sub>
</p>
