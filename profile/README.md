<h1 align="center">
  <br>
  Dakera AI
  <br>
</h1>

<h3 align="center">The Memory Engine for AI Agents</h3>

<p align="center">
  <em>Persistent, searchable, semantic memory built in Rust.</em><br>
  <em>Store, recall, and consolidate agent knowledge. One binary. Zero dependencies.</em>
</p>

<p align="center">
  <a href="https://dakera.ai">Website</a> &nbsp;&bull;&nbsp;
  <a href="https://www.linkedin.com/company/dakera-ai">LinkedIn</a> &nbsp;&bull;&nbsp;
  <a href="https://github.com/Dakera-AI">GitHub</a>
</p>

---

## The Problem

Every AI agent session starts from zero. Every mistake repeated. Every preference forgotten. Context windows aren't memory -- they're expensive, fragile, and hit a hard ceiling.

**Dakera gives your agents real memory** -- persistent, searchable, and semantic -- so they learn, adapt, and improve across every session.

---

## What Dakera Does

Dakera is not a generic database. It is a purpose-built **memory and retrieval engine** for AI agent systems. It unifies semantic memory, hybrid search, built-in inference, knowledge graphs, and multi-agent coordination into a single Rust binary.

```mermaid
graph LR
    A["AI Agent"] -->|store| B["Dakera"]
    A -->|recall| B
    A -->|search| B
    A -->|consolidate| B
    B -->|semantic memory| C["Learns over time"]
    B -->|knowledge graph| D["Connects ideas"]
    B -->|hybrid search| E["Finds anything"]
```

---

## Architecture

```mermaid
graph TB
    subgraph Client Layer
        REST["REST API"]
        GRPC["gRPC"]
        MCP["MCP Server<br><sub>45+ tools</sub>"]
        DASH["Dashboard<br><sub>34 pages</sub>"]
        CLI["CLI"]
    end

    subgraph Middleware
        AUTH["Auth & API Keys<br><sub>RBAC scoped access</sub>"]
        RATE["Rate Limiting"]
        AUDIT["Audit Logging"]
        PROM["Prometheus Metrics"]
    end

    subgraph Core Engine
        INF["Inference Engine<br><sub>Built-in embeddings</sub>"]
        VEC["Vector Search<br><sub>IVF &middot; HNSW &middot; IVFPQ</sub>"]
        BM25["Full-Text Search<br><sub>BM25</sub>"]
        MEM["Agent Memory<br><sub>Store &middot; Recall &middot; Consolidate</sub>"]
        KG["Knowledge Graph<br><sub>Similarity &middot; Clustering</sub>"]
        CLUSTER["Clustering & Sync<br><sub>Gossip protocol HA</sub>"]
    end

    subgraph Storage Backends
        MEMORY["In-Memory"]
        FS["Filesystem"]
        S3["S3 / MinIO"]
    end

    REST & GRPC & MCP & DASH & CLI --> AUTH & RATE & AUDIT & PROM
    AUTH & RATE & AUDIT & PROM --> INF & VEC & BM25 & MEM & KG & CLUSTER
    INF & VEC & BM25 & MEM & KG & CLUSTER --> MEMORY & FS & S3
```

---

## Core Capabilities

### Agent Memory

Real memory for AI agents -- not just storage, but a cognitive layer that makes agents smarter over time.

```mermaid
graph LR
    subgraph Memory Types
        EP["Episodic<br><sub>What happened</sub>"]
        SEM["Semantic<br><sub>What it means</sub>"]
        PROC["Procedural<br><sub>How to do it</sub>"]
        WORK["Working<br><sub>Right now</sub>"]
    end

    subgraph Lifecycle
        STORE["Store"] --> RECALL["Recall"]
        RECALL --> CONSOLIDATE["Consolidate"]
        CONSOLIDATE --> FORGET["Forget"]
    end

    EP & SEM & PROC & WORK --> STORE
```

- **Per-agent namespaces** -- Hundreds of agents, each with isolated memory
- **Session lifecycle** -- Context survives across conversations
- **Importance scoring** -- Agents remember what matters, forget what doesn't
- **Memory consolidation** -- Automatically merge and strengthen related memories

### Hybrid Search

Three search modes in one engine with tunable weights:

| Mode | How it Works | Best For |
|:--|:--|:--|
| **Vector Similarity** | Cosine, Euclidean, Dot Product with IVF/HNSW/IVFPQ indexes | Semantic meaning, "find similar" |
| **Full-Text (BM25)** | Classic keyword ranking | Exact terms, names, codes |
| **Hybrid** | Weighted combination of vector + BM25 | Best of both worlds |

All modes support **metadata filtering** with rich operators: `$eq`, `$gt`, `$lt`, `$in`, `$and`, `$or`.

### Built-in Inference

No external embedding service needed. Dakera embeds text automatically on upsert and query:

```
POST /v1/namespaces/docs/upsert-text
{"texts": [{"id": "doc-1", "text": "Vector databases enable semantic search"}]}

POST /v1/namespaces/docs/query-text
{"text": "how do semantic search systems work", "top_k": 5}
```

HuggingFace models built in. Auto-vectorization. Semantic routing. Zero configuration.

### Knowledge Graph

Automatically construct knowledge graphs from stored memories:

