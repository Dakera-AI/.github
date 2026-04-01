<p align="center">
  <img src="https://raw.githubusercontent.com/Dakera-AI/.github/main/assets/logo.png" alt="Dakera AI" width="96">
</p>

<h2 align="center">The Memory Engine for AI Agents</h2>

<p align="center">
  Semantic memory. Hybrid search. Knowledge graphs.<br>
  One Rust binary — no external dependencies.
</p>

<p align="center">
  <a href="https://dakera.ai"><strong>dakera.ai</strong></a> &nbsp;&middot;&nbsp;
  <a href="https://www.linkedin.com/company/dakera-ai">LinkedIn</a>
</p>

<p align="center">
  <a href="https://pypi.org/project/dakera/"><img src="https://img.shields.io/pypi/v/dakera?label=python&color=D4A843&style=flat-square" alt="PyPI"></a>
  <a href="https://www.npmjs.com/package/@dakera-ai/dakera"><img src="https://img.shields.io/npm/v/@dakera-ai/dakera?label=npm&color=D4A843&style=flat-square" alt="npm"></a>
  <a href="https://crates.io/crates/dakera-client"><img src="https://img.shields.io/crates/v/dakera-client?label=rust&color=D4A843&style=flat-square" alt="crates.io"></a>
  <a href="https://github.com/dakera-ai/dakera-go/releases/latest"><img src="https://img.shields.io/github/v/release/dakera-ai/dakera-go?label=go&color=D4A843&style=flat-square" alt="Go"></a>
  <a href="https://crates.io/crates/dakera-mcp"><img src="https://img.shields.io/crates/v/dakera-mcp?label=mcp&color=D4A843&style=flat-square" alt="MCP"></a>
</p>

<br>

> **Your agents have amnesia.** Every session starts from zero. Every mistake repeated. Every preference forgotten. Context windows are not memory — they are expensive, fragile, and hit a hard ceiling. Dakera gives AI agents real memory so they learn, adapt, and improve across every session.

<br>

## Overview

Dakera is **memory infrastructure** for AI agents — not a library, not a wrapper. A self-contained engine that unifies semantic memory, hybrid search, built-in inference, knowledge graphs, and multi-agent coordination into a single Rust binary. No embedding service. No separate graph database. No external search engine. Everything ships together.

```mermaid
%%{init: {'theme': 'dark', 'themeVariables': { 'fontSize': '14px', 'primaryColor': '#1a1a2e', 'primaryTextColor': '#f0cc70', 'primaryBorderColor': '#D4A843', 'lineColor': '#D4A843', 'secondaryColor': '#161616', 'tertiaryColor': '#0d0d0d', 'edgeLabelBackground': '#0d0d0d' }}}%%
flowchart LR
    subgraph Agents
        A1(["Agent 1"])
        A2(["Agent 2"])
        A3(["Agent N"])
    end

    subgraph Dakera
        direction TB
        MEM["Memory Engine"]
        SEARCH["Hybrid Search"]
        INF["Inference"]
        KG["Knowledge Graph"]
    end

    A1 & A2 & A3 -- "store · recall · search" --> Dakera

    Dakera -- "learns over time" --> OUT(["Smarter Agents"])
```

<br>

## Architecture

Multiple Rust crates. One binary. No external services, no sidecars — deploy anywhere with a single container.

