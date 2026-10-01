<div align="center">

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/Dakera-AI/.github/main/assets/logo.png">
  <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/Dakera-AI/.github/main/assets/logo.png">
  <img src="https://raw.githubusercontent.com/Dakera-AI/.github/main/assets/logo.png" width="72" height="72" alt="Dakera AI" />
</picture>

<br />

# Dakera AI

### Self-hosted memory for AI agents

Persistent, session-aware memory for agents: hybrid vector and full-text recall, a knowledge graph and decay-weighted importance, served by one self-hosted Rust engine with built-in embeddings. Your data stays on your infrastructure.

<br />

[![Website](https://img.shields.io/badge/dakera.ai-Website-22c55e?style=for-the-badge)](https://dakera.ai)
[![Docs](https://img.shields.io/badge/Documentation-latest-3b82f6?style=for-the-badge)](https://dakera.ai/docs)
[![Docker](https://img.shields.io/badge/ghcr.io-dakera:0.12.0-f59e0b?style=for-the-badge)](https://github.com/orgs/Dakera-AI/packages)

<sub><em>ذاكرة — Dhākira — Arabic for memory</em></sub>

</div>

<br />

## Dakera v0.12.0

Released 2026-10-01. An unchanged v0.11.108 deployment upgrades in place, and going back is one command (`dakera downgrade`). Read the upgrade guide in the [documentation](https://dakera.ai/docs) before upgrading: a few behaviours change (gRPC needs an API key, namespace quotas are enforced, ranking and `smart_score` scale change, a stored backup schedule starts running).

- **Faster, leaner recall.** In a paired run against v0.11.108, recall p50 went from 3.8 s to 1.8 s and resident memory from 2.4 GB to 0.97 GB. Reranked recall went from 11.5 s to 6.2 s, and container memory after reranking from 4.98 GB to 0.65 GB.
- **Recall quality.** 89.5 % on LoCoMo (1,540 questions), up from 86.4 % for v0.11.93 on the same gate metric (definition below).
- **Security.** gRPC requires an API key, cluster traffic is authenticated, encrypted values are bound to their record with a replicated keyring, and keys pinned to namespaces no longer reach node-wide admin routes.
- **Reliability.** Versioned, merging cluster replication; durable tiered storage that survives S3 outages; live and ready health endpoints that answer while models download.
- **Opt-in features.** Multimodal memory (attachments, speech to text, image indexing, records), late interaction, multilingual search (`bge-m3`), RaBitQ search mode and `GET /v1/capabilities`. All are off by default.
- **Smaller image.** `ghcr.io/dakera-ai/dakera:0.12.0` is about 0.8 GB (783 MB measured, amd64) and ships its default models, so it starts without a download.

Full list: the release notes in the [documentation](https://dakera.ai/docs), including the known limitations.

<br />

## Benchmark

| Metric | Value |
|:---|:---|
| **LoCoMo, dakera-bench gate metric** (1,540 questions, v0.12.0) | **89.5 %** (v0.11.93: 86.4 %, same metric) |
| **Per category** | single-hop 90.1 % · multi-hop 88.5 % · temporal 72.9 % · open-domain 91.7 % |
| **Paper-comparable R@K** (single production ranked list) | R@1 48.2 % · R@5 63.4 % · R@10 67.7 % · R@20 84.7 % |

<sub>Definition of the headline: it is the dakera-bench LoCoMo gate metric. A question counts when its gold evidence is retrieved by the production recall (top 10) or by the benchmark's additional deep-probe passes. It is recall only: categories 1-4, production configuration, no LLM judge. It is not an R@K over one ranked list, so do not compare it with published R@K numbers or with the 88.2 % Recall@20 we published for v0.11; use the paper-comparable row for that. Raw data is published under [dakera.ai/benchmark](https://dakera.ai/benchmark) (under `/benchmark/v0.12.0/`). In the paired three-conversation run against v0.11.108, v0.12.0 scored 70.2 % against 73.0 % (recall@10 in the release notes; p = 0.052, within the release gate); the release notes explain why and what is planned for 0.12.1.</sub>

<br />

## Quick start

A fresh install with authentication on needs a root API key. The image ships its default models; the second volume keeps any other model you enable.

```bash
docker run -d --name dakera \
  -p 3000:3000 \
  -v dakera-data:/data \
  -v dakera-models:/app/models \
  -e DAKERA_ROOT_API_KEY=dk-mykey \
  -e DAKERA_STORAGE=filesystem \
  ghcr.io/dakera-ai/dakera:0.12.0

curl http://localhost:3000/health/ready   # 200 once the models are loaded
curl http://localhost:3000/health
```

`:latest` tracks the newest release; pin a version in production. Run `dakera --check-config` against your environment before a rollout. Kubernetes (Helm), Docker Compose, clustering and air-gapped installs are covered in [dakera-deploy](https://github.com/Dakera-AI/dakera-deploy), [dakera-helm](https://github.com/Dakera-AI/dakera-helm) and the docs.

```python
pip install dakera

from dakera import DakeraClient

client = DakeraClient(base_url="http://localhost:3000", api_key="dk-mykey")
client.memories.store(agent_id="my-agent", content="User prefers TypeScript over Python",
                      importance=0.8, tags=["preference"])
memories = client.memories.recall(agent_id="my-agent", query="language preferences")
```

<br />

## What Dakera combines

| Instead of running separately | Dakera provides |
|:---|:---|
| A vector database | HNSW and IVF vector indexes |
| A full-text search service | BM25 full-text search |
| A hosted embeddings API | Local ONNX inference, models in the image |
| A key-value store for agent memory | Decay-weighted memories, sessions and namespaces |
| A graph database | A knowledge graph with entity extraction |

<br />

## Ecosystem

The core engine is proprietary and distributed as a container image. The repositories below are public. SDK, CLI and MCP support for v0.12.0 is in review as draft pull requests and is not released yet; until it is, use the current releases (they work with v0.11.108).

### Server, deployment and tooling

| Repository | What it is | Status |
|:---|:---|:---|
| [dakera-deploy](https://github.com/Dakera-AI/dakera-deploy) | Docker Compose, Kubernetes manifests, clustering and monitoring configs | v0.12 update in review (#299) |
| [dakera-helm](https://github.com/Dakera-AI/dakera-helm) | Helm chart, published to Artifact Hub and as an OCI chart | v0.12 update in review (#12) |
| [dakera-cli](https://github.com/Dakera-AI/dakera-cli) | `dk`, the command-line client | v0.12 support in review (#152) |
| [dakera-mcp](https://github.com/Dakera-AI/dakera-mcp) | MCP server for Claude, Cursor and Windsurf; 14 core tools by default, more through profiles | v0.12 support in review (#155) |
| [homebrew-tap](https://github.com/Dakera-AI/homebrew-tap), [apt-repo](https://github.com/Dakera-AI/apt-repo), [rpm-repo](https://github.com/Dakera-AI/rpm-repo) | Package repositories for `dk` (Homebrew also carries `dakera-mcp`); updated by the CLI and MCP releases | Follow the dakera-cli and dakera-mcp releases |

### SDKs

| Repository | Install | Status |
|:---|:---|:---|
| [dakera-py](https://github.com/Dakera-AI/dakera-py) | `pip install dakera` | v0.12 support in review (#198) |
| [dakera-js](https://github.com/Dakera-AI/dakera-js) | `npm install @dakera-ai/dakera` | v0.12 support in review (#246) |
| [dakera-rs](https://github.com/Dakera-AI/dakera-rs) | `cargo add dakera-client` | v0.12 support in review (#168) |
| [dakera-go](https://github.com/Dakera-AI/dakera-go) | `go get github.com/dakera-ai/dakera-go` | v0.12 support in review (#159) |

### Framework integrations

| Repository | Install | Status |
|:---|:---|:---|
| [dakera-langchain](https://github.com/Dakera-AI/dakera-langchain) | `pip install langchain-dakera` | Released, v0.2.0 |
| [dakera-langchain-js](https://github.com/Dakera-AI/dakera-langchain-js) | `npm install @dakera-ai/langchain` | Released, v0.2.0 |
| [dakera-llamaindex](https://github.com/Dakera-AI/dakera-llamaindex) | `pip install llamaindex-dakera` | Released, v0.2.0 |
| [dakera-crewai](https://github.com/Dakera-AI/dakera-crewai) | `pip install crewai-dakera` | Released, v0.2.0 |
| [dakera-autogen](https://github.com/Dakera-AI/dakera-autogen) | `pip install autogen-dakera` | Released, v0.2.0 |
| [dakera-ai-sdk](https://github.com/Dakera-AI/dakera-ai-sdk) | `npm install @dakera-ai/ai-sdk` | Released, v0.1.2 (Vercel AI SDK) |
| [strands-dakera](https://github.com/Dakera-AI/strands-dakera) | see the repository | Released, python v0.2.0 (Strands Agents) |

<sub>The SDKs and integrations that declare a licence are MIT (Apache-2.0 for strands-dakera); check each repository. Status reflects the state on 2026-10-01 and is not updated automatically.</sub>

<br />

## Documentation

- [Documentation](https://dakera.ai/docs) (latest, v0.12)
- [v0.11 documentation](https://dakera.ai/docs/v0-11/) (kept for deployments still on v0.11; available once the versioned docs are published)
- Upgrading from v0.11.108 and the release notes (in the [documentation](https://dakera.ai/docs))
- [Benchmark](https://dakera.ai/benchmark)
- [Support](SUPPORT.md) · [Security policy](SECURITY.md) · [Contributing](CONTRIBUTING.md)

<sub>The self-hosted engine sends product telemetry unless you turn it off with `DAKERA_TELEMETRY=0` or `DO_NOT_TRACK=1`. It never sends memory content; what it sends is listed in the engine's `docs/telemetry.md`.</sub>

<br />

<div align="center">

<a href="https://dakera.ai">dakera.ai</a> · <a href="https://dakera.ai/docs">Docs</a> · <a href="https://github.com/Dakera-AI">GitHub</a>

</div>
