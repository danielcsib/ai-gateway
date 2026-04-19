# AI Gateway

A fork of [envoyproxy/ai-gateway](https://github.com/envoyproxy/ai-gateway) — an Envoy-based gateway for routing and managing AI/LLM traffic.

## Overview

AI Gateway provides a unified interface for routing requests to various AI providers (OpenAI, Anthropic, Gemini, Ollama, etc.) with features like:

- **Unified API**: Single endpoint for multiple LLM backends
- **Load balancing**: Distribute requests across providers
- **Rate limiting**: Control request rates per provider
- **Observability**: Metrics, tracing, and logging
- **Local models**: First-class support for Ollama and other local providers

## Prerequisites

- Go 1.22+
- Envoy Proxy (see [.envoy-version](.envoy-version))
- Docker (optional, for local development)

## Getting Started

### Local Development with Ollama

1. Copy the environment file and configure your keys:
   ```bash
   cp .env.ollama .env.local
   # Edit .env.local with your settings
   ```

2. Start the gateway:
   ```bash
   go run ./cmd/ai-gateway
   ```

> **Personal note:** I primarily use this with Ollama running `llama3` locally on port 11434.
> The `.env.local` approach works well — just make sure Ollama is running before starting the gateway.

### Configuration

See the [docs](./docs) directory for full configuration reference.

## Project Structure

```
.
├── cmd/            # Main entry points
├── internal/       # Internal packages
├── pkg/            # Public packages
├── config/         # Example configurations
└── docs/           # Documentation
```

## Contributing

Please read [CONTRIBUTING.md](./CONTRIBUTING.md) before submitting pull requests.

## License

Apache 2.0 — see [LICENSE](./LICENSE) for details.
