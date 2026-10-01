# Support

## Documentation

Start with **[dakera.ai/docs](https://dakera.ai/docs)**: quickstart, concepts, API reference, SDK guides, deployment and troubleshooting, for v0.12.0 (latest). The [v0.11 docs](https://dakera.ai/docs/v0-11/) stay available for deployments that have not upgraded. Upgrading from v0.11.108? Read the [upgrade guide](https://dakera.ai/docs/upgrade-from-v0-11-108) first.

## Questions, bugs and feature requests

Open an issue on the repository you use:

| Repository | For |
|:--|:--|
| [dakera-py](https://github.com/Dakera-AI/dakera-py/issues) | Python SDK |
| [dakera-js](https://github.com/Dakera-AI/dakera-js/issues) | TypeScript / JavaScript SDK |
| [dakera-go](https://github.com/Dakera-AI/dakera-go/issues) | Go SDK |
| [dakera-rs](https://github.com/Dakera-AI/dakera-rs/issues) | Rust SDK |
| [dakera-cli](https://github.com/Dakera-AI/dakera-cli/issues) | `dk` command-line client |
| [dakera-mcp](https://github.com/Dakera-AI/dakera-mcp/issues) | MCP server |
| [dakera-deploy](https://github.com/Dakera-AI/dakera-deploy/issues) | Docker Compose, Kubernetes, clustering, monitoring, **and the Dakera server itself** |
| [dakera-helm](https://github.com/Dakera-AI/dakera-helm/issues) | Helm chart |

Framework integrations (LangChain, LangChain.js, LlamaIndex, CrewAI, AutoGen, Vercel AI SDK, Strands) each take issues in their own repository.

The server's source repository is private: report server bugs on [dakera-deploy](https://github.com/Dakera-AI/dakera-deploy/issues). Every report helps more when it includes:

- the server version (`curl http://localhost:3000/health`) and the client version;
- how you deploy (Docker, Compose, Kubernetes, cluster) and the relevant `DAKERA_*` settings, without secrets;
- steps to reproduce, what you expected and what happened, with logs.

## Security

Never report a vulnerability in a public issue. Follow [SECURITY.md](SECURITY.md).

## Commercial enquiries

For commercial use, partnerships or deployment help, contact us through [dakera.ai](https://dakera.ai) or [LinkedIn](https://linkedin.com/company/dakera-ai).
