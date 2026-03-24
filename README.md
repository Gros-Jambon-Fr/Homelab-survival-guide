# Homelab Survival Guide

Documentation of my homelab resilience strategy: backup, security, and restoration procedures.

## Core principle — 3-2-1 Backup Strategy

| Copy | Location | Technology |
|---|---|---|
| Production | Internal drives (SSD + HDD) | — |
| Local backup | Dedicated internal drives | Timeshift + Borg |
| Offsite backup | Hetzner Storage Box (Germany) | rclone crypt + Restic |

## Structure

```
backup/         → What is backed up, where, and how
security/       → Access, secrets, 2FA
restoration/    → Step-by-step restoration procedures by scenario
scripts/        → Utility scripts (coming soon)
```

## Infrastructure

- Server: x86 mini PC running Debian
- ~40 Docker containers
- Remote access: WireGuard VPN
- Reverse proxy: Traefik

## Quick navigation

- [Backup overview](backup/overview.md)
- [Where to start when things go wrong](restoration/overview.md)
- [Secret recovery chain](restoration/vaultwarden-chain.md)
