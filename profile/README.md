<p align="center">
  <a href="https://dakera.ai">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="/profile/assets/banner-dark.png">
      <source media="(prefers-color-scheme: light)" srcset="/profile/assets/banner-light.png">
      <img alt="Dakera: self-hosted memory for AI agents" src="/profile/assets/banner-light.png" width="100%">
    </picture>
  </a>
</p>

<h3 align="center">Long-term memory for AI agents, on your own hardware.</h3>

<p align="center">
  One self-hosted engine for storing and recalling what your agents learn: hybrid vector, full-text and graph recall,<br>
  embeddings and reranking built in, on CPU, with no LLM call at recall time.
</p>

<p align="center">
  <a href="https://dakera.ai/docs/whats-new"><img alt="Release v0.12.0" src="https://img.shields.io/badge/release-v0.12.0-D4A843?style=flat-square&labelColor=1a1712"></a>
  <a href="https://dakera.ai/docs"><img alt="Documentation" src="https://img.shields.io/badge/docs-dakera.ai%2Fdocs-D4A843?style=flat-square&labelColor=1a1712"></a>
  <a href="https://dakera.ai/benchmark"><img alt="LoCoMo 88.2% Recall@20" src="https://img.shields.io/badge/LoCoMo-88.2%25%20Recall%4020-D4A843?style=flat-square&labelColor=1a1712"></a>
  <a href="https://pypi.org/project/dakera/"><img alt="PyPI" src="https://img.shields.io/pypi/v/dakera?style=flat-square&label=pypi&labelColor=1a1712&color=D4A843"></a>
  <a href="https://www.npmjs.com/package/@dakera-ai/dakera"><img alt="npm" src="https://img.shields.io/npm/v/%40dakera-ai%2Fdakera?style=flat-square&label=npm&labelColor=1a1712&color=D4A843"></a>
  <a href="#licence"><img alt="SDKs MIT" src="https://img.shields.io/badge/SDKs-MIT-D4A843?style=flat-square&labelColor=1a1712"></a>
</p>

<p align="center">
  <a href="https://dakera.ai"><b>Website</b></a> &nbsp;·&nbsp;
  <a href="https://dakera.ai/docs"><b>Docs</b></a> &nbsp;·&nbsp;
  <a href="#quickstart"><b>Quickstart</b></a> &nbsp;·&nbsp;
  <a href="https://dakera.ai/benchmark"><b>Benchmark</b></a> &nbsp;·&nbsp;
  <a href="https://dakera.ai/blog/dakera-v0-12-0-release"><b>What's new in v0.12.0</b></a>
</p>

<br>

## What Dakera is

Dakera is a self-hosted memory engine for AI agents. Agents **store** what they learn and **recall** it later by meaning, keyword, time and relationship, across sessions and across agents.

It ships as one container image with its models inside: embeddings, a cross-encoder reranker and entity extraction run locally on CPU. Recall never calls an LLM, so cost and latency stay predictable, and your data never leaves your infrastructure.

Under the hood: HNSW and IVF vector indexes, BM25 full text, a knowledge graph, sessions, namespaces and decay-weighted importance, served over REST, gRPC and MCP.

<br>

## Why Dakera

<table>
  <tr>
    <td width="50%" valign="top">
      <h4>Multilingual</h4>
      <p><code>bge-m3</code> embeddings, full-text stemming and stop words per language, CJK bigrams, and dates understood in seven query languages, with a per-request <code>lang</code>.</p>
    </td>
    <td width="50%" valign="top">
      <h4>Multimodal</h4>
      <p>File attachments per agent, speech to text that turns audio into memories, and visual search over document pages. Each is opt-in, with one variable.</p>
    </td>
  </tr>
  <tr>
    <td valign="top">
      <h4>Multi-vector records and late interaction</h4>
      <p>Records keep one indexed vector plus named token and patch multivectors. Late interaction (<code>colbert-small</code>) reranks with per-token MaxSim.</p>
    </td>
    <td valign="top">
      <h4>Measured performance</h4>
      <p>Reranked recall <b>11.5&nbsp;s → 6.2&nbsp;s</b>. Container memory after reranking <b>4.98 → 0.65&nbsp;GB</b>. HNSW memory per vector <b>−43&nbsp;%</b> at the same recall@10. Vector search p95 <b>1.17&nbsp;ms</b> at 50k vectors.</p>
    </td>
  </tr>
  <tr>
    <td valign="top">
      <h4>Security hardening</h4>
      <p>API keys on REST and gRPC, authenticated cluster traffic, AES-256-GCM encryption at rest with values bound to their record, and a replicated keyring rotated one namespace at a time.</p>
    </td>
    <td valign="top">
      <h4>One-command rollback</h4>
      <p>v0.11.108 upgrades in place. <code>dakera downgrade</code> converts a stopped deployment's data back, and refuses, changing nothing, when it cannot do so safely.</p>
    </td>
  </tr>
  <tr>
    <td valign="top">
      <h4>Clustering and tiered storage</h4>
      <p>Versioned replication that merges instead of overwriting, a durable outbox, and filesystem, S3 or tiered hot / warm / cold storage that survives S3 outages.</p>
    </td>
    <td valign="top">
      <h4>Built for operators</h4>
      <p>Live and ready health checks while models load, a <code>--check-config</code> dry run before a rollout, Prometheus metrics, alert rules and a Grafana dashboard.</p>
    </td>
  </tr>
