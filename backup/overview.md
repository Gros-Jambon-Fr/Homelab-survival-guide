# Backup — Overview

## 3-2-1 Strategy

- **3** copies of the data
- **2** different storage media
- **1** offsite copy

## What is backed up

| Data | Tool | Local | Offsite |
|---|---|---|---|
| User/app data, appdata, system config | Borg | ✅ `/mnt/backup` | ✅ rclone crypt → Hetzner Storage Box |
| System rollback snapshots (OS) | Timeshift | ✅ `/mnt/timeshift` | — |
| Docker compose files | Git | ✅ Self-hosted Git | — |
| Automation scripts | Git | ✅ Self-hosted Git | — |
| System config files (host) | Git | ✅ Self-hosted Git | — |
| Passwords / secrets | Vaultwarden | ✅ (container) | ✅ Encrypted export via email |

> Offsite backup targets a **Hetzner Storage Box** via rclone crypt (client-side encryption). Timeshift is local only — it is a rollback tool, not a disaster recovery solution.

## What is NOT backed up

- Docker images (rebuilt from compose files)
- Temporary files and caches

## Schedule

| Tool | Frequency | Retention |
|---|---|---|
| Timeshift | Daily (systemd timer) | 7 days (local only) |
| Borg | Daily | 7 days |
| rclone (Borg → Hetzner Storage Box) | After each Borg backup | Incremental sync |
| Vaultwarden export | Weekly | Email |

## Detailed documentation

- [Timeshift](local-timeshift.md)
- [Borg](local-borg.md)
- [rclone → Hetzner Storage Box](offsite-rclone.md) — Borg data, encrypted
- [Docker compose files](docker-compose.md)
- [Backup automation](automation.md)
