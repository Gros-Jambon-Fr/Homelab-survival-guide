# Local Backup — Borg

## Purpose

Backup of user data, Docker application data, and system configuration. Borg provides native deduplication, compression and encryption.

## Backed up sources

- `/etc` — system configuration (network, SSH, Docker daemon, crontabs, systemd units…)
- `/opt` — Docker appdata, compose files, script-server
- `/root` — root home directory
- `/home/immich` — Immich photo library
- `/home/<user>` — user data
- `/home/opencloud` — OpenCloud data
- `/home/music` — music library

## Configuration

- Destination: `/mnt/backup` (dedicated HDD)
- Compression: lz4
- Retention: 7 days
- Exclusion: `/etc/machine-id` (machine-specific, must be regenerated on new hardware)

## Useful commands

```bash
# List archives
borg list /mnt/backup/borg

# Browse an archive
borg list /mnt/backup/borg::<archive-name>

# Repository info
borg info /mnt/backup/borg
```
