# Backup — Overview

## 3-2-1 Strategy

- **3** copies of the data
- **2** different storage media
- **1** offsite copy

## What is backed up

| Data | Tool | Local | Offsite |
|---|---|---|---|
| System snapshots (OS + config + appdata) | Timeshift | ✅ `/mnt/timeshift` | ✅ Restic → Hetzner Storage Box |
| User/app data (Immich, OpenCloud, etc.) | Borg | ✅ `/mnt/backup` | ✅ rclone crypt → Hetzner Storage Box |
| Docker compose files | Git | ✅ Self-hosted Git | — |
| Automation scripts | Git | ✅ Self-hosted Git | — |
| System config files (host) | Git | ✅ Self-hosted Git | — |
| Passwords / secrets | Vaultwarden | ✅ (container) | ✅ Encrypted export via email |

> Both offsite syncs target the **same Hetzner Storage Box**, in separate directories — Borg data via rclone crypt, Timeshift snapshots via Restic.

## What is NOT backed up

- Docker images (rebuilt from compose files)
- Temporary files and caches

## Schedule

| Tool | Frequency | Retention |
|---|---|---|
| Timeshift | Daily (systemd timer) | 7 days |
| Borg | Configurable | 7 days |
| Restic (Timeshift → Hetzner Storage Box) | After each snapshot | `--keep-daily 7` |
| rclone (Borg → Hetzner Storage Box) | After each Borg backup | Incremental sync |
| Vaultwarden export | Weekly (Sunday 8pm) | Email |

## Detailed documentation

- [Timeshift](local-timeshift.md)
- [Borg](local-borg.md)
- [rclone → Hetzner Storage Box](offsite-rclone.md) — Borg data, encrypted
- [Restic → Hetzner Storage Box](offsite-restic.md) — Timeshift snapshots
- [Docker compose files](docker-compose.md)
- [Backup automation](automation.md)
