---
tags: [brand/forge42, type/runbook, status/active]
updated: 2026-04-19
---

# Backup and Recovery

Backup strategy for all LUNA services and data. Follows the **3-2-1 rule**: three copies, on two different media, with one offsite.

## Backup Status

| Copy | Location | Method | Status |
|---|---|---|---|
| Copy 1 | LUNA NVMe (live data) | Docker volumes | Active |
| Copy 2 | NAS (local backup) | docker-volume-backup via NFS | **PLACEHOLDER — pending NAS purchase** |
| Copy 3 | Backblaze B2 (offsite) | Synology Hyper Backup | **PLACEHOLDER — pending NAS purchase** |

**Current state:** No automated backup is running. This is the highest priority item after the NAS is purchased.

**Interim option:** Back up Docker volumes to Aurora's large storage array (`G:` drive, 16TB) manually until the NAS arrives. This covers the "two media" requirement but not offsite.

## Backup Components

### Docker Volume Backup

**Tool:** `offen/docker-volume-backup` (runs as a Docker container)

**Schedule:** Nightly at 3:00 AM
- Gracefully stops containers before backup to prevent partial writes
- Compresses volumes to tar archives
- Uploads to NAS NFS share

**Setup (when NAS is ready):**
```yaml
# In the backup Docker Compose stack
services:
  backup:
    image: offen/docker-volume-backup:latest
    restart: always
    environment:
      BACKUP_CRON_EXPRESSION: "0 3 * * *"
      BACKUP_FILENAME: "backup-%Y-%m-%dT%H-%M-%S.tar.gz"
      BACKUP_RETENTION_DAYS: "7"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock:ro
      - nas_backup:/archive
      # Add each volume to back up:
      - postgres_data:/backup/postgres_data:ro
      - litellm_data:/backup/litellm_data:ro
```

### PostgreSQL Logical Backups

**Tool:** `pg_dump` via cron or n8n scheduled workflow

In addition to volume backups, each Postgres database gets a logical dump nightly. Volume backups can be corrupted; logical dumps are always restorable.

```bash
# Run per database — add to cron or n8n
docker exec postgres pg_dump -U postgres -d n8n > /backups/n8n_$(date +%Y%m%d).sql
docker exec postgres pg_dump -U postgres -d authentik > /backups/authentik_$(date +%Y%m%d).sql
# etc. for each database
```

**Backup directory:** `/var/backups/postgres/` — gets included in the docker-volume-backup pickup.

### Backblaze B2 Offsite

**Tool:** Synology Hyper Backup (runs on the NAS)

Synology Cloud Sync pushes encrypted snapshots from the NAS to Backblaze B2. At $6/TB/month, B2 is the cheapest reliable offsite option.

**Retention policy (suggested):**
- Daily snapshots: keep 7 days
- Weekly snapshots: keep 4 weeks
- Monthly snapshots: keep 12 months

## Recovery Procedures

### Recover a Single Docker Volume

```bash
# 1. Stop the affected service
docker compose stop <service>

# 2. Extract the backup
tar -xzf backup-YYYY-MM-DD.tar.gz -C /tmp/restore

# 3. Restore the volume
docker run --rm \
  -v <volume_name>:/restore \
  -v /tmp/restore:/backup \
  alpine sh -c "cd /restore && tar xvf /backup/<volume_name>.tar"

# 4. Start the service
docker compose start <service>
```

### Recover a PostgreSQL Database

```bash
# 1. Stop services using the database
docker compose stop n8n

# 2. Drop and recreate the database
docker exec -it postgres psql -U postgres -c "DROP DATABASE n8n;"
docker exec -it postgres psql -U postgres -c "CREATE DATABASE n8n;"

# 3. Restore from logical dump
docker exec -i postgres psql -U postgres -d n8n < /backups/n8n_20260419.sql

# 4. Start the service
docker compose start n8n
```

### Full Stack Recovery

Full recovery procedure when hardware fails:
1. Install Ubuntu Server 24.04 LTS on new hardware
2. Install Docker Engine + Docker Compose v2
3. Install Tailscale
4. `git clone` infrastructure repo from GitHub
5. Copy Infisical secrets (or re-enter manually)
6. `docker compose up` per stack
7. Restore Docker volumes from NAS backup
8. Restore Postgres dumps from NAS backup
9. Verify each service is healthy in Uptime Kuma

**Target:** Back online in under 2 hours from hardware failure.

## Monthly Restore Test Checklist

The most overlooked step. Untested backups are not real backups.

- [ ] Pick one database (e.g., n8n) and restore it to a test container
- [ ] Verify the restored data is intact and queryable
- [ ] Pick one Docker volume and verify it restores cleanly
- [ ] Document the result with date in log: `90-Meta/log.md`
- [ ] Note any issues found and fix before the next test

## Related

- [[docker-services]] — all services and their volumes
- [[docker-commands]] — Docker commands for manual operations
- [[luna-plan]] — NAS purchase is Phase 2 of the build plan
- [[incident-response]] — what to do when things break
