# Security Policy

## Reporting a Vulnerability

If you discover a security vulnerability in any Dakera repository, **please do not open a public issue**.

**Use GitHub Private Security Reporting:**
Navigate to the affected repository → **Security** tab → **Report a vulnerability**. This creates a private advisory visible only to maintainers and keeps the disclosure confidential until a fix is ready.

We will acknowledge receipt within 48 hours and aim to provide an initial assessment within 5 business days.

For the core engine, whose repository is private, you can also contact us through our [LinkedIn page](https://linkedin.com/company/dakera-ai) with "Security" in the message, as the engine's security policy describes.

## Supported Versions

Security fixes are applied to the latest released version, as stated in the engine's own security policy. We recommend always running the most recent release.

| Component | Supported | Not supported |
|:--|:--|:--|
| Dakera engine | 0.12.x (current line, 0.12.0 released 2026-10-01) | 0.11.x and earlier |
| SDKs, CLI, MCP server, integrations | The latest release of each repository | Earlier releases |

Deployments on v0.11.108 can upgrade in place, and can go back with `dakera downgrade` if needed; see the upgrade guide in the [documentation](https://dakera.ai/docs). Dakera is in public alpha.

## Data Handling

The self-hosted engine processes all memory data on your own infrastructure. Encryption at rest is available (AES-256-GCM, `DAKERA_ENCRYPTION_KEY`). The engine sends operational telemetry that never includes content, ids, secrets or paths, and that you can turn off with `DAKERA_TELEMETRY=0` or `DO_NOT_TRACK=1`; the full list is in the engine's `docs/telemetry.md`.

## Disclosure Policy

- We follow coordinated disclosure. We will work with you to understand and address the issue before any public disclosure.
- Credit will be given to reporters unless they prefer to remain anonymous.
- We will publish security advisories through GitHub Security Advisories.

## Scope

This policy applies to all repositories under the [Dakera-AI](https://github.com/Dakera-AI) organization.