```mermaid
graph TD
    A["Memory: User prefers TypeScript"] --- B["Memory: Built React dashboard"]
    B --- C["Memory: Uses Next.js for SSR"]
    A --- C
    C --- D["Memory: Deployed on Vercel"]
    B --- E["Memory: Chose Tailwind for styling"]

    style A fill:#1a1a2e,stroke:#D4A843,color:#f8f8f8
    style B fill:#1a1a2e,stroke:#D4A843,color:#f8f8f8
    style C fill:#1a1a2e,stroke:#D4A843,color:#f8f8f8
    style D fill:#1a1a2e,stroke:#D4A843,color:#f8f8f8
    style E fill:#1a1a2e,stroke:#D4A843,color:#f8f8f8
```

- **Similarity edges** -- Automatically link related memories
- **Cluster summarization** -- Group and summarize knowledge clusters
- **Deduplication** -- Keep memory clean and non-redundant

### MCP Native

45+ tools for any MCP-compatible agent -- Claude, Cursor, Windsurf, and more. Memory, search, knowledge graphs, agent management -- all exposed as MCP tools your AI can call directly.

### Production-Grade

| Capability | Details |
|:--|:--|
| **Auth** | Scoped API keys with RBAC (read / write / admin / super_admin) |
| **APIs** | REST (Axum) + gRPC (Tonic) |
| **Clustering** | Multi-node HA with gossip protocol and automatic rebalancing |
| **Storage** | In-memory, filesystem, or S3/MinIO backends |
| **Observability** | Prometheus metrics, health/readiness/liveness probes, audit logging |
| **Operations** | Rate limiting, backup/restore, configuration via environment variables |

---

## How It Works

```mermaid
sequenceDiagram
    participant Agent as AI Agent
    participant D as Dakera
    participant S as Storage

    Note over Agent,S: 1. Store Memory
    Agent->>D: POST /v1/memory/store
    D->>D: Auto-embed text
    D->>D: Score importance
    D->>S: Persist memory
    D-->>Agent: stored

    Note over Agent,S: 2. Recall by Meaning
    Agent->>D: POST /v1/memory/recall
    D->>D: Embed query
    D->>S: Semantic search
    D-->>Agent: ranked memories

    Note over Agent,S: 3. Build Knowledge
    Agent->>D: POST /v1/knowledge/build
    D->>D: Find connections
    D->>D: Cluster & summarize
    D-->>Agent: knowledge graph
```

---

## SDKs & Interfaces

Native SDKs for the languages you already use, plus REST and gRPC for everything else:

| | Language | Install |
|:--|:--|:--|
| **Python** | Python 3.8+ | `pip install dakera` |
| **TypeScript** | Node.js / Deno / Browser | `npm install dakera` |
| **Go** | Go 1.21+ | `go get github.com/dakera-ai/dakera-go` |
| **Rust** | Rust 1.75+ | `cargo add dakera-client` |
| **REST** | Any | `curl localhost:3000/v1/...` |
| **gRPC** | Any | Port `50051` |
| **MCP** | Claude, Cursor, Windsurf | 45+ tools |
| **CLI** | Cross-platform | `dk` binary |
| **Dashboard** | Browser | Leptos / WASM, 34 pages |

---

## Quick Start

```bash
docker run -p 3000:3000 -p 50051:50051 ghcr.io/dakera-ai/dakera:latest
```

```python
from dakera import DakeraClient

client = DakeraClient("http://localhost:3000")

# Store a memory
client.memory_store(
    agent_id="assistant-1",
    text="User prefers TypeScript for frontend work",
    memory_type="semantic",
    importance=0.9
)

# Recall by meaning
memories = client.memory_recall(
    agent_id="assistant-1",
    query="what languages does the user like?",
    top_k=5
)
# => [{score: 0.97, text: "User prefers TypeScript for frontend work"}]
```

---

## The Ecosystem

Dakera is a complete platform -- not just a database. The ecosystem includes the core engine, native SDKs for four languages, a CLI, an MCP server for AI tool integration, an admin dashboard, deployment configurations, comprehensive documentation, and a multi-agent research demo showcasing real-world usage.

We're actively building in the open and will be progressively releasing components as they reach production readiness. **Star and watch this organization to stay updated.**

---

## Built With

<p>
  <img src="https://img.shields.io/badge/Rust-000000?style=for-the-badge&logo=rust&logoColor=white" alt="Rust">
  <img src="https://img.shields.io/badge/Axum-000000?style=for-the-badge" alt="Axum">
  <img src="https://img.shields.io/badge/Tonic_(gRPC)-000000?style=for-the-badge" alt="Tonic">
  <img src="https://img.shields.io/badge/Leptos-000000?style=for-the-badge" alt="Leptos">
  <img src="https://img.shields.io/badge/WebAssembly-000000?style=for-the-badge&logo=webassembly&logoColor=white" alt="WASM">
  <img src="https://img.shields.io/badge/Docker-000000?style=for-the-badge&logo=docker&logoColor=white" alt="Docker">
  <img src="https://img.shields.io/badge/Prometheus-000000?style=for-the-badge&logo=prometheus&logoColor=white" alt="Prometheus">
</p>

---

## License

All Dakera repositories are released under the [MIT License](https://opensource.org/licenses/MIT).

---

<p align="center">
  <a href="https://dakera.ai">dakera.ai</a> &nbsp;&bull;&nbsp;
  <a href="https://www.linkedin.com/company/dakera-ai">LinkedIn</a> &nbsp;&bull;&nbsp;
  <a href="https://github.com/Dakera-AI">GitHub</a>
</p>
