# Local Backup — Timeshift

## Purpose

System snapshots (OS, config files, packages). Allows quick rollback after a system update or accidental change.

Does **not** back up user/application data (handled by Borg).

## Configuration

- Destination: `/mnt/timeshift` (dedicated SSD)
- Type: RSYNC
- Retention: 7 daily snapshots
- Trigger: custom systemd timer (`timeshift-daily.timer`)

## Manual snapshot

```bash
sudo timeshift --create --comments "before update" --tags D
```

## List available snapshots

```bash
sudo timeshift --list
```
