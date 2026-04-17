# Offsite Backup — Borg → Hetzner via rclone

## Purpose

Sync the local Borg repository to Hetzner Storage Box, encrypted via rclone crypt.

## How it works

```
/mnt/backup/borg  →  rclone crypt  →  Hetzner Storage Box
```

- Incremental sync: only new blocks are transferred
- Double encryption: Borg's `repokey-blake2` + rclone crypt on top
- Hetzner only sees doubly-encrypted data
- Both keys are stored in Vaultwarden

## rclone configuration

Config file: `~/.config/rclone/rclone.conf`

Two remotes configured:
- `hetzner`: SFTP remote pointing to the Storage Box
- `hetzner-crypt`: crypt remote layered on top of `hetzner`

## Restoring from Hetzner

Both keys are needed:
1. **rclone encryption key** — lets rclone decrypt the outer layer and reveal the Borg repo files
2. **Borg passphrase** — lets Borg decrypt the repository content

Both are stored in Vaultwarden. See [data restoration](../restoration/data-restore.md) for the full procedure.

## Useful commands

```bash
# Manual sync (run on host, BORG_PASSPHRASE not needed for rclone itself)
rclone sync /mnt/backup/borg hetzner-crypt:borg --progress

# Check remote contents
rclone ls hetzner-crypt:borg
```
