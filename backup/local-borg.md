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

- Destination: `/mnt/backup/borg` (dedicated HDD)
- Encryption: `repokey-blake2`
- Compression: lz4
- Retention: 7 days
- Exclusions: `/etc/machine-id` (machine-specific, must be regenerated on new hardware), socket files under `/opt/docker/appdata/`
- `--one-file-system`: prevents traversal into Docker bind mounts

## Encryption

The repository uses `repokey-blake2` encryption. The passphrase is stored in two places:

- **Vaultwarden** — primary copy (under the backup entry)
- **Infisical** (`/backup` path, `BORG_PASSPHRASE` key) — used by the automation scripts

The repository key (repokey) is also stored in Vaultwarden as a separate entry. In a full disaster scenario where the repository is intact but the key is lost, restoring Vaultwarden first is the prerequisite — see [Vaultwarden recovery chain](../restoration/vaultwarden-chain.md).

## Useful commands

All borg commands require the passphrase. Set it before running any command:

```bash
export BORG_PASSPHRASE="<passphrase from Vaultwarden>"

# List archives
borg list /mnt/backup/borg

# Browse an archive
borg list /mnt/backup/borg::<archive-name>

# Repository info
borg info /mnt/backup/borg
```
