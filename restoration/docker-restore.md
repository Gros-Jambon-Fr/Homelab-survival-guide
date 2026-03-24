# Docker Services Restoration

## Prerequisites

- Compose files available (from Git repo)
- appdata restored from Timeshift (or accepted as partially lost)
- Docker installed and running

## Step 1 — Restore appdata from Timeshift

appdata (`/opt/docker/appdata/`) is covered by Timeshift snapshots only.

```bash
# List available snapshots
sudo timeshift --list

# Restore the most recent snapshot
sudo timeshift --restore --snapshot <snapshot-name>
```

Or restore a specific path only (without full system rollback):

```bash
# Mount a snapshot and copy appdata manually
ls /run/timeshift/backup/timeshift-linux/snapshots/
cp -a /run/timeshift/backup/timeshift-linux/snapshots/<snapshot>/localhost/opt/docker/appdata /opt/docker/
```

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

appdata is covered by Timeshift (daily) synced offsite to Hetzner via Restic. The maximum data loss window is less than 24 hours.
