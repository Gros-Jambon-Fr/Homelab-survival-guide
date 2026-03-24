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

## Restic — Restore from Hetzner

If the local `/mnt/timeshift` drive is lost, restore from Hetzner.

```bash
# List available snapshots
restic -r <hetzner-repo> snapshots

# Restore to target directory
restic -r <hetzner-repo> restore <snapshot-id> --target /
```

⚠️ The Restic encryption key is stored in Vaultwarden.
