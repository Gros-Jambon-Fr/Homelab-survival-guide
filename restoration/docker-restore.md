# Docker Services Restoration

## Prerequisites

- Compose files available (from Git repo)
- appdata restored from Borg (see below)
- Docker installed and running

## Step 1 — Restore appdata from Borg

appdata (`/opt/docker/appdata/`) is part of the `/opt` Borg source.

```bash
# List available archives
borg list /mnt/backup/borg

# Restore appdata only
cd /
borg extract /mnt/backup/borg::<archive-name> opt/docker/appdata/
```

If `/mnt/backup` is unavailable, pull from Hetzner first:

```bash
rclone sync hetzner-crypt:borg /mnt/backup/borg --progress
```

See [Data restoration](data-restore.md) for full details.

## Step 2 — Clone the compose repo

```bash
git clone git@<your-git-server>:<org>/docker-compose-collection.git
cd docker-compose-collection
```

## Step 3 — Bring services back up

```bash
# For a single service
cd <service-folder>
docker compose up -d

# For all services (if scripted)
# adapt to your own structure
```

## Data loss window

appdata is covered by Borg (daily) synced offsite to Hetzner via rclone crypt. The maximum data loss window is less than 24 hours.
