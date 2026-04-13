# Security — Overview

## Network exposure

- No service exposed to the internet
- Everything accessible on LAN only, or via WireGuard VPN
- Traefik reverse proxy with Let's Encrypt certificates (DNS challenge)

## Server access

- SSH by key only (no password authentication)
- SSH key backed up in Vaultwarden
- Physical access: server at home

## Secret management

- [Vaultwarden](secrets.md) — passwords, SSH keys, encrypted exports
- [Infisical](secrets.md) — application secrets injected at runtime into containers
- No secrets hardcoded in versioned config files

## Strong authentication

See [2FA strategy](2fa.md).

## Container hardening

All containers run with `cap_drop: ALL` — Linux capabilities are dropped by default and re-added only when strictly required (e.g. `SETUID`/`SETGID` for services that drop privileges, `NET_BIND_SERVICE` for services binding low ports). Containers using `privileged: true` (e.g. system monitoring agents) are excluded from this rule.

## Accepted risk points

| Service | Risk | Justification |
|---|---|---|
| Portainer | No 2FA | LAN/VPN only, acceptable risk |
| 2FA for Git/DNS/cloud providers stored in Vaultwarden | VW dependency | ProtonMail recovery chain validated and tested |
