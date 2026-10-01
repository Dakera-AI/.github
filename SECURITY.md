# Security policy

## Reporting a vulnerability

Report security issues privately. **Do not open a public issue, pull request or discussion.**

Send a message to the [Dakera LinkedIn page](https://linkedin.com/company/dakera-ai) with "Security" in it, so it reaches the right people. We acknowledge reports within 5 business days.

Include, where you can:

- the affected component and version (server version from `GET /health`, or the SDK, CLI or MCP server version);
- a description of the issue and its impact;
- steps to reproduce, and any logs or proof of concept.

## Supported versions

Security fixes go into the latest release. Run the most recent version.

| Component | Supported | Not supported |
|:--|:--|:--|
| Dakera server | Latest `0.12.x` release | `0.11.x` and earlier: upgrade to `0.12.x` |
| SDKs, integrations, CLI, MCP server, Helm chart | Latest release of each | Earlier releases |

Deployments on v0.11.108 upgrade in place to v0.12.0 and can go back with `dakera downgrade`. See the [upgrade guide](https://dakera.ai/docs/upgrade-from-v0-11-108). Dakera is in public alpha.

## Disclosure

- We follow coordinated disclosure: we work with you on a fix before anything is made public.
- We credit reporters who want to be credited.
- Advisories are published as GitHub Security Advisories on the affected repository.

## Data handling

- The self-hosted server processes memory data on your infrastructure. Memory content is never sent to Dakera.
- Encryption at rest uses AES-256-GCM (`DAKERA_ENCRYPTION_KEY`). Since v0.12.0 each sealed value is bound to its record, and keys live in a replicated keyring that can be rotated one namespace at a time.
- API keys are required by default, on gRPC as well as REST since v0.12.0. Cluster traffic is authenticated with `DAKERA_CLUSTER_SECRET`.
- The server sends product telemetry: never memory content, ids, secrets or paths, but it includes the machine's hostname, and the collector records the connecting IP address. Turn it off with `DAKERA_TELEMETRY=0` or `DO_NOT_TRACK=1`. Details: [telemetry docs](https://dakera.ai/docs/telemetry).

## Scope

This policy covers every repository in the [Dakera-AI](https://github.com/Dakera-AI) organization and the Dakera server image, `ghcr.io/dakera-ai/dakera`.
