<div align="center">
  <img src="https://raw.githubusercontent.com/Dakera-AI/.github/main/assets/logo.png" width="80" height="80" alt="Dakera AI" />
  <br /><br />

  <h1>DAKERA AI</h1>

  <p><strong>The memory engine for AI agents.</strong><br/>
  Persistent, searchable, decay-weighted agent memory — built in Rust — as a single self-hosted binary.</p>

  <p>
    <a href="https://github.com/dakera-ai/dakera/releases"><img src="https://img.shields.io/github/v/release/dakera-ai/dakera?label=server&style=flat-square&color=blue" alt="Server version" /></a>
    <a href="https://crates.io/crates/dakera-mcp"><img src="https://img.shields.io/crates/v/dakera-mcp?label=dakera-mcp&style=flat-square&color=8b5cf6" alt="MCP version" /></a>
    <a href="https://pypi.org/project/dakera/"><img src="https://img.shields.io/pypi/v/dakera?label=python-sdk&style=flat-square&color=22c55e" alt="Python SDK version" /></a>
    <img src="https://img.shields.io/badge/built_in-Rust-orange?style=flat-square" alt="Built in Rust" />
    <a href="https://github.com/dakera-ai/dakera-py/blob/main/LICENSE"><img src="https://img.shields.io/badge/license-MIT-blue?style=flat-square" alt="MIT License" /></a>
  </p>

  <p>
    <a href="https://dakera.ai"><strong>dakera.ai</strong></a>
    &nbsp;·&nbsp;
    <a href="https://dakera.ai/docs">Documentation</a>
    &nbsp;·&nbsp;
    <a href="https://dakera.ai#cta"><strong>Request Early Access →</strong></a>
  </p>

  <p><em>ذاكرة — Dhākira — Arabic for memory</em></p>
</div>

---

## Why Dakera?

Every AI agent session starts from zero. Thousands of interactions — zero retained knowledge. You're paying to re-teach your agents the same things, every time.

Dakera gives your agents **persistent, compounding memory** backed by production-grade vector search, hybrid retrieval, knowledge graphs, and built-in ML embeddings — in one self-hosted Rust binary.

> Stop managing five services. Deploy one binary.

---

## How It Works

```
1. Store  →  Your agent writes a memory (content + importance score)
2. Recall →  Query by meaning — hybrid vector + BM25 retrieval returns ranked results
3. Decay  →  Memories auto-decay by access pattern; important ones rise, stale ones fade
```

Everything runs inside the same process — no sidecars, no embedding APIs, no message queues.

---

## Performance

| Metric | Value |
|---|---|
| LoCoMo recall benchmark | **87.6%** |
| p99 query latency | **< 10 ms** |
| Insert throughput | **27.4M / second** |
| Binary size | **~44 MB** |
| External runtime dependencies | **0** |

Benchmarked against the full 1,540-question LoCoMo conversational recall suite.

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

### Core SDKs

| Package | Version | Install |
|---|---|---|
| [dakera-py](https://github.com/dakera-ai/dakera-py) | [![PyPI](https://img.shields.io/pypi/v/dakera?style=flat-square)](https://pypi.org/project/dakera/) | `pip install dakera` |
| [dakera-js](https://github.com/dakera-ai/dakera-js) | [![npm](https://img.shields.io/npm/v/dakera?style=flat-square)](https://www.npmjs.com/package/dakera) | `npm install dakera` |
| [dakera-rs](https://github.com/dakera-ai/dakera-rs) | [![crates.io](https://img.shields.io/crates/v/dakera-client?style=flat-square)](https://crates.io/crates/dakera-client) | `cargo add dakera-client` |
| [dakera-go](https://github.com/dakera-ai/dakera-go) | [![GitHub release](https://img.shields.io/github/v/release/dakera-ai/dakera-go?style=flat-square)](https://github.com/dakera-ai/dakera-go/releases) | `go get github.com/dakera-ai/dakera-go` |
| [dakera-cli](https://github.com/dakera-ai/dakera-cli) | [![crates.io](https://img.shields.io/crates/v/dakera-cli?style=flat-square)](https://crates.io/crates/dakera-cli) | `cargo install dakera-cli` |
| [dakera-mcp](https://github.com/dakera-ai/dakera-mcp) | [![crates.io](https://img.shields.io/crates/v/dakera-mcp?style=flat-square)](https://crates.io/crates/dakera-mcp) | bundled with server |

### Framework Integrations

| Package | Version | Install |
|---|---|---|
| [dakera-langchain](https://github.com/dakera-ai/dakera-langchain) | [![PyPI](https://img.shields.io/pypi/v/langchain-dakera?style=flat-square)](https://pypi.org/project/langchain-dakera/) | `pip install langchain-dakera` |
| [dakera-llamaindex](https://github.com/dakera-ai/dakera-llamaindex) | [![PyPI](https://img.shields.io/pypi/v/llamaindex-dakera?style=flat-square)](https://pypi.org/project/llamaindex-dakera/) | `pip install llamaindex-dakera` |
| [dakera-crewai](https://github.com/dakera-ai/dakera-crewai) | [![PyPI](https://img.shields.io/pypi/v/crewai-dakera?style=flat-square)](https://pypi.org/project/crewai-dakera/) | `pip install crewai-dakera` |
| [dakera-autogen](https://github.com/dakera-ai/dakera-autogen) | [![PyPI](https://img.shields.io/pypi/v/autogen-dakera?style=flat-square)](https://pypi.org/project/autogen-dakera/) | `pip install autogen-dakera` |
| [dakera-langchain-js](https://github.com/dakera-ai/dakera-langchain-js) | [![npm](https://img.shields.io/npm/v/langchain-dakera?style=flat-square)](https://www.npmjs.com/package/langchain-dakera) | `npm install langchain-dakera` |

All SDKs and integrations are MIT licensed. The core engine and dashboard are proprietary.

---

## Deployment

```bash
# Docker (quickest)
docker run -d -p 3300:3300 -p 3500:3500 \
  -e DAKERA_API_KEY=my-key \
  ghcr.io/dakera-ai/dakera:latest

# Helm
helm install dakera oci://ghcr.io/dakera-ai/dakera-helm/dakera \
  --namespace dakera --create-namespace \
  --set dakera.rootApiKey=my-key \
  --set minio.rootPassword=my-password
```

→ [Full deployment docs](https://dakera.ai/docs)

---

## Documentation

→ [**dakera.ai/docs**](https://dakera.ai/docs) — Getting started, MCP setup, SDK references, configuration, deployment, architecture

---

<div align="center">
  <a href="https://dakera.ai">dakera.ai</a> &nbsp;·&nbsp; Built in Rust 🦀 &nbsp;·&nbsp; <a href="https://dakera.ai#cta">Request early access →</a>
</div>
