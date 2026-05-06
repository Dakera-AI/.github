<div align="center">
  <img src="https://raw.githubusercontent.com/Dakera-AI/.github/main/assets/logo.png" width="80" height="80" alt="Dakera AI" />
  <br /><br />

  <h1>DAKERA AI</h1>

  <p><strong>The memory engine for AI agents.</strong><br/>
  Persistent, searchable, decay-weighted agent memory — built in Rust — as a single self-hosted binary.</p>

  <p>
    <a href="https://dakera.ai/docs"><img src="https://img.shields.io/badge/server-v0.11.52-blue?style=flat-square" alt="Server v0.11.52" /></a>
    <a href="https://dakera.ai/docs#mcp"><img src="https://img.shields.io/badge/dakera--mcp-v0.9.7-8b5cf6?style=flat-square" alt="MCP v0.9.7" /></a>
    <a href="https://pypi.org/project/dakera/"><img src="https://img.shields.io/badge/SDKs-v0.11.51-22c55e?style=flat-square" alt="SDKs v0.11.51" /></a>
    <a href="https://github.com/dakera-ai/dakera-py/blob/main/LICENSE"><img src="https://img.shields.io/badge/license-MIT-blue?style=flat-square" alt="MIT License" /></a>
  </p>

  <p>
    <a href="https://dakera.ai"><strong>dakera.ai</strong></a>
    &nbsp;·&nbsp;
    <a href="https://dakera.ai/docs">Documentation</a>
    &nbsp;·&nbsp;
    <a href="https://dakera.ai#cta"><strong>Join Early Access →</strong></a>
  </p>

  <p><em>ذاكرة — Dhākira — Arabic for memory</em></p>
</div>

---

## Why Dakera?

Every AI agent session starts from zero. Thousands of interactions — zero retained knowledge. You're paying to re-teach your agents the same things, every time.

Dakera gives your agents **persistent, compounding memory** backed by production-grade vector search, hybrid retrieval, knowledge graphs, and built-in embeddings — in one self-hosted Rust binary.

> Stop managing five services. Deploy one binary.

---

## What's Inside

| Instead of running separately | Dakera provides |
|---|---|
| Qdrant / Pinecone / Weaviate | HNSW · IVF · SPFresh vector index |
| Elasticsearch / OpenSearch | BM25 full-text search engine |
| OpenAI / Cohere embeddings | On-device ONNX — zero API calls |
| Redis / Postgres memory layer | Decay-weighted agent memory with sessions |
| Neo4j | Built-in knowledge graph with cross-agent network |

---

## Quick Start

```bash
# Pull and run
docker run -d -p 3300:3300 -e DAKERA_API_KEY=my-key ghcr.io/dakera-ai/dakera:latest

# Verify
curl http://localhost:3300/health
```

**Python:**
```python
pip install dakera
```
```python
from dakera import DakeraClient

client = DakeraClient(base_url="http://localhost:3300", api_key="my-key")

# Store a memory
client.memories.store(
    agent_id="my-agent",
    content="User prefers TypeScript over Python",
    importance=0.8,
    tags=["preference"]
)

# Recall relevant memories
memories = client.memories.recall(agent_id="my-agent", query="language preferences")
```

**TypeScript:**
```bash
npm install dakera
```
```typescript
import { DakeraClient } from 'dakera';

const client = new DakeraClient({ baseUrl: 'http://localhost:3300', apiKey: 'my-key' });

await client.memories.store({
  agentId: 'my-agent',
  content: 'User prefers TypeScript over Python',
  importance: 0.8,
});

const memories = await client.memories.recall({
  agentId: 'my-agent',
  query: 'language preferences',
});
```

---

## MCP — 84 Tools for AI Assistants

Add Dakera to Claude, Cursor, or Windsurf in 30 seconds:

**Claude Desktop** — `~/Library/Application Support/Claude/claude_desktop_config.json`:
```json
{
  "mcpServers": {
    "dakera": {
      "command": "dakera-mcp",
      "env": { "DAKERA_API_URL": "http://localhost:3300", "DAKERA_API_KEY": "your-key" }
    }
  }
}
```

**Claude Code** — `.claude/settings.json` in your project:
```json
{
  "mcpServers": {
    "dakera": {
      "command": "dakera-mcp",
      "env": { "DAKERA_API_URL": "http://localhost:3300", "DAKERA_API_KEY": "your-key" }
    }
  }
}
```

84 tools across: Memory CRUD · Vector Operations · Knowledge Graph · Sessions · Namespaces · Decay Engine · AutoPilot · Full-text Index

→ [Full MCP documentation](https://dakera.ai/docs#mcp)

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

All SDKs are MIT licensed. The core engine and dashboard are proprietary.

---

## Documentation

→ [**dakera.ai/docs**](https://dakera.ai/docs) — Getting started, MCP setup, SDK references, configuration, deployment, architecture

---

<div align="center">
  <a href="https://dakera.ai">dakera.ai</a> &nbsp;·&nbsp; Early access open &nbsp;·&nbsp; Built in Rust 🦀 &nbsp;·&nbsp; <a href="https://dakera.ai#cta">Join the waitlist →</a>
</div>
