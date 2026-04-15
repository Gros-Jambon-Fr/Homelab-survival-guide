# Data Restoration — Borg

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

```bash
# Sync the Borg repo from Hetzner to a local drive
rclone sync hetzner-crypt:borg /mnt/backup/borg --progress

# Then restore as above
borg list /mnt/backup/borg
```

⚠️ The rclone encryption key is stored in Vaultwarden.

## Backed up sources

- `/etc` — system configuration
- `/opt` — Docker appdata, compose files, script-server
- `/root` — root home directory
- `/home/immich`
- `/home/<user>`
- `/home/opencloud`
- `/home/music`
