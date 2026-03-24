# Backup Automation

## Overview

All backup tasks run automatically inside a dedicated Docker container using a self-hosted workflow orchestrator ([xyOps](https://github.com/nicholaswilde/xyops) — a cron-based event scheduler with a web UI).

The automation scripts are versioned in a dedicated Git repository, separate from the compose files.

```
Git repo (scripts)  ←→  xyOps container  →  Borg / Timeshift / Restic / rclone
```

## Why run scripts inside a container?

The backup container uses `nsenter` to access host binaries and filesystems without running privileged scripts directly on the host. This keeps the host clean while still allowing full access to system tools (Borg, Timeshift, rclone, Restic).

## Workflow schedule

### Daily backup (every day except Monday, 2am)

```
Borg local backup
  → Borg integrity check
  → Borg archive count check
  → Borg cloud sync (rclone → Hetzner)

Timeshift local snapshot
  → Timeshift integrity check
  → Timeshift snapshot count check
  → Timeshift cloud snapshot (Restic → Hetzner)
```

### Weekly backup (Monday, 2am)

Same as daily, with additional steps:
- Full Borg integrity check
- Full Timeshift cloud integrity check (Restic)

### Weekly Vaultwarden export (Sunday, 8pm)

Encrypted JSON export sent by email. See [recovery chain](../restoration/vaultwarden-chain.md).

### Weekly host reboot (Monday, 1am)

Triggered 1 hour before the weekly backup to ensure a clean state.

## Chain logic

Each step in a backup workflow uses **success chaining** — if a step fails, the rest of the chain is aborted. This prevents sending a corrupted backup to the cloud if the local backup failed.

Monitoring tasks (disk usage, uptime, container updates) use **complete chaining** — they always run regardless of the previous step's result.

## What happens if the automation container goes down?

No backups run. This is the single point of failure of the entire backup pipeline.

**Recovery**: bring the xyOps container back up from the compose repo, scripts are restored from the Git repo.

## Scripts repository structure

```
scripts/
├── backup/
│   ├── daily_borg_local_backup.sh
│   ├── daily_borg_cloud_backup.sh
│   ├── daily_timeshift_local_snapshot.sh
│   ├── daily_timeshift_cloud_snapshot.sh
│   ├── weekly_borg_integrity_check.sh
│   └── weekly_timeshift_cloud_integrity_check.sh
├── system/
│   ├── daily_disk_usage_check.sh
│   ├── daily_updates_check.sh
│   └── weekly_host_reboot.sh
├── containers/
│   ├── daily_containers_updates_check.sh
│   └── weekly_containers_update.sh
└── personal/
    └── weekly_vaultwarden_export.sh
```
