---
name: coolify
description: Coolify v4 — service management, deployment, environment variables, Docker Compose, Traefik routing, health monitoring, and MCP integration. Auto-triggers on Coolify, service deployment, server management, and Traefik routing tasks.
disable-model-invocation: false
user-invocable: true
argument-hint: "task description"
---

# Coolify Management Skill

Complete reference for managing services on Coolify v4.

**As of March 2026.** Coolify v4 (self-hosted PaaS).

---

## 1. MCP Operations

Use the project's Coolify MCP server to inspect services. Read operations are safe; write operations require approval.

### Common MCP Queries

```
# List all services
Use coolify MCP: list_services

# Get service details
Use coolify MCP: get_service with service_id

# Get service environment variables
Use coolify MCP: get_service_envs with service_id

# Check deployment status
Use coolify MCP: get_service with service_id → check status field

# View recent deployments
Use coolify MCP: list_deployments with service_id
```

### Safe Operations (do without asking)
- List services, get details
- Read environment variables
- View deployment logs
- Check health status

### Approval Required
- Restart services
- Redeploy services
- Change environment variables
- Add/remove services
- Modify Docker Compose
- Change domain routing

---

## 3. Coolify API (Direct)

Auth: `Authorization: Bearer {COOLIFY_TOKEN}`

```bash
export COOLIFY_BASE="https://coolify.yourdomain.com"
export COOLIFY_TOKEN="your-token"

# List servers
curl -s "$COOLIFY_BASE/api/v1/servers" \
  -H "Authorization: Bearer $COOLIFY_TOKEN"

# List services
curl -s "$COOLIFY_BASE/api/v1/services" \
  -H "Authorization: Bearer $COOLIFY_TOKEN"

# Get specific service
curl -s "$COOLIFY_BASE/api/v1/services/{uuid}" \
  -H "Authorization: Bearer $COOLIFY_TOKEN"

# Restart a service
curl -s -X POST "$COOLIFY_BASE/api/v1/services/{uuid}/restart" \
  -H "Authorization: Bearer $COOLIFY_TOKEN"

# Deploy/redeploy
curl -s -X POST "$COOLIFY_BASE/api/v1/services/{uuid}/deploy" \
  -H "Authorization: Bearer $COOLIFY_TOKEN"
```

---

## 4. Service Configuration

### Docker Compose in Coolify

Coolify supports raw Docker Compose definitions. Services can be:
- **Docker Image** — pull and run a public/private image
- **Docker Compose** — full compose file with multiple containers
- **Git Repository** — build from Dockerfile in a repo

### Environment Variables

Set via Coolify UI or API. Variables are injected into containers at runtime.

```bash
# List env vars for a service
curl -s "$COOLIFY_BASE/api/v1/services/{uuid}/envs" \
  -H "Authorization: Bearer $COOLIFY_TOKEN"

# Add/update env var
curl -s -X POST "$COOLIFY_BASE/api/v1/services/{uuid}/envs" \
  -H "Authorization: Bearer $COOLIFY_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"key": "API_KEY", "value": "new-value", "is_build_time": false}'
```

### Domain Routing (Traefik)

Coolify auto-generates Traefik labels. To add a custom domain:
1. Set domain in Coolify UI → Service → Settings → Domains
2. DNS must point to server IP (CNAME or A record in Cloudflare)
3. Coolify auto-provisions SSL via Let's Encrypt (or Cloudflare handles SSL when proxied)

When DNS is managed via Terraform or Cloudflare, add an A record pointing to the server IP, proxied as appropriate.

---

## 5. Monitoring & Troubleshooting

### Check Service Health

```bash
# Quick health check via HTTP
curl -s -o /dev/null -w "%{http_code}" https://your-service.yourdomain.com/healthz

# Check multiple services
for url in service1.yourdomain.com/healthz service2.yourdomain.com; do
  code=$(curl -s -o /dev/null -w "%{http_code}" "https://$url" 2>/dev/null)
  echo "$url: $code"
done
```

### View Logs

Via Coolify UI: Service → Logs tab.
Via API: deployment logs available through the MCP or API.

### Common Issues

| Symptom | Likely Cause | Fix |
|---------|-------------|-----|
| 502 Bad Gateway | Service crashed or restarting | Check Coolify logs, restart service |
| OOM Killed | Service exceeded memory limit | Increase limit or reduce load |
| Connection refused | Service not running | Check Docker status, restart |
| SSL error | Certificate expired/missing | Coolify auto-renews; check Traefik logs |
| Slow response | Memory pressure on server | Check `docker stats`, identify memory hog |

---

## 6. Deployment Patterns

### Rolling Update (default)

Coolify deploys new container, waits for health check, then stops old container. Zero-downtime if health check is configured.

### Manual Deploy

```bash
# Via API
curl -s -X POST "$COOLIFY_BASE/api/v1/services/{uuid}/deploy" \
  -H "Authorization: Bearer $COOLIFY_TOKEN"
```

### Git-Based Deploy

For services linked to a git repo:
1. Push to main branch
2. Coolify webhook triggers build
3. Dockerfile builds image
4. Deploy new container

---

## 7. Backup Considerations

**Currently missing (known issue):**
- No automated backups for n8n database
- No automated backups for Chatwoot database
- Session data in Docker volume (no backup)

**Recommendation:** Set up `pg_dump` cron job + R2 upload for critical databases.

---

## 8. Critical Gotchas

1. **limited RAM, no swap configured** — OOM kills are real. Always set memory limits on services.
2. **Self-hosted server** — all services on one instance means resource contention is frequent.
3. **Coolify UI is the source of truth** — env vars and compose config live in Coolify, not in git.
4. **Traefik labels auto-generated** — don't manually set Traefik labels in compose files used with Coolify.
5. **Let's Encrypt rate limits** — too many cert requests (>5/week for same domain) get rate-limited.
6. **Docker volume persistence** — data persists in named volumes across deploys but NOT across server rebuilds.
7. **No database backups** — critical gap. PostgreSQL data for n8n and Chatwoot has no backup schedule.
9. **Database version EOL** — Chatwoot's PostgreSQL is end-of-life. Upgrade needed.
10. **Coolify updates** — update Coolify itself carefully. Test in off-hours, have rollback plan.

