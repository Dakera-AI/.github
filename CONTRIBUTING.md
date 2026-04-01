# Contributing to Dakera

Thanks for your interest in contributing to Dakera! This guide applies to all repositories under the [Dakera-AI](https://github.com/Dakera-AI) organization.

## Architecture Overview

Dakera is a multi-crate Rust project compiled into a single binary. The core engine handles agent memory, hybrid search (vector + BM25), built-in inference, and knowledge graph operations. A REST API, gRPC interface, MCP server, and WebAssembly dashboard sit above the engine. Native SDKs (Python, TypeScript, Rust, Go) wrap the REST API.

Before contributing, it's worth reading the [documentation](https://github.com/dakera-ai/dakera-docs) to understand the memory model and API surface. Dev setup instructions are in each repository's README.

## Getting Started

1. **Fork** the repository you want to contribute to
2. **Clone** your fork locally
3. **Create a branch** for your changes (`git checkout -b feature/my-change`)
4. **Make your changes** and commit with clear messages
5. **Push** to your fork and open a **Pull Request**

## Pull Request Guidelines

- Keep PRs focused on a single change
- Include tests for new functionality
- Update documentation if your change affects public APIs
- Follow the existing code style and conventions
- Write clear commit messages describing *why*, not just *what*

## Commit Messages

We follow [Conventional Commits](https://www.conventionalcommits.org/):

```
feat: add hybrid search weight parameter
fix: correct cosine similarity calculation for zero vectors
docs: update quickstart example
test: add integration tests for clustering
```

## Reporting Issues

- Use the issue tracker on the relevant repository
- Include steps to reproduce, expected behavior, and actual behavior
- For security vulnerabilities, see [SECURITY.md](SECURITY.md)

## Code of Conduct

All participants are expected to follow our [Code of Conduct](CODE_OF_CONDUCT.md). Be respectful, constructive, and inclusive.
