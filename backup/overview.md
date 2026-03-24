# Backup — Overview

## 3-2-1 Strategy

- **3** copies of the data
- **2** different storage media
- **1** offsite copy

## What is backed up

| Data | Tool | Local | Offsite |
|---|---|---|---|
| System snapshots (OS + config) | Timeshift | ✅ `/mnt/timeshift` | ✅ Restic → Hetzner |
| Docker application data | Borg | ✅ `/mnt/backup` | ✅ rclone crypt → Hetzner |
| System config files (host) | Git | ✅ Self-hosted Git | — |
| Passwords / secrets | Vaultwarden | ✅ (container) | ✅ Encrypted export via email |

## What is NOT backed up

- Docker images (rebuilt from compose files)
- Temporary files and caches

## Schedule

| Tool | Frequency | Retention |
|---|---|---|
| Timeshift | Daily (systemd timer) | 7 days |
| Borg | Configurable | 7 days |
| Restic (Timeshift → Hetzner) | After each snapshot | `--keep-daily 7` |
| rclone (Borg → Hetzner) | After each Borg backup | Incremental sync |
| Vaultwarden export | Weekly (Sunday 8pm) | Email |

## Detailed documentation

- [Timeshift](local-timeshift.md)
- [Borg](local-borg.md)
- [rclone → Hetzner](offsite-rclone.md)
- [Restic → Hetzner](offsite-restic.md)
