# Coolify Skill

[![Author](https://img.shields.io/badge/Author-Daniel_Rudaev-000000?style=flat)](https://github.com/daniel-rudaev)
[![Studio](https://img.shields.io/badge/Studio-D1DX-000000?style=flat)](https://d1dx.com)
[![Coolify](https://img.shields.io/badge/Coolify-Skill-6B16ED?style=flat&logo=coolify&logoColor=white)](https://coolify.io)
[![License](https://img.shields.io/badge/License-MIT-green?style=flat)](./LICENSE)

A complete reference for managing services on Coolify v4 — covering MCP operations (with gap documentation), REST API, artisan tinker for operations the API can't do, Docker Compose configuration, Traefik domain routing, project/environment organization, health monitoring, deployment patterns, and the most common failure modes. Built from real production work at [D1DX](https://d1dx.com).

## What's Included

| Topic | What it covers |
|-------|---------------|
| Method Decision Tree | When to use MCP vs REST API vs artisan tinker vs Livewire |
| MCP Operations | Full capability table, safe vs approval-required, documented gaps |
| REST API | Auth, service CRUD, restart, redeploy via direct `curl` calls |
| artisan tinker | Laravel model access via SSH for MCP gaps — move resources, rename services, direct DB queries |
| Livewire Internal API | How Coolify's browser UI works internally (and why it's not practical for automation) |
| Service Configuration | Docker Image / Compose / Git repo types, environment variable management |
| Domain Routing | Traefik label generation, custom domains, SSL via Let's Encrypt, Cloudflare DNS integration |
| Project Organization | Resource hierarchy (Project → Environment → Resource), moving resources between projects |
| Monitoring & Troubleshooting | Diagnostics-first approach, log access, common issue table (502, 504, OOM, SSL, stuck health checks) |
| Deployment Patterns | Rolling update, manual deploy, git-based webhook deploy |
| Critical Gotchas | 10 hard-won lessons — MCP gaps, memory limits, Traefik labels, Livewire checksums, volume persistence |

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

Copy `SKILL.md` into your agent's prompt or knowledge directory. The skill is structured markdown — works with any LLM agent that reads reference files.

## Structure

```
coolify-skill/
├── SKILL.md    — Main skill file (method decision tree, MCP, API, artisan tinker, gotchas)
├── README.md   — This file
└── LICENSE     — MIT
```

## Sources

Distilled from operating a Coolify v4 instance on AWS EC2 Frankfurt — managing 16+ resources across 10 projects including n8n, Chatwoot, OpenClaw, WAHA, and custom services. Covers patterns learned through production deployments, incident response, MCP gap discovery, artisan tinker workarounds, and Traefik/DNS integration with Cloudflare.

## Credits

Built by [Daniel Rudaev](https://github.com/daniel-rudaev) at [D1DX](https://d1dx.com).

## License

MIT License — Copyright (c) 2026 Daniel Rudaev @ D1DX
