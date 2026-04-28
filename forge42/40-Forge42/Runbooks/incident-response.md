---
tags: [brand/forge42, type/runbook, status/active]
updated: 2026-04-19
---

# Incident Response

Runbook for service outages and incidents on the Forge42 stack. Follows a triage-then-escalate pattern.

## Severity Levels

| Level | Description | Examples | Response Time |
|---|---|---|---|
| P0 — Critical | All AI services down; no access to any tool | LUNA unreachable, Tailscale down | Immediate |
| P1 — High | Core service failure affecting active work | LiteLLM down, Postgres down | Within 1 hour |
| P2 — Medium | Individual service failure, workaround exists | AnythingLLM down, n8n down | Same day |
| P3 — Low | Minor degradation, no blocker | Monitoring alert, slow response | Next scheduled session |

## First Response Checklist

When something breaks, check in this order before making changes:

1. **Check ntfy** — did an alert come in? What service?
2. **Check Uptime Kuma** at `http://luna:3001` — which services are red?
3. **SSH into LUNA** — is the machine responsive?
   ```bash
   ssh marty@192.168.0.11   # LAN
   ssh marty@<luna-ts-ip>   # Tailscale
   ```
4. **Check Docker status:**
   ```bash
   docker ps -a
   # Look for containers with status "Exited" or restart loops
   ```
5. **Read the logs of the failed service:**
   ```bash
   docker logs --tail 200 <container_name>
   # Or via Portainer: Container → Logs
   ```
6. **Try a restart:**
   ```bash
   docker compose restart <service_name>
   # Wait 30 seconds, then check again
   ```

## Common Failure Patterns

### Service Won't Start (Restart Loop)

```bash
# Check logs for the error
docker logs --tail 50 <container_name>

# Common causes:
# - Postgres not yet ready → add depends_on with healthcheck
# - Missing environment variable → check Infisical secret was loaded
# - Port conflict → check another service isn't on the same port
# - Volume permissions → check ownership of mounted directories
```

### PostgreSQL Connection Errors

```bash
# Verify postgres is running
docker ps | grep postgres

# Check postgres is accepting connections
docker exec postgres pg_isready -U postgres

# Check which databases exist
docker exec -it postgres psql -U postgres -c "\l"

# Check connections are within limits
docker exec -it postgres psql -U postgres -c "SELECT count(*) FROM pg_stat_activity;"
```

### LiteLLM Not Routing Correctly

```bash
# Check LiteLLM health
curl http://localhost:4000/health

# Check available models
curl http://localhost:4000/model/info

# Check LiteLLM logs for API key errors
docker logs --tail 100 litellm

# Verify OpenRouter API key in Infisical is current
```

### Disk Space Full

```bash
# Check disk usage
df -h

# Check Docker's usage
docker system df

# Remove unused images and containers
docker system prune -a

# Check largest Docker volumes
du -sh /var/lib/docker/volumes/*
```

### Memory Pressure (Services Crashing)

```bash
# Check current memory usage
free -h

# Check which containers are using most memory
docker stats --no-stream

# Consider stopping non-essential services temporarily
docker compose stop appflowy hoarder glance
```

## P0 Response: LUNA Unreachable

If you cannot SSH into LUNA or reach it via Tailscale:

1. **Check if LUNA is on the LAN:** Ping `192.168.0.11` from Nova (MacBook)
2. **Check Tailscale node status:** `tailscale status` from Nova — is LUNA listed as connected?
3. **Physical access:** If at home, check LUNA is powered on and not frozen
4. **Power cycle:** If unresponsive, force restart LUNA
5. **On boot:** Docker services with `restart: always` should come back automatically
6. **Verify services after restart:** Check Uptime Kuma, spot-check each service

## Data Incident Response

If data loss or corruption is suspected:

1. **STOP writes immediately.** Take the affected service offline before making anything worse.
   ```bash
   docker compose stop <service>
   ```
2. **Preserve the evidence.** Copy logs before they rotate.
   ```bash
   docker logs <container> > /tmp/incident-$(date +%Y%m%d-%H%M%S).log
   ```
3. **Assess scope.** Which data is affected? Which service(s)?
4. **Check backup recency.** When was the last successful backup?
5. **Restore from backup.** Follow [[backup-recovery]] restore procedures.
6. **Document the incident** as a log entry in `90-Meta/log.md` (Checkpoint 7 per [[operating-model]]).

## Post-Incident

After resolving any P0 or P1 incident:

1. **Root cause:** What actually failed and why?
2. **Timeline:** When did it start? When detected? When resolved?
3. **Prevention:** What change prevents recurrence?
4. **Log entry:** Add to `90-Meta/log.md` with date, action-type `update`, description of incident and resolution

The post-mortem is a hard line — always done by Marty, not delegated to agents (per [[zero-human-narrative]] Director responsibilities).

## Escalation

There is no escalation beyond Marty — this is a zero-human operation. If a problem exceeds Marty's ability to resolve:
- Take the service offline (protect data integrity)
- Search the service's documentation and community forums
- Ask Claude Code or Open WebUI for debugging help
- Open a support ticket with the service vendor if applicable

## Related

- [[docker-commands]] — Docker operations reference
- [[backup-recovery]] — restore procedures
- [[docker-services]] — service inventory and ports
- [[home-network]] — machine IPs for SSH access
