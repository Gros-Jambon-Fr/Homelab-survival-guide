# System Restoration

## Timeshift — Quick rollback

Best option to undo a bad update or system change.

```bash
# List available snapshots
sudo timeshift --list

# Restore a snapshot
sudo timeshift --restore --snapshot <snapshot-name>
```

The system reboots automatically after restoration.

## Borg — Restore from Hetzner

If the local `/mnt/backup` drive is lost, restore from Hetzner via rclone + Borg.

```bash
# Sync Borg repo from Hetzner to a local directory
rclone sync hetzner-crypt:borg /mnt/backup --config ~/.config/rclone/rclone.conf

# List available archives
borg list /mnt/backup/borg

# Restore a specific archive
borg extract /mnt/backup/borg::<archive-name> --target /
```

⚠️ The rclone encryption key is stored in Vaultwarden.