```mermaid
%%{init: {'theme': 'dark', 'themeVariables': { 'fontSize': '13px', 'primaryColor': '#1a1a2e', 'primaryTextColor': '#f8f8f8', 'primaryBorderColor': '#D4A843', 'lineColor': '#3a3a3a', 'secondaryColor': '#161616', 'tertiaryColor': '#0d0d0d' }}}%%
block-beta
    columns 5

    block:client:5
        columns 5
        REST["REST API"]
        GRPC["gRPC"]
        MCP_S["MCP Server"]
        DASHBOARD["Dashboard"]
        CLI_T["CLI"]
    end

    space:5

    block:middleware:5
        columns 4
        AUTH["Auth & RBAC"]
        RATE["Rate Limiting"]
        AUDIT["Audit Logging"]
        METRICS["Metrics"]
    end

    space:5

    block:engine:5
        columns 6
        INFERENCE["Inference"]
        VECTOR["Vector Search"]
        FULLTEXT["Full-Text"]
        MEMORY["Agent Memory"]
        KNOWLEDGE["Knowledge Graph"]
        CLUSTER["Clustering"]
    end

    space:5

    block:storage:5
        columns 3
        MEM_S["In-Memory"]
        FS["Filesystem"]
        S3["S3 / MinIO"]
    end

    client --> middleware
    middleware --> engine
    engine --> storage
```

<br>

## Capabilities

### Agent Memory

A cognitive memory layer modeled after how intelligent systems actually work — four memory types, automatic consolidation, and importance-based decay.

| Memory Type | Purpose |
|:--|:--|
| **Episodic** | What happened — event-based recall |
| **Semantic** | What it means — distilled knowledge |
| **Procedural** | How to do it — learned processes |
| **Working** | Active context — current session state |

```mermaid
%%{init: {'theme': 'dark', 'themeVariables': { 'fontSize': '13px', 'primaryColor': '#1a1a2e', 'primaryTextColor': '#f0cc70', 'primaryBorderColor': '#D4A843', 'lineColor': '#D4A843', 'secondaryColor': '#161616', 'tertiaryColor': '#0d0d0d' }}}%%
flowchart LR
    STORE["Store"] --> RECALL["Recall"]
    RECALL --> CONSOLIDATE["Consolidate"]
    CONSOLIDATE --> FORGET["Forget"]
    FORGET -.->|"strengthens\nwhat remains"| STORE
```

- **Per-agent namespaces** — Isolated memory per agent
- **Session lifecycle** — Context persists across conversations
- **Importance scoring** — Prioritized retention and decay
- **Memory consolidation** — Related memories merge automatically

<br>

### Hybrid Search

Three search modes. One engine. Fully tunable weights — so the right memory surfaces at the right moment.

| Mode | Method | Use Case |
|:--|:--|:--|
| **Vector** | Cosine · Euclidean · Dot Product | Semantic similarity |
| **Full-Text** | BM25 keyword ranking | Exact terms and identifiers |
| **Hybrid** | Weighted vector + BM25 | Combined precision and recall |

Supports multiple index types and rich metadata filtering.

<br>

### Built-in Inference

No embedding service to configure, scale, or pay for. Dakera runs inference natively — text is embedded automatically at ingest and at query time.

<br>

### Knowledge Graph

Dakera continuously discovers and weights relationships between memories — no schema design, no manual edges.

```mermaid
%%{init: {'theme': 'dark', 'themeVariables': { 'fontSize': '12px', 'primaryColor': '#1a1a2e', 'primaryTextColor': '#f0cc70', 'primaryBorderColor': '#D4A843', 'lineColor': '#8B6914', 'secondaryColor': '#161616', 'tertiaryColor': '#0d0d0d', 'edgeLabelBackground': '#0d0d0d' }}}%%
graph TD
    M1(["Prefers TypeScript"]) ---|0.94| M2(["Built React dashboard"])
    M2 ---|0.91| M3(["Uses Next.js"])
    M1 ---|0.87| M3
    M3 ---|0.82| M4(["Deployed on Vercel"])
    M2 ---|0.79| M5(["Chose Tailwind CSS"])
```

- **Similarity edges** — Weighted connections between related memories
- **Cluster summarization** — Group and summarize knowledge regions
- **Deduplication** — Eliminate redundancy automatically

<br>

### MCP Integration

First-class MCP support — 83 tools spanning memory, search, knowledge graphs, and agent management. Compatible with Claude, GPT, and any MCP-capable agent framework.

<br>

### Production Infrastructure

