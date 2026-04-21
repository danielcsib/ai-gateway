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
> I also run `mistral` as a fallback: `ollama pull mistral` and set `OLLAMA_FALLBACK_MODEL=mistral` in `.env.local`.
> I've also been experimenting with `codellama` for code-related queries — `ollama pull codellama` and
> point specific routes at it via `OLLAMA_CODE_MODEL=codellama` in `.env.local`.
> 
> **Tip:** If Ollama is slow on first request, it's loading the model into memory. Subsequent requests
> are much faster. Run `ollama run llama3` once in a terminal to pre-warm the model before starting the gateway.
>
> **Tip:** To quickly check which models you have available locally, run `ollama list`. Handy when
> you forget what you've pulled and want to add a new route without downloading anything extra.
>
> **Tip:** I've found that setting `OLLAMA_NUM_PARALLEL=2` in `.env.local` helps when testing
> multiple routes concurrently — prevents requests from queuing up behind each other during dev.
>
> **Tip:** On macOS with Apple Silicon, Ollama uses the GPU by default which is great, but if you're
> running other GPU-heavy tasks, set `OLLAMA_NUM_GPU=0` in `.env.local` to force CPU-only mode and
> avoid memory pressure. Slower, but more stable during heavy multitasking.
>
> **Tip:** When switching between models frequently during testing, `ollama ps` shows which models
> are currently loaded in memory. Useful for debugging latency spikes — if a model got unloaded,
> the next request will be slow while it reloads.

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
