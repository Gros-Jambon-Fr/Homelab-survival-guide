# Data Restoration — Borg

The Borg repository is encrypted with `repokey-blake2`. The passphrase is required for every command — retrieve it from Vaultwarden before starting.

```bash
export BORG_PASSPHRASE="<passphrase from Vaultwarden>"
```

## From local backup (`/mnt/backup`)

```bash
# List available archives
borg list /mnt/backup/borg

# Browse an archive
borg list /mnt/backup/borg::<archive-name>

# Restore a full archive
cd /
borg extract /mnt/backup/borg::<archive-name>

# Restore a specific path
borg extract /mnt/backup/borg::<archive-name> home/immich/
borg extract /mnt/backup/borg::<archive-name> opt/docker/appdata/
```

## From Hetzner (offsite backup)

If `/mnt/backup` is lost, pull the Borg repo back from Hetzner first.

The offsite copy has two layers of encryption: Borg's own `repokey-blake2` + rclone crypt on top. To restore, you need both the rclone encryption key (in Vaultwarden) and the Borg passphrase (also in Vaultwarden).

```bash
# Sync the Borg repo from Hetzner to a local drive
rclone sync hetzner-crypt:borg /mnt/backup/borg --progress

# Then restore as above (BORG_PASSPHRASE must be set)
borg list /mnt/backup/borg
```

⚠️ Both the rclone encryption key and the Borg passphrase are stored in Vaultwarden.

## Backed up sources

- `/etc` — system configuration
- `/opt` — Docker appdata, compose files, script-server
- `/root` — root home directory
- `/home/immich`
- `/home/<user>`
- `/home/opencloud`
- `/home/music`
