# ⚡ Dakera AI

**Give your AI agents persistent memory.** Dakera is the complete agent-native data stack — built in Rust — providing vector search, hybrid retrieval, knowledge graphs, session management, and built-in embeddings in a single self-hosted binary. No external services. Your data stays on your stack.

ذاكرة — Dhākira — Arabic for memory

> Stop managing five services. Deploy one binary.

[![Server](https://img.shields.io/badge/dakera-v0.9.14-blue)](https://github.com/dakera-ai/dakera-docs)
[![MCP](https://img.shields.io/badge/dakera--mcp-v0.9.3-purple)](https://github.com/dakera-ai/dakera-mcp)
[![SDKs](https://img.shields.io/badge/SDKs-v0.9.13-green)](https://github.com/dakera-ai/dakera-py)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://github.com/dakera-ai/dakera-py/blob/main/LICENSE)

---

## What Dakera Does

One binary replaces the separate services your AI agents currently depend on:

| What you'd run separately | What Dakera provides |
|--------------------------|---------------------|
| Vector database (Qdrant, Pinecone, Weaviate) | Built-in HNSW / IVF / SPFresh vector index |
| Full-text search (Elasticsearch, OpenSearch) | BM25 full-text search engine |
| Embedding service (OpenAI, Cohere) | On-device ONNX embeddings — no API calls |
| Memory layer (custom Redis/Postgres) | Decay-weighted agent memory with sessions |
| Knowledge graph (Neo4j) | Built-in entity graph with cross-agent network |

**Key capabilities:**
- **Vector search** — HNSW, IVF, and SPFresh indexes; sub-10ms recall at 1M vectors
- **Hybrid retrieval** — vector + BM25 full-text combined scoring in a single call
- **Knowledge graphs** — entity extraction, relationship storage, cross-agent network visualization
- **Session management** — group memories by agent session with session summaries
- **Decay engine** — access-weighted importance scoring; memories fade when not recalled
- **AutoPilot** — automated memory lifecycle management
- **MCP native** — 83 tools, works with Claude Desktop, Claude Code, Cursor out of the box
- **Self-hosted** — PostgreSQL + pgvector; your data never leaves your infrastructure

---

## Quick Start

```bash
# Docker (server)
docker run -p 3000:3000 ghcr.io/dakera-ai/dakera:0.9.14

# Or with Docker Compose (recommended)
curl -O https://raw.githubusercontent.com/dakera-ai/dakera-deploy/main/docker-compose.yml
docker compose up -d
```

```python
# Python SDK
pip install dakera

from dakera import DakeraClient
client = DakeraClient(base_url="http://localhost:3000", api_key="your-key")

# Store a memory
await client.store(agent_id="my-agent", content="User prefers TypeScript", importance=0.8)

# Recall relevant memories
memories = await client.recall(agent_id="my-agent", query="coding preferences")
```

```typescript
// TypeScript SDK
npm install @dakera-ai/dakera

import { DakeraClient } from '@dakera-ai/dakera';
const client = new DakeraClient({ baseUrl: 'http://localhost:3000', apiKey: 'your-key' });

const memories = await client.batchRecall([
  { agentId: 'agent-1', tags: ['important'], minImportance: 0.7 },
  { agentId: 'agent-2', tags: ['context'] },
]);
```

---

## Open Source Packages

| Package | Latest | Install |
|---------|--------|---------|
| [dakera-py](https://github.com/dakera-ai/dakera-py) | v0.9.13 | `pip install dakera` |
| [@dakera-ai/dakera](https://github.com/dakera-ai/dakera-js) | v0.9.13 | `npm install @dakera-ai/dakera` |
| [dakera-rs](https://github.com/dakera-ai/dakera-rs) | v0.9.13 | `cargo add dakera-client` |
| [dakera-go](https://github.com/dakera-ai/dakera-go) | v0.9.13 | `go get github.com/dakera-ai/dakera-go` |
| [dakera-cli](https://github.com/dakera-ai/dakera-cli) | v0.9.13 | `npm install -g @dakera-ai/cli` |
| [dakera-mcp](https://github.com/dakera-ai/dakera-mcp) | v0.9.3 | bundled with server |
| [dakera-docs](https://github.com/dakera-ai/dakera-docs) | — | documentation |

---

## MCP Integration (83 Tools)

Dakera ships an MCP server with 83 tools across 8 categories. Works with Claude Desktop, Claude Code, Cursor, and any MCP-compatible host.

**Tool categories:**
- Memory CRUD — store, recall, batch recall, forget, update, importance tuning
- Vector operations — upsert, query, hybrid search, multi-search, bulk operations
- Knowledge graph — entity storage, relationship queries, cross-agent network
- Session management — start/end sessions, session summaries, session memories
- Namespace operations — create, configure, delete, list namespaces
- Decay Engine — config, stats, decay scheduling
- AutoPilot — trigger, status, automated lifecycle
- Full-text index — index, search, stats, delete

```json
{
  "mcpServers": {
    "dakera": {
      "command": "dakera-mcp",
      "env": { "DAKERA_API_URL": "http://localhost:3000", "DAKERA_API_KEY": "your-key" }
    }
  }
}
```

→ Full reference: [MCP Reference](https://github.com/dakera-ai/dakera-docs/blob/main/docs/mcp.md)

---

## Open Core Model

The engine and dashboard are proprietary. **Everything you need to build with Dakera is open.** Everything that makes Dakera fast is ours.

| Repo | License | What it is |
|------|---------|-----------|
| [dakera-py](https://github.com/dakera-ai/dakera-py) | MIT | Python SDK |
| [dakera-js](https://github.com/dakera-ai/dakera-js) | MIT | TypeScript SDK |
| [dakera-go](https://github.com/dakera-ai/dakera-go) | MIT | Go SDK |
| [dakera-rs](https://github.com/dakera-ai/dakera-rs) | MIT | Rust client |
| [dakera-cli](https://github.com/dakera-ai/dakera-cli) | MIT | CLI (`dk`) |
| [dakera-mcp](https://github.com/dakera-ai/dakera-mcp) | MIT | MCP server |
| [dakera-docs](https://github.com/dakera-ai/dakera-docs) | MIT | Documentation |
| dakera | Proprietary | Core server engine |
| dakera-dashboard | Proprietary | Web dashboard |

---

## Documentation

→ [**Getting Started**](https://github.com/dakera-ai/dakera-docs/blob/main/docs/getting-started.md) — Docker quick start, MCP setup, first SDK call in 10 minutes
→ [**API Reference**](https://github.com/dakera-ai/dakera-docs/blob/main/API.md) — Complete REST + gRPC reference
→ [**MCP Reference**](https://github.com/dakera-ai/dakera-docs/blob/main/docs/mcp.md) — 83 tools reference
→ [**Deployment**](https://github.com/dakera-ai/dakera-docs/blob/main/DEPLOYMENT.md) — Docker, Kubernetes, AWS, GCP, Azure
→ [**Architecture**](https://github.com/dakera-ai/dakera-docs/blob/main/ARCHITECTURE.md) — IVF indexing, BM25, SIMD, storage layer
→ [**Benchmarks**](https://github.com/dakera-ai/dakera-docs/blob/main/BENCHMARKS.md) — HNSW, IVF, SPFresh throughput and latency

---

→ [dakera.ai](https://dakera.ai) · Early access open
→ Built in Rust 🦀 · Single Binary · Sub-10ms Recall · Open Core · MIT SDKs
