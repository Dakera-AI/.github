# Contributing to Dakera

Thank you for helping. This guide applies to every public repository in the [Dakera-AI](https://github.com/Dakera-AI) organization.

## What is open to contributions

The Dakera server is a Rust engine distributed as a container image; its source is private. Everything around it is public and accepts pull requests:

- **SDKs:** [dakera-py](https://github.com/Dakera-AI/dakera-py), [dakera-js](https://github.com/Dakera-AI/dakera-js), [dakera-go](https://github.com/Dakera-AI/dakera-go), [dakera-rs](https://github.com/Dakera-AI/dakera-rs)
- **Integrations:** LangChain, LangChain.js, LlamaIndex, CrewAI, AutoGen, Vercel AI SDK, Strands Agents
- **Tools:** [dakera-cli](https://github.com/Dakera-AI/dakera-cli), [dakera-mcp](https://github.com/Dakera-AI/dakera-mcp), and the Homebrew, apt and rpm package repositories
- **Deployment:** [dakera-deploy](https://github.com/Dakera-AI/dakera-deploy), [dakera-helm](https://github.com/Dakera-AI/dakera-helm)

For server behaviour you would like changed, open an issue on [dakera-deploy](https://github.com/Dakera-AI/dakera-deploy/issues). Read the [documentation](https://dakera.ai/docs) for the memory model and the API first. Each repository's README explains its development setup.

## Workflow

1. For anything larger than a small fix, open an issue first so we can agree on the approach.
2. Fork the repository and create a branch (`git checkout -b fix/short-description`).
3. Make the change, with tests, and update the documentation when public behaviour changes.
4. Run the repository's tests and linters locally.
5. Open a pull request against `main` and fill in the template.

## Pull requests

- One change per pull request.
- New behaviour comes with tests; bug fixes come with a test that failed before.
- Follow the existing style of the repository.
- Explain *why* in the description, not only *what*.

## Commit messages

We use [Conventional Commits](https://www.conventionalcommits.org/):

```
feat: add hybrid search weight parameter
fix: retry recall on 503 with Retry-After
docs: update the quickstart
test: cover namespace quota errors
```

## Reporting issues

Use the issue tracker of the relevant repository; [SUPPORT.md](SUPPORT.md) lists them. Report security issues privately, as [SECURITY.md](SECURITY.md) describes.

## Code of conduct

Everyone taking part follows our [Code of Conduct](CODE_OF_CONDUCT.md).
