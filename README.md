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

### Why OpenX

| | |
|---|---|
| **OpenAI Compatible** | Drop-in replacement — works with any OpenAI SDK, LangChain, or HTTP client |
| **Real-time Data** | Live X/Twitter data, not cached or stale datasets |
| **Structured Output** | Clean JSON with full tweet metadata, user profiles, media links |
| **Streaming Support** | Server-Sent Events (SSE) for progressive result delivery |
| **Production Ready** | High-availability infrastructure with 500+ account pool rotation |

### Available Models

| Model | Description | Key Parameters |
|-------|-------------|----------------|
| `x-search-tweets` | Search tweets by keyword or query expression | `query`, `limit`, `product` (Top/Latest/Photos/Videos) |
| `x-search-users` | Discover X users by keyword | `query`, `limit` |
| `x-user-tweets` | Fetch tweets from a specific user | `user_id`, `limit` |
| `x-user-followers` | Get a user's follower list | `user_id`, `limit` |
| `x-user-following` | Get accounts a user follows | `user_id`, `limit` |

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

### Use Cases

- **AI Agents** — Give your agent real-time awareness of social media trends
- **RAG Systems** — Enrich your retrieval pipeline with live X data
- **Crypto / Finance** — Monitor KOL opinions, token mentions, and market sentiment
- **Brand Monitoring** — Track mentions, competitor activity, and audience reactions
- **Research** — Collect structured social data for NLP and analytics projects
- **Influencer Analysis** — Map follower graphs and discover key opinion leaders

### API Endpoints

| Method | Path | Description |
|--------|------|-------------|
| `POST` | `/v1/chat/completions` | Main data retrieval endpoint (OpenAI compatible) |
| `GET` | `/v1/models` | List available models |
| `GET` | `/health` | Service health check |

### Documentation

Full API documentation is available at [docs.gugou.ai](https://docs.gugou.ai).

---

<a id="中文"></a>

## 中文

OpenX API 将 X/Twitter 的海量实时数据转化为结构化输出，完全兼容 OpenAI 接口协议。搜索推文、发现用户、获取粉丝关系图谱 —— 全部通过标准的 `/v1/chat/completions` 接口完成，你现有的 AI 技术栈无需任何改动。

**无需新 SDK，零学习成本，只需替换 Base URL。**

### 为什么选择 OpenX

| | |
|---|---|
| **OpenAI 协议兼容** | 即插即用 —— 支持所有 OpenAI SDK、LangChain 及任意 HTTP 客户端 |
| **实时数据** | 直连 X/Twitter 实时数据，非缓存或过期数据集 |
| **结构化输出** | 干净的 JSON 格式，包含完整推文元数据、用户资料、媒体链接 |
| **流式传输** | 支持 Server-Sent Events (SSE) 逐步返回结果 |
| **生产可用** | 高可用基础设施，500+ 账号池自动轮转 |

### 可用模型

| 模型 | 功能说明 | 关键参数 |
|------|---------|---------|
| `x-search-tweets` | 按关键词或查询语句搜索推文 | `query`、`limit`、`product`（Top/Latest/Photos/Videos） |
| `x-search-users` | 按关键词搜索 X 用户 | `query`、`limit` |
| `x-user-tweets` | 获取指定用户发布的推文 | `user_id`、`limit` |
| `x-user-followers` | 获取用户的粉丝列表 | `user_id`、`limit` |
| `x-user-following` | 获取用户的关注列表 | `user_id`、`limit` |

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

### 应用场景

- **AI Agent** — 赋予智能体实时感知社交媒体动态的能力
- **RAG 系统** — 用实时 X 数据增强检索管线
- **加密货币 / 金融** — 监控 KOL 观点、代币提及和市场情绪
- **品牌监控** — 追踪品牌提及、竞品动态和用户反馈
- **学术研究** — 采集结构化社交数据用于 NLP 和数据分析
- **KOL 分析** — 绘制粉丝关系图谱，发现关键意见领袖

### API 接口

| 方法 | 路径 | 说明 |
|------|------|------|
| `POST` | `/v1/chat/completions` | 主数据检索接口（OpenAI 兼容） |
| `GET` | `/v1/models` | 获取可用模型列表 |
| `GET` | `/health` | 服务健康检查 |

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
