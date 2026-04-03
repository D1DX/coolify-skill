# Coolify Skill

[![Author](https://img.shields.io/badge/Author-Daniel_Rudaev-000000?style=flat)](https://github.com/daniel-rudaev)
[![Studio](https://img.shields.io/badge/Studio-D1DX-000000?style=flat)](https://d1dx.com)
[![Coolify](https://img.shields.io/badge/Coolify-Skill-6B16ED?style=flat&logo=coolify&logoColor=white)](https://coolify.io)
[![License](https://img.shields.io/badge/License-MIT-green?style=flat)](./LICENSE)

A complete reference for managing services on Coolify v4 — covering MCP operations, the REST API, Docker Compose configuration, Traefik domain routing, environment variables, health monitoring, deployment patterns, and the most common failure modes. Built from real production work at [D1DX](https://d1dx.com).

## What's Included

| Topic | What it covers |
|-------|---------------|
| MCP Operations | Safe vs approval-required operations, common queries (list, get, envs, deployments) |
| REST API | Auth, service CRUD, restart, redeploy via direct `curl` calls |
| Service Configuration | Docker Image / Compose / Git repo types, environment variable management |
| Domain Routing | Traefik label generation, custom domains, SSL via Let's Encrypt, Cloudflare DNS integration |
| Monitoring & Troubleshooting | Health checks, log access, common issue table (502, OOM, SSL, slow response) |
| Deployment Patterns | Rolling update, manual deploy, git-based webhook deploy |
| Critical Gotchas | Memory limits, Coolify as source of truth, Traefik label conflicts, Let's Encrypt rate limits, volume persistence, backup requirements |

## Install

### Claude Code

```bash
git clone https://github.com/D1DX/coolify-skill.git
cp -r coolify-skill ~/.claude/skills/coolify
```

Or as a git submodule:

```bash
git submodule add https://github.com/D1DX/coolify-skill.git path/to/skills/coolify
```

### Other AI Agents

Copy `SKILL.md` (and supporting files) into your agent's prompt or knowledge directory. The skill is structured markdown — works with any LLM agent that reads reference files.

## Structure

```
coolify-skill/
├── SKILL.md    — Main skill file
└── README.md   — This file
```

## Sources

Distilled from operating a Coolify v4 instance on AWS EC2 Frankfurt — managing 10+ services including n8n, Chatwoot, WAHA, and custom Hono workers. Covers patterns learned through production deployments, incident response, and Traefik/DNS integration with Cloudflare.

## Credits

Built by [Daniel Rudaev](https://github.com/daniel-rudaev) at [D1DX](https://d1dx.com).

## License

MIT License — Copyright (c) 2026 Daniel Rudaev @ D1DX