</table>

<sub>Performance figures compare v0.12.0 with v0.11.108 on the same hardware: reranked recall at <code>top_k</code> 16 in a 4 CPU / 8 GiB container; HNSW figures on BEIR Quora, 50k 1024-d vectors. Details in the <a href="https://dakera.ai/docs/whats-new">release notes</a>.</sub>

<br>

## Quickstart

**1. Run the server.** A fresh install with authentication on needs a root API key. The image ships its default models, so it starts without a download.

```bash
export DAKERA_API_KEY="dk-$(openssl rand -hex 16)"

docker run -d --name dakera -p 3000:3000 \
  -v dakera-data:/data \
  -v dakera-models:/app/models \
  -e DAKERA_ROOT_API_KEY="$DAKERA_API_KEY" \
  -e DAKERA_STORAGE=filesystem \
  ghcr.io/dakera-ai/dakera:0.12.0

curl -s http://localhost:3000/health/ready   # 200 once the models are loaded
```

**2. Store a memory, then recall it.**

```bash
curl -s http://localhost:3000/v1/memory/store \
  -H "Authorization: Bearer $DAKERA_API_KEY" -H "Content-Type: application/json" \
  -d '{"agent_id": "my-agent", "importance": 0.8,
       "content": "The user prefers concise answers with code examples"}'

curl -s http://localhost:3000/v1/memory/recall \
  -H "Authorization: Bearer $DAKERA_API_KEY" -H "Content-Type: application/json" \
  -d '{"agent_id": "my-agent", "query": "How does the user like answers?", "top_k": 5}'
```

<details>
<summary><b>Python</b> &nbsp;<code>pip install dakera</code></summary>

```python
import os
from dakera import DakeraClient

client = DakeraClient(base_url="http://localhost:3000", api_key=os.environ["DAKERA_API_KEY"])

client.store_memory(
    agent_id="my-agent",
    content="The user prefers concise answers with code examples",
    importance=0.8,
    tags=["preference"],
)

response = client.recall(agent_id="my-agent", query="How does the user like answers?", top_k=5)
for memory in response.memories:
    print(f"{memory.score:.2f}  {memory.content}")
```

</details>

<details>
<summary><b>TypeScript</b> &nbsp;<code>npm install @dakera-ai/dakera</code></summary>

```typescript
import { DakeraClient } from '@dakera-ai/dakera';

const client = new DakeraClient({ baseUrl: 'http://localhost:3000', apiKey: process.env.DAKERA_API_KEY });

await client.storeMemory('my-agent', {
  content: 'The user prefers concise answers with code examples',
  importance: 0.8,
  tags: ['preference'],
});

const { memories } = await client.recall('my-agent', 'How does the user like answers?', { top_k: 5 });
memories.forEach((m) => console.log(m.score.toFixed(2), m.content));
```

</details>

<sub>The snippets use the released SDKs (<code>dakera</code> 0.12.12 on PyPI, <code>@dakera-ai/dakera</code> 0.11.106 on npm), which cover these calls on a v0.12.0 server. Client releases with the v0.12 additions are coming. Pin an image version in production; <code>:latest</code> tracks the newest release.</sub>

<br>

## Benchmark

<table>
  <tr>
    <td colspan="4" align="center"><h3>88.2% Recall@20</h3><sub><b>LoCoMo recall benchmark</b> · LLM-judged retrieval recall</sub></td>
  </tr>
  <tr>
    <td align="center" width="25%"><b>86.9%</b><br><sub>Cat1</sub></td>
    <td align="center" width="25%"><b>85.4%</b><br><sub>Cat2</sub></td>
    <td align="center" width="25%"><b>73.9%</b><br><sub>Cat3</sub></td>
    <td align="center" width="25%"><b>91.0%</b><br><sub>Cat4</sub></td>
  </tr>
</table>

<sub>Recall@20 on the LoCoMo evaluation set — 10 conversations, 1,536 evaluated questions (adversarial category excluded), no LLM in the retrieval path. Dakera v0.11.107.</sub>