| Area | Details |
|:--|:--|
| **Authentication** | Scoped API keys with role-based access control |
| **APIs** | REST and gRPC |
| **High Availability** | Multi-node clustering with automatic rebalancing |
| **Storage** | In-memory, filesystem, or S3-compatible backends |
| **Observability** | Metrics, health probes, audit logging |
| **Operations** | Rate limiting, backup / restore, environment-based configuration |

<br>

## Request Lifecycle

```mermaid
%%{init: {'theme': 'dark', 'themeVariables': { 'fontSize': '13px', 'primaryColor': '#1a1a2e', 'primaryTextColor': '#f8f8f8', 'primaryBorderColor': '#D4A843', 'lineColor': '#D4A843', 'actorTextColor': '#f0cc70', 'actorBorder': '#D4A843', 'actorBkg': '#1a1a2e', 'activationBorderColor': '#D4A843', 'signalColor': '#D4A843', 'noteBkgColor': '#1a1a2e', 'noteTextColor': '#f0cc70', 'noteBorderColor': '#D4A843' }}}%%
sequenceDiagram
    participant Agent
    participant Dakera
    participant Storage

    rect rgba(212, 168, 67, 0.05)
        Note over Agent,Storage: Store
        Agent->>Dakera: Store memory
        Dakera->>Dakera: Embed & score
        Dakera->>Storage: Persist
        Dakera-->>Agent: Stored
    end

    rect rgba(212, 168, 67, 0.05)
        Note over Agent,Storage: Recall
        Agent->>Dakera: Recall by meaning
        Dakera->>Dakera: Embed query
        Dakera->>Storage: Semantic search
        Dakera-->>Agent: Ranked memories
    end

    rect rgba(212, 168, 67, 0.05)
        Note over Agent,Storage: Connect
        Agent->>Dakera: Build knowledge
        Dakera->>Dakera: Discover edges
        Dakera->>Dakera: Cluster & summarize
        Dakera-->>Agent: Knowledge graph
    end
```

<br>

## Ecosystem

| Repository | Purpose |
|:--|:--|
| [dakera-deploy](https://github.com/dakera-ai/dakera-deploy) | Production deployment — Docker Compose, Helm, Kustomize |
| [dakera-py](https://github.com/dakera-ai/dakera-py) | Python SDK |
| [dakera-js](https://github.com/dakera-ai/dakera-js) | TypeScript / JavaScript SDK |
| [dakera-rs](https://github.com/dakera-ai/dakera-rs) | Rust SDK |
| [dakera-go](https://github.com/dakera-ai/dakera-go) | Go SDK |
| dakera-mcp | MCP Server — 83 tools for MCP-compatible agents |
| dakera-cli | CLI — manage namespaces, memories, and sessions |
| [dakera-docs](https://github.com/dakera-ai/dakera-docs) | Documentation and API reference |

<br>

**Documentation and API reference:** [dakera-docs](https://github.com/dakera-ai/dakera-docs)

<br>

## Built With

<p>
  <img src="https://img.shields.io/badge/Rust-0d0d0d?style=flat-square&logo=rust&logoColor=D4A843" alt="Rust">
  <img src="https://img.shields.io/badge/Docker-0d0d0d?style=flat-square&logo=docker&logoColor=D4A843" alt="Docker">
  <img src="https://img.shields.io/badge/WebAssembly-0d0d0d?style=flat-square&logo=webassembly&logoColor=D4A843" alt="WASM">
  <img src="https://img.shields.io/badge/Prometheus-0d0d0d?style=flat-square&logo=prometheus&logoColor=D4A843" alt="Prometheus">
</p>

---

<p align="center">
  Watch this organization to stay updated on releases.<br>
  <br>
  <a href="https://dakera.ai">dakera.ai</a> &nbsp;&middot;&nbsp;
  <a href="https://www.linkedin.com/company/dakera-ai">LinkedIn</a>
</p>
