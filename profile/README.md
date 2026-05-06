<div align="center">
  <img src="https://raw.githubusercontent.com/dakera-ai/website/main/assets/logo-128.png" width="80" height="80" alt="Dakera AI" />
  <br /><br />

  <h1>DAKERA AI</h1>

  <p><strong>The memory engine for AI agents.</strong><br/>
  Persistent, searchable, decay-weighted agent memory — built in Rust — as a single self-hosted binary.</p>

  <p>
    <a href="https://github.com/dakera-ai/dakera-docs"><img src="https://img.shields.io/badge/server-v0.11.52-blue?style=flat-square" alt="Server v0.11.52" /></a>
    <a href="https://github.com/dakera-ai/dakera-mcp"><img src="https://img.shields.io/badge/mcp-v0.9.7-purple?style=flat-square&label=dakera--mcp" alt="MCP v0.9.7" /></a>
    <a href="https://github.com/dakera-ai/dakera-py"><img src="https://img.shields.io/badge/SDKs-v0.11.51-green?style=flat-square" alt="SDKs v0.11.51" /></a>
    <a href="https://github.com/dakera-ai/dakera-py/blob/main/LICENSE"><img src="https://img.shields.io/badge/license-MIT-blue?style=flat-square" alt="MIT License" /></a>
  </p>

  <p>
    <a href="https://dakera.ai">dakera.ai</a> ·
    <a href="https://dakera.ai/docs">Documentation</a> ·
    <a href="https://dakera.ai#cta">Early Access →</a>
  </p>

  <blockquote>ذاكرة — <em>Dhākira</em> — Arabic for memory</blockquote>
</div>

---

## Why Dakera

Every AI agent session starts from zero. Thousands of interactions — zero retained knowledge. Dakera gives your agents persistent, compounding memory backed by production-grade vector search, hybrid retrieval, and knowledge graphs.

One binary. No external services. Sub-10ms recall.

```bash
# Run with Docker
docker run -d -p 3300:3300 ghcr.io/dakera-ai/dakera:latest

# Verify
curl http://localhost:3300/health
# {"service":"dakera","status":"healthy","version":"0.11.52"}
```

---

## What's Inside

One binary replaces five separate services your agents depend on:

| Instead of | Dakera provides |
|---|---|
| Qdrant / Pinecone / Weaviate | Built-in HNSW · IVF · SPFresh vector index |
| Elasticsearch / OpenSearch | BM25 full-text search engine |
| OpenAI Embeddings | On-device ONNX embeddings — zero API calls |
| Redis / Postgres memory layer | Decay-weighted agent memory with sessions |
| Neo4j knowledge graph | Built-in entity graph with cross-agent network |

**Core capabilities:**

- **Hybrid retrieval** — vector + BM25 combined scoring in a single call
- **Decay engine** — access-weighted importance; memories fade naturally when not recalled
- **Knowledge graphs** — entity extraction, relationship storage, cross-agent network
- **Session management** — group memories by session, generate session summaries
- **MCP native** — 84 tools for Claude Desktop, Claude Code, Cursor, Windsurf
- **Self-hosted** — your data never leaves your infrastructure

---

## Quick Start — Python

```python
pip install dakera
```

```python
from dakera import DakeraClient

client = DakeraClient(base_url="http://localhost:3300", api_key="your-key")

# Store a memory
client.memories.store(
    agent_id="my-agent",
    content="User prefers concise responses",
    importance=0.8,
    tags=["preference"]
)

# Recall relevant memories
memories = client.memories.recall(
    agent_id="my-agent",
    query="how does the user like responses?"
)
```

## Quick Start — TypeScript

```bash
npm install dakera
```

```typescript
import { DakeraClient } from 'dakera';

const client = new DakeraClient({ baseUrl: 'http://localhost:3300', apiKey: 'your-key' });

await client.memories.store({
  agentId: 'my-agent',
  content: 'User prefers concise responses',
  importance: 0.8,
  tags: ['preference'],
});

const memories = await client.memories.recall({
  agentId: 'my-agent',
  query: 'how does the user like responses?',
});
```

---

## MCP — 84 Tools for AI Assistants

Add Dakera to your AI assistant in 30 seconds:

**Claude Desktop** (`~/Library/Application Support/Claude/claude_desktop_config.json`):

```json
{
  "mcpServers": {
    "dakera": {
      "command": "dakera-mcp",
      "env": {
        "DAKERA_API_URL": "http://localhost:3300",
        "DAKERA_API_KEY": "your-api-key"
      }
    }
  }
}
```

**Claude Code** (`.claude/settings.json` in your project):

```json
{
  "mcpServers": {
    "dakera": {
      "command": "dakera-mcp",
      "env": { "DAKERA_API_URL": "http://localhost:3300", "DAKERA_API_KEY": "your-api-key" }
    }
  }
}
```

Tool categories: Memory CRUD · Vector Operations · Knowledge Graph · Sessions · Namespaces · Decay Engine · AutoPilot · Full-text Index

→ [Full MCP reference](https://github.com/dakera-ai/dakera-docs/blob/main/docs/mcp.md)

---

## Open Source Packages

| Package | Version | Install |
|---|---|---|
| [dakera-py](https://github.com/dakera-ai/dakera-py) | `v0.11.51` | `pip install dakera` |
| [dakera-js](https://github.com/dakera-ai/dakera-js) | `v0.11.51` | `npm install dakera` |
| [dakera-rs](https://github.com/dakera-ai/dakera-rs) | `v0.11.51` | `cargo add dakera-client` |
| [dakera-go](https://github.com/dakera-ai/dakera-go) | `v0.11.51` | `go get github.com/dakera-ai/dakera-go` |
| [dakera-cli](https://github.com/dakera-ai/dakera-cli) | `v0.5.5` | `npm install -g @dakera-ai/cli` |
| [dakera-mcp](https://github.com/dakera-ai/dakera-mcp) | `v0.9.7` | bundled with server |
| [dakera-docs](https://github.com/dakera-ai/dakera-docs) | — | documentation |

All SDKs are MIT licensed. The core server engine and dashboard are proprietary.

---

## Documentation

| Guide | What it covers |
|---|---|
| [Getting Started](https://github.com/dakera-ai/dakera-docs/blob/main/docs/getting-started.md) | Docker setup, CLI init, first memory in 10 minutes |
| [MCP Reference](https://github.com/dakera-ai/dakera-docs/blob/main/docs/mcp.md) | 84 tools, client setup, tool categories |
| [API Reference](https://github.com/dakera-ai/dakera-docs/blob/main/API.md) | Complete REST API reference |
| [Configuration](https://github.com/dakera-ai/dakera-docs/blob/main/CONFIGURATION.md) | All environment variables and tuning |
| [Deployment](https://github.com/dakera-ai/dakera-docs/blob/main/DEPLOYMENT.md) | Docker, Kubernetes, AWS, GCP, Azure |
| [Architecture](https://github.com/dakera-ai/dakera-docs/blob/main/ARCHITECTURE.md) | Storage layer, HNSW, BM25, decay engine |
| [Python SDK](https://github.com/dakera-ai/dakera-docs/blob/main/docs/sdk-python.md) | Full Python SDK reference |
| [TypeScript SDK](https://github.com/dakera-ai/dakera-docs/blob/main/docs/sdk-typescript.md) | Full TypeScript SDK reference |

---

<div align="center">
  <a href="https://dakera.ai">dakera.ai</a> · Early access open · Built in Rust 🦀
</div>
