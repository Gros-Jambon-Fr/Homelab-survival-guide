# Backup Automation

## Overview

All backup tasks run automatically inside a dedicated Docker container using a self-hosted workflow orchestrator ([xyOps](https://www.xyops.io/) — a schedule-based event scheduler with a web UI).

The automation scripts are versioned in a dedicated Git repository, separate from the compose files.

```
Git repo (scripts)  ←→  xyOps container  →  Borg / Timeshift / rclone
```

## Why run scripts inside a container?

The backup container uses `nsenter` to access host binaries and filesystems without running privileged scripts directly on the host. This keeps the host clean while still allowing full access to system tools (Borg, Timeshift, rclone).

## Workflow schedule

### Daily backup (every day except Monday, 2am)

```
Borg local backup
  → Borg integrity check ──→ Borg cloud sync (rclone → Hetzner)
                         └──→ Borg archive count check (monitoring only)

Timeshift local snapshot
  → Timeshift integrity check → Timeshift snapshot count check
```

### Weekly backup (Monday, 2am)

Same as daily, with additional steps:
- Full Borg integrity check (instead of quick daily check)

### Weekly Vaultwarden export

Encrypted JSON export sent by email. See [recovery chain](../restoration/vaultwarden-chain.md).

### Weekly host update & reboot (Monday, 1am)

The host reboots weekly, 1 hour before the weekly backup to ensure a clean state. System package updates are triggered manually via a separate web UI ([script-server](https://github.com/bugy/script-server)) running directly on the host — keeping manual oversight over what gets installed.

## Chain logic

Backup workflows use two types of chaining:

**Success chaining** — if a step fails, the rest of the chain is aborted. Used for action steps (local backup, integrity check, cloud sync). This prevents sending a corrupted or missing backup to the cloud.

**Monitoring steps** (count checks) run after the integrity check and log a warning if archives are missing, but never block the cloud sync. Their role is observability, not gating.

Monitoring tasks outside backup workflows (disk usage, uptime, container updates) use **complete chaining** — they always run regardless of the previous step's result.

## What happens if the automation container goes down?

No backups run. This is the single point of failure of the entire backup pipeline.

**Recovery**: bring the xyOps container back up from the compose repo, scripts are restored from the Git repo.

## Scripts repository structure

```
scripts/
├── backup/
│   ├── daily_borg_local_backup.sh
│   ├── daily_borg_cloud_backup.sh
│   ├── daily_borg_local_count_check.sh
│   ├── daily_borg_local_integrity_check.sh
│   ├── weekly_borg_local_integrity_check.sh
│   ├── daily_timeshift_local_snapshot.sh
│   ├── daily_timeshift_local_count_check.sh
│   ├── daily_timeshift_local_integrity_check.sh
│   └── weekly_timeshift_local_integrity_check.sh
├── system/
│   ├── daily_disks_usage_check.sh
│   ├── daily_host_updates_check.sh
│   ├── daily_host_uptime_check.sh
│   ├── weekly_host_update.sh
│   └── weekly_host_reboot.sh
├── containers/
│   ├── daily_containers_updates_check.sh
│   ├── daily_containers_last_update_check.sh
│   ├── daily_gluetun_vpn_check.sh
│   ├── weekly_containers_update.sh
│   └── weekly_dockcheck_update_check.sh
└── personal/
    └── weekly_vaultwarden_export.sh
```
