# System Config Restoration

System config files are versioned in a dedicated Git repository (`homelab-config`).

## Clone the repo

```bash
git clone git@<your-git-server>:<org>/homelab-config.git
cd homelab-config
```

## Restore a single file

The relative path in `configs/` mirrors the absolute path on the system.

```bash
# Examples
cp configs/etc/ssh/sshd_config /etc/ssh/sshd_config
cp configs/etc/docker/daemon.json /etc/docker/daemon.json
```

## Restore all files at once

```bash
cd homelab-config
find configs/ -type f | while read src; do
  dest="/${src#configs/}"
  mkdir -p "$(dirname "$dest")"
  cp "$src" "$dest"
  echo "Restored: $dest"
done
```

⚠️ Files containing `REDACTED` values must be updated manually with the real values (available in Vaultwarden).

## What is covered

See `manifest.md` in the `homelab-config` repo for the full list.

Key files: `sshd_config`, `docker/daemon.json`, `fstab`, `hosts`, `resolv.conf`, `sysctl.d/`, custom `systemd/system/` units, crontabs, APT sources.
