# Local Backup — Borg

## Purpose

Backup of Docker application data. Borg provides native deduplication, compression and encryption.

## Backed up sources

- `/home/immich` — Immich photo library
- `/home/<user>` — user data
- `/home/nextcloud` — Nextcloud data
- `/home/music` — music library

## Configuration

- Destination: `/mnt/backup` (dedicated HDD)
- Compression: lz4
- Retention: 7 days

## Useful commands

```bash
# List archives
borg list /mnt/backup/borg

# Browse an archive
borg list /mnt/backup/borg::<archive-name>

# Repository info
borg info /mnt/backup/borg
```
