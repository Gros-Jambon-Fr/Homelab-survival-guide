# Offsite Backup — Timeshift → Hetzner via Restic

## Purpose

Send Timeshift snapshots to Hetzner Storage Box via Restic. Restic adds native deduplication and encryption.

## How it works

```
/mnt/timeshift  →  Restic  →  Hetzner Storage Box
```

- Native Restic encryption (key stored in Vaultwarden)
- Retention: `--keep-daily 7`

## Useful commands

```bash
# List snapshots
restic -r <repo> snapshots

# Check repository integrity
restic -r <repo> check
```
