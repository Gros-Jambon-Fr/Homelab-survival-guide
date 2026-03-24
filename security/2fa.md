# 2FA Strategy

## Principle

2FA is enabled everywhere possible. Recovery codes are stored in Vaultwarden **and** kept offline.

## Services with physical 2FA (independent from Vaultwarden)

These services remain accessible even if Vaultwarden is unavailable:

| Service | Method |
|---|---|
| ProtonMail | Offline recovery code + verified phone |
| Email alias provider | Offline recovery code |
| Vaultwarden | Offline recovery code |

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
