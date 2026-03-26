# Backup Automation

## Overview

All backup tasks run automatically inside a dedicated Docker container using a self-hosted workflow orchestrator ([xyOps](https://www.xyops.io/) — a schedule-based event scheduler with a web UI).

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
  → Borg integrity check ──→ Borg cloud sync (rclone → Hetzner)
                         └──→ Borg archive count check (monitoring only)

Timeshift local snapshot
  → Timeshift integrity check ──→ Timeshift cloud snapshot (Restic → Hetzner)
                              └──→ Timeshift snapshot count check (monitoring only)
```

### Weekly backup (Monday, 2am)

Same as daily, with additional steps:
- Full Borg integrity check (instead of quick daily check)
- Full Timeshift cloud integrity check (Restic) — runs **after** the cloud snapshot

```
Timeshift local snapshot
  → Full Timeshift integrity check ──→ Timeshift cloud snapshot
                                   │     → Timeshift cloud integrity check
                                   └──→ Timeshift snapshot count check (monitoring only)
```

### Weekly Vaultwarden export

Encrypted JSON export sent by email. See [recovery chain](../restoration/vaultwarden-chain.md).

### Weekly host update & reboot (Monday, 1am)

System packages are updated first, then the host reboots. The reboot is triggered 1 hour before the weekly backup to ensure a clean state. If the update fails, the reboot is skipped.

## Chain logic

Backup workflows use two types of chaining:

**Success chaining** — if a step fails, the rest of the chain is aborted. Used for action steps (local backup, integrity check, cloud sync). This prevents sending a corrupted or missing backup to the cloud.

**Monitoring steps** (count checks) are **leaf nodes** — they run in parallel with the cloud sync, log a warning if days are missing, but never block the cloud backup. Their role is observability, not gating. Alerting will eventually be handled by Grafana once a database is provisioned.

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
│   ├── daily_timeshift_local_snapshot.sh
│   ├── daily_timeshift_cloud_snapshot.sh
│   ├── daily_timeshift_local_count_check.sh
│   ├── daily_timeshift_local_integrity_check.sh
│   ├── weekly_borg_integrity_check.sh
│   └── weekly_timeshift_cloud_integrity_check.sh
├── system/
│   ├── daily_disk_usage_check.sh
│   ├── daily_updates_check.sh
│   ├── weekly_host_update.sh
│   └── weekly_host_reboot.sh
├── containers/
│   ├── daily_containers_updates_check.sh
│   └── weekly_containers_update.sh
└── personal/
    └── weekly_vaultwarden_export.sh
```
