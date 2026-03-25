# 2FA Strategy

## Principle

2FA is enabled everywhere possible. Recovery codes are stored in Vaultwarden **and** kept offline.

## Services with physical 2FA (independent from Vaultwarden)

These four services use a phone-based TOTP app (not Vaultwarden) and have offline recovery codes stored physically. They remain fully accessible even if Vaultwarden is unavailable.

| Service | TOTP | Offline fallback |
|---|---|---|
| ProtonMail | Phone (TOTP app) | Account passphrase (disables 2FA per Proton docs) |
| Email alias provider | Phone (TOTP app) | Recovery code (paper) |
| Vaultwarden | Phone (TOTP app) | Recovery code (paper) |
| Cloud/VPS provider | Phone (TOTP app) | Recovery code (paper) |

## Services with 2FA stored in Vaultwarden

| Service | Note |
|---|---|
| DNS provider | Accessible via ProtonMail → VW |
| Cloud/VPS provider | Accessible via ProtonMail → VW |
| Secrets manager | Accessible via ProtonMail → VW |
| Self-hosted Git | Accessible via ProtonMail → VW |

## No 2FA

| Service | Reason |
|---|---|
| Portainer | Not supported — LAN/VPN only |

## Recovery chain

ProtonMail is the entry point for the entire chain. See [vaultwarden-chain.md](../restoration/vaultwarden-chain.md).
