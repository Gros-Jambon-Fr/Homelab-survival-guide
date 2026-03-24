# Offsite Backup — Borg → Hetzner via rclone

## Purpose

Sync the local Borg repository to Hetzner Storage Box, encrypted via rclone crypt.

## How it works

```
/mnt/backup/borg  →  rclone crypt  →  Hetzner Storage Box
```

- Incremental sync: only new blocks are transferred
- Client-side encryption: Hetzner only sees encrypted data
- rclone encryption key: stored in Vaultwarden

## rclone configuration

Config file: `~/.config/rclone/rclone.conf`

Two remotes configured:
- `hetzner`: SFTP remote pointing to the Storage Box
- `hetzner-crypt`: crypt remote layered on top of `hetzner`

## Useful commands

```bash
# Manual sync
rclone sync /mnt/backup/borg hetzner-crypt:borg --progress

# Check remote contents
rclone ls hetzner-crypt:borg
```
