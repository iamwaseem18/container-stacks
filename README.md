# Container Stacks — Reusable Docker Compositions

> Reusable Docker stacks for AI and WordPress.

## What I Built

Production-grade Docker Compose stacks for the services our team deploys most — AI agents, WordPress sites, and internal tools. Each stack is version-pinned, health-checked, and documented.

### Stacks

**AnythingLLM (AI Agent)**
- Pinned image tags (no `:latest` in production)
- Default model and agent search configuration
- HTTP healthchecks for container orchestration
- Environment-driven configuration for multi-tenant deployments

**WordPress**
- Ansible pulls plugins from object storage at build time — no manual plugin installs post-deploy
- Caddy for automatic TLS
- wp-cli for automated search-replace during migrations

**OpenClaw (Cooperative Governance)**
- HTTP healthchecks and pinned images
- Handles KYC, digital signing, and cooperative governance

**Docker MCP Gateway (POC)**
- Proof-of-concept for safe agent tool access via Docker containers
- MCP (Model Context Protocol) gateway pattern for AI agent isolation

## Architecture Pattern

```
┌──────────────────────────────┐
│  Docker Compose Stack        │
│                              │
│  ┌──────────┐  ┌──────────┐ │
│  │  Service  │  │  Service │ │
│  │ (pinned)  │  │ (pinned) │ │
│  └─────┬────┘  └────┬─────┘ │
│        │  healthcheck │      │
│  ┌─────▼─────────────▼────┐ │
│  │        Caddy           │ │
│  │   (TLS + routing)      │ │
│  └────────────┬───────────┘ │
└───────────────┼──────────────┘
                │ :443
                ▼
```

## Decisions

| Decision | Why |
|---|---|
| Pinned image tags | Reproducible deploys; `:latest` caused silent regressions |
| Ansible for plugins | Plugin state is code, not a manual click in wp-admin |
| Caddy over Nginx | Automatic TLS with zero config; simpler reverse proxy syntax |
| HTTP healthchecks | Docker restarts unhealthy containers; no external monitoring needed for basic liveness |

## Stack

`Docker Compose` · `Caddy` · `Ansible` · `AnythingLLM` · `MCP` · `WordPress`

## Context

Built as Platform Engineer at [WeOwn](https://weown.net) (2025–present). Source code is proprietary.