Methodology and results: **[dakera.ai/benchmark](https://dakera.ai/benchmark)**

<br>

## Ecosystem

The Dakera server is distributed as a container image, [`ghcr.io/dakera-ai/dakera`](https://github.com/orgs/Dakera-AI/packages/container/package/dakera). Everything else below is open source.

<table>
  <tr><th align="left" width="38%">Repository</th><th align="left">Install</th></tr>
  <tr><td colspan="2"><b>SDKs</b></td></tr>
  <tr><td><a href="https://github.com/Dakera-AI/dakera-py">dakera-py</a> &nbsp;<sub>Python</sub></td><td><code>pip install dakera</code></td></tr>
  <tr><td><a href="https://github.com/Dakera-AI/dakera-js">dakera-js</a> &nbsp;<sub>TypeScript / JavaScript</sub></td><td><code>npm install @dakera-ai/dakera</code></td></tr>
  <tr><td><a href="https://github.com/Dakera-AI/dakera-go">dakera-go</a> &nbsp;<sub>Go</sub></td><td><code>go get github.com/dakera-ai/dakera-go</code></td></tr>
  <tr><td><a href="https://github.com/Dakera-AI/dakera-rs">dakera-rs</a> &nbsp;<sub>Rust</sub></td><td><code>cargo add dakera-client</code></td></tr>
  <tr><td colspan="2"><b>Framework integrations</b></td></tr>
  <tr><td><a href="https://github.com/Dakera-AI/dakera-langchain">dakera-langchain</a> &nbsp;<sub>LangChain</sub></td><td><code>pip install langchain-dakera</code></td></tr>
  <tr><td><a href="https://github.com/Dakera-AI/dakera-langchain-js">dakera-langchain-js</a> &nbsp;<sub>LangChain.js</sub></td><td><code>npm install @dakera-ai/langchain</code></td></tr>
  <tr><td><a href="https://github.com/Dakera-AI/dakera-llamaindex">dakera-llamaindex</a> &nbsp;<sub>LlamaIndex</sub></td><td><code>pip install llamaindex-dakera</code></td></tr>
  <tr><td><a href="https://github.com/Dakera-AI/dakera-crewai">dakera-crewai</a> &nbsp;<sub>CrewAI</sub></td><td><code>pip install crewai-dakera</code></td></tr>
  <tr><td><a href="https://github.com/Dakera-AI/dakera-autogen">dakera-autogen</a> &nbsp;<sub>AutoGen</sub></td><td><code>pip install autogen-dakera</code></td></tr>
  <tr><td><a href="https://github.com/Dakera-AI/dakera-ai-sdk">dakera-ai-sdk</a> &nbsp;<sub>Vercel AI SDK</sub></td><td><code>npm install @dakera-ai/ai-sdk</code></td></tr>
  <tr><td><a href="https://github.com/Dakera-AI/strands-dakera">strands-dakera</a> &nbsp;<sub>Strands Agents</sub></td><td><code>pip install strands-dakera</code></td></tr>
  <tr><td colspan="2"><b>Tools</b></td></tr>
  <tr><td><a href="https://github.com/Dakera-AI/dakera-mcp">dakera-mcp</a> &nbsp;<sub>MCP server</sub></td><td><code>npx @dakera-ai/dakera-mcp</code></td></tr>
  <tr><td><a href="https://github.com/Dakera-AI/dakera-cli">dakera-cli</a> &nbsp;<sub>CLI, <code>dk</code></sub></td><td><code>brew install dakera-ai/tap/dk</code></td></tr>
  <tr><td><a href="https://github.com/Dakera-AI/homebrew-tap">homebrew-tap</a> · <a href="https://github.com/Dakera-AI/apt-repo">apt-repo</a> · <a href="https://github.com/Dakera-AI/rpm-repo">rpm-repo</a></td><td>Homebrew, apt and dnf packages for <code>dk</code></td></tr>
  <tr><td colspan="2"><b>Deployment</b></td></tr>
  <tr><td><a href="https://github.com/Dakera-AI/dakera-deploy">dakera-deploy</a> &nbsp;<sub>Compose and Kubernetes</sub></td><td>Compose profiles, HA cluster, manifests, monitoring</td></tr>
  <tr><td><a href="https://github.com/Dakera-AI/dakera-helm">dakera-helm</a> &nbsp;<sub>Helm chart</sub></td><td>OCI chart on <code>ghcr.io</code>, also on <a href="https://artifacthub.io/packages/helm/dakera/dakera">Artifact Hub</a></td></tr>
</table>

<br>

## Deploy

<table>
  <tr>
    <td width="33%" valign="top">
      <h4>Docker</h4>
      <p>One container with its models inside, as in the <a href="#quickstart">quickstart</a>. Starts air-gapped and on read-only file systems.</p>
    </td>
    <td width="33%" valign="top">
      <h4>Docker Compose</h4>
      <p>Single node with MinIO, a three-node HA cluster behind Traefik, and a Prometheus and Grafana stack: <a href="https://github.com/Dakera-AI/dakera-deploy">dakera-deploy</a>.</p>
    </td>
    <td width="33%" valign="top">
      <h4>Kubernetes</h4>
      <p>The Helm chart from <code>oci://ghcr.io</code> or <a href="https://artifacthub.io/packages/helm/dakera/dakera">Artifact Hub</a>, with probes and a model cache: <a href="https://github.com/Dakera-AI/dakera-helm">dakera-helm</a>.</p>
    </td>
  </tr>
</table>

```bash
# Docker Compose: Dakera with MinIO object storage
git clone https://github.com/Dakera-AI/dakera-deploy && cd dakera-deploy/docker
cp .env.example .env          # set DAKERA_ROOT_API_KEY and the MinIO credentials
docker compose up -d

# Kubernetes: the Helm chart
helm install dakera oci://ghcr.io/dakera-ai/dakera-helm/dakera \
  --namespace dakera --create-namespace \
  --set dakera.rootApiKey="$DAKERA_API_KEY" \
  --set minio.rootPassword="<password>"
```

Configuration, clustering, storage backends and air-gapped installs: [deployment docs](https://dakera.ai/docs/deployment).

<br>

## Docs and learning

<table>
  <tr><td width="34%"><a href="https://dakera.ai/docs"><b>Documentation</b></a></td><td>v0.12.0, the latest release. The <a href="https://dakera.ai/docs/v0-11/">v0.11 docs</a> stay available as an archive.</td></tr>
  <tr><td><a href="https://dakera.ai/blog/dakera-v0-12-0-release"><b>What's new in v0.12.0</b></a></td><td>The release announcement.</td></tr>
  <tr><td><a href="https://dakera.ai/docs/upgrade-from-v0-11-108"><b>Upgrade from v0.11.108</b></a></td><td>What changes on the first start, what to check, and <a href="https://dakera.ai/docs/rollback-to-v0-11">how to go back</a>.</td></tr>
  <tr><td><a href="https://dakera.ai/docs/concepts"><b>Concepts</b></a> · <a href="https://dakera.ai/docs/api"><b>API reference</b></a></td><td>Memories, agents, namespaces and sessions; every route.</td></tr>
  <tr><td><a href="https://dakera.ai/playground"><b>Playground</b></a></td><td>Try store and recall in the browser.</td></tr>
</table>

<br>

## Community and support

- **Questions and bugs:** open an issue on the repository you use, for example [dakera-py](https://github.com/Dakera-AI/dakera-py/issues) or [dakera-deploy](https://github.com/Dakera-AI/dakera-deploy/issues). Include the server version from `/health`. See [SUPPORT.md](https://github.com/Dakera-AI/.github/blob/main/SUPPORT.md).
- **Security:** report vulnerabilities privately, never in a public issue. See [SECURITY.md](https://github.com/Dakera-AI/.github/blob/main/SECURITY.md).
- **Contributing:** the SDKs, integrations, CLI, MCP server and deployment repositories accept pull requests. See [CONTRIBUTING.md](https://github.com/Dakera-AI/.github/blob/main/CONTRIBUTING.md) and the [code of conduct](https://github.com/Dakera-AI/.github/blob/main/CODE_OF_CONDUCT.md).
- **News:** [LinkedIn](https://linkedin.com/company/dakera-ai) and the [blog](https://dakera.ai/blog).

<br>

## Licence

The SDKs, framework integrations, CLI and MCP server are MIT licensed (strands-dakera is Apache-2.0); check each repository. The Dakera server is proprietary and is distributed as a container image with no usage fees.

<sub>The server sends product telemetry, never memory content, unless you turn it off with <code>DAKERA_TELEMETRY=0</code> or <code>DO_NOT_TRACK=1</code>. What it sends is listed in the <a href="https://dakera.ai/docs/telemetry">telemetry docs</a>.</sub>

<br>

<p align="center">
  <a href="https://dakera.ai">dakera.ai</a> &nbsp;·&nbsp; <a href="https://dakera.ai/docs">Docs</a> &nbsp;·&nbsp; <a href="https://dakera.ai/benchmark">Benchmark</a> &nbsp;·&nbsp; <a href="https://linkedin.com/company/dakera-ai">LinkedIn</a>
  <br>
  <sub><i>ذاكرة · dhākira · Arabic for memory</i></sub>
</p>
