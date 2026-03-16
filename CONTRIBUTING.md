# Contributing to Dakera

Thanks for your interest in contributing to Dakera! This guide applies to all repositories under the Dakera-AI organization.

## Getting Started

1. **Fork** the repository you want to contribute to
2. **Clone** your fork locally
3. **Create a branch** for your changes (`git checkout -b feature/my-change`)
4. **Make your changes** and commit with clear messages
5. **Push** to your fork and open a **Pull Request**

## Development Setup

### Core Engine (Rust)

```bash
git clone https://github.com/Dakera-AI/dakera.git
cd dakera
cargo build
cargo test
```

### Python SDK

```bash
git clone https://github.com/Dakera-AI/dakera-py.git
cd dakera-py
pip install -e ".[dev]"
pytest
```

### JavaScript SDK

```bash
git clone https://github.com/Dakera-AI/dakera-js.git
cd dakera-js
npm install
npm test
```

### Go SDK

```bash
git clone https://github.com/Dakera-AI/dakera-go.git
cd dakera-go
go test ./...
```

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
docs: update Python SDK quickstart example
test: add integration tests for clustering
```

## Reporting Issues

- Use the issue tracker on the relevant repository
- Include steps to reproduce, expected behavior, and actual behavior
- For security vulnerabilities, see [SECURITY.md](SECURITY.md)

## Code of Conduct

All participants are expected to follow our [Code of Conduct](CODE_OF_CONDUCT.md). Be respectful, constructive, and inclusive.

## License

By contributing, you agree that your contributions will be licensed under the MIT License.
