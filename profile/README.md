<div align="center">

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/Dakera-AI/.github/main/assets/logo.png">
  <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/Dakera-AI/.github/main/assets/logo.png">
  <img src="https://raw.githubusercontent.com/Dakera-AI/.github/main/assets/logo.png" width="72" height="72" alt="Dakera AI" />
</picture>

<br />

# Dakera AI

### The memory engine for AI agents

Persistent · Searchable · Decay-weighted · Self-hosted · Built in Rust

<br />

[![Python SDK](https://img.shields.io/pypi/v/dakera?label=python-sdk&style=flat-square&color=22c55e)](https://pypi.org/project/dakera/)
[![TypeScript SDK](https://img.shields.io/npm/v/@dakera-ai/dakera?label=typescript-sdk&style=flat-square&color=3b82f6)](https://www.npmjs.com/package/@dakera-ai/dakera)
[![LangChain](https://img.shields.io/pypi/v/langchain-dakera?label=langchain&style=flat-square&color=8b5cf6)](https://pypi.org/project/langchain-dakera/)
[![LlamaIndex](https://img.shields.io/pypi/v/llamaindex-dakera?label=llamaindex&style=flat-square&color=8b5cf6)](https://pypi.org/project/llamaindex-dakera/)
[![CrewAI](https://img.shields.io/pypi/v/crewai-dakera?label=crewai&style=flat-square&color=8b5cf6)](https://pypi.org/project/crewai-dakera/)
[![AutoGen](https://img.shields.io/pypi/v/autogen-dakera?label=autogen&style=flat-square&color=8b5cf6)](https://pypi.org/project/autogen-dakera/)
[![Helm](https://img.shields.io/badge/helm-dakera--deploy-f59e0b?style=flat-square)](https://github.com/dakera-ai/dakera-deploy)
[![Built in Rust](https://img.shields.io/badge/built_in-Rust-orange?style=flat-square)](#)
[![MIT License](https://img.shields.io/badge/SDKs-MIT-blue?style=flat-square)](https://github.com/dakera-ai/dakera-py/blob/main/LICENSE)

<br />

[**dakera.ai**](https://dakera.ai) · [Documentation](https://dakera.ai/docs) · [Quickstart](https://dakera.ai/docs/quickstart) · [Request Early Access →](https://dakera.ai#cta)

<sub><em>ذاكرة — Dhākira — Arabic for memory</em></sub>

</div>

<br />

## The Problem

Every AI agent session starts from zero. Thousands of interactions — zero retained knowledge. You're paying to re-teach your agents the same things, every conversation.

**Dakera solves this.** One self-hosted Rust binary gives your agents persistent, compounding memory — backed by hybrid search, knowledge graphs, and built-in ML embeddings.

<br />

## Architecture

<div align="center">
<img src="https://raw.githubusercontent.com/Dakera-AI/.github/main/assets/architecture.svg" alt="Dakera Architecture" width="100%" />
</div>

<br />

## Performance

| Metric | Value |
|:---|:---|
| **LoCoMo recall benchmark** | **87.6%** overall |
| **Category breakdown** | Cat1 87.2% · Cat2 86.3% · Cat3 72.0% · Cat4 90.6% |
| **p99 query latency** | < 10 ms |
| **Insert throughput** | 27.4M / second |
| **Binary size** | ~44 MB |
| **External runtime deps** | **0** |

<sub>Benchmarked on the full 1,540-question LoCoMo conversational memory suite (v0.11.55).</sub>

<br />

## What Dakera Replaces

| Running separately | Dakera provides |
|:---|:---|
| Qdrant / Pinecone / Weaviate | HNSW + IVF vector index |
| Elasticsearch / OpenSearch | BM25 full-text search |
| OpenAI / Cohere embeddings | On-device ONNX inference |
| Redis / Postgres memory | Decay-weighted sessions & namespaces |
| Neo4j | Knowledge graph with entity extraction |

> Stop managing five services. Deploy one binary.

<br />

## Quick Start

```bash
docker run -d -p 3300:3300 -e DAKERA_API_KEY=my-key ghcr.io/dakera-ai/dakera:latest
curl http://localhost:3300/health
```

**Python:**
```python
pip install dakera
```
```python
from dakera import DakeraClient

client = DakeraClient(base_url="http://localhost:3300", api_key="my-key")

client.memories.store(
    agent_id="my-agent",
    content="User prefers TypeScript over Python",
    importance=0.8,
    tags=["preference"]
)

memories = client.memories.recall(agent_id="my-agent", query="language preferences")
```

**TypeScript:**
```bash
npm install @dakera-ai/dakera
```
```typescript
import { DakeraClient } from '@dakera-ai/dakera';

const client = new DakeraClient({ baseUrl: 'http://localhost:3300', apiKey: 'my-key' });

await client.memories.store({
  agentId: 'my-agent',
  content: 'User prefers TypeScript over Python',
  importance: 0.8,
  tags: ['preference'],
});

const memories = await client.memories.recall({ agentId: 'my-agent', query: 'language preferences' });
```

<br />

## MCP — 83 Tools for AI Assistants

Add persistent memory to Claude, Cursor, or Windsurf:

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

83 tools: Memory CRUD · Vector Operations · Knowledge Graph · Sessions · Namespaces · Decay Engine · AutoPilot · Full-text Index

→ [MCP documentation](https://dakera.ai/docs#mcp)

<br />

## Packages

### Core SDKs

| Package | Version | Install |
|:---|:---|:---|
| [dakera-py](https://github.com/dakera-ai/dakera-py) | [![PyPI](https://img.shields.io/pypi/v/dakera?style=flat-square)](https://pypi.org/project/dakera/) | `pip install dakera` |
| [dakera-js](https://github.com/dakera-ai/dakera-js) | [![npm](https://img.shields.io/npm/v/@dakera-ai/dakera?style=flat-square)](https://www.npmjs.com/package/@dakera-ai/dakera) | `npm install @dakera-ai/dakera` |
| [dakera-rs](https://github.com/dakera-ai/dakera-rs) | [![GitHub](https://img.shields.io/github/v/release/dakera-ai/dakera-rs?style=flat-square&label=version)](https://github.com/dakera-ai/dakera-rs/releases) | `cargo add dakera-client` |
| [dakera-go](https://github.com/dakera-ai/dakera-go) | [![GitHub](https://img.shields.io/github/v/release/dakera-ai/dakera-go?style=flat-square&label=version)](https://github.com/dakera-ai/dakera-go/releases) | `go get github.com/dakera-ai/dakera-go` |
| [dakera-cli](https://github.com/dakera-ai/dakera-cli) | [![GitHub](https://img.shields.io/github/v/release/dakera-ai/dakera-cli?style=flat-square&label=version)](https://github.com/dakera-ai/dakera-cli/releases) | `cargo install dakera-cli` |
| [dakera-mcp](https://github.com/dakera-ai/dakera-mcp) | [![GitHub](https://img.shields.io/github/v/release/dakera-ai/dakera-mcp?style=flat-square&label=version)](https://github.com/dakera-ai/dakera-mcp/releases) | bundled with server |

### Framework Integrations

| Package | Version | Install |
|:---|:---|:---|
| [dakera-langchain](https://github.com/dakera-ai/dakera-langchain) | [![PyPI](https://img.shields.io/pypi/v/langchain-dakera?style=flat-square)](https://pypi.org/project/langchain-dakera/) | `pip install langchain-dakera` |
| [dakera-llamaindex](https://github.com/dakera-ai/dakera-llamaindex) | [![PyPI](https://img.shields.io/pypi/v/llamaindex-dakera?style=flat-square)](https://pypi.org/project/llamaindex-dakera/) | `pip install llamaindex-dakera` |
| [dakera-crewai](https://github.com/dakera-ai/dakera-crewai) | [![PyPI](https://img.shields.io/pypi/v/crewai-dakera?style=flat-square)](https://pypi.org/project/crewai-dakera/) | `pip install crewai-dakera` |
| [dakera-autogen](https://github.com/dakera-ai/dakera-autogen) | [![PyPI](https://img.shields.io/pypi/v/autogen-dakera?style=flat-square)](https://pypi.org/project/autogen-dakera/) | `pip install autogen-dakera` |
| [dakera-langchain-js](https://github.com/dakera-ai/dakera-langchain-js) | [![npm](https://img.shields.io/npm/v/langchain-dakera?style=flat-square)](https://www.npmjs.com/package/langchain-dakera) | `npm install langchain-dakera` |

<sub>All SDKs and integrations are MIT licensed. The core engine is proprietary.</sub>

<br />

## Deployment

```bash
# Docker
docker run -d -p 3300:3300 -p 3500:3500 \
  -e DAKERA_API_KEY=my-key \
  ghcr.io/dakera-ai/dakera:latest

# Helm (Kubernetes)
helm install dakera oci://ghcr.io/dakera-ai/dakera-helm/dakera \
  --namespace dakera --create-namespace \
  --set dakera.rootApiKey=my-key
```

→ [Full deployment documentation](https://dakera.ai/docs)

<br />

---

<div align="center">

<a href="https://dakera.ai">dakera.ai</a> · <a href="https://dakera.ai/docs">Docs</a> · <a href="https://github.com/dakera-ai">GitHub</a> · <a href="https://dakera.ai#cta">Request Early Access →</a>

<sub>Built with Rust · Self-hosted · Zero dependencies · 87.6% LoCoMo</sub>

</div>
