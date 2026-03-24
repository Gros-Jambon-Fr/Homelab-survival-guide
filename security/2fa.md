# Stratégie 2FA

## Principe

Le 2FA est activé partout où c'est possible. Les codes de récupération sont stockés dans Vaultwarden **et** imprimés/stockés hors ligne.

## Services avec 2FA physique (indépendant de Vaultwarden)

Ces services sont accessibles même si Vaultwarden est indisponible :

| Service | Méthode |
|---|---|
| ProtonMail | Code de récupération offline + téléphone vérifié |
| Addy.io | Code de récupération offline |
| Vaultwarden | Code de récupération offline |

## Services avec 2FA dans Vaultwarden

| Service | Remarque |
|---|---|
| OVH | Accessible via ProtonMail → VW |
| Hetzner | Accessible via ProtonMail → VW |
| Infisical | Accessible via ProtonMail → VW |
| Forgejo | Accessible via ProtonMail → VW |

## Absence de 2FA

| Service | Raison |
|---|---|
| Portainer | Non supporté — LAN/VPN uniquement |

## Chaîne de récupération

ProtonMail est le point d'entrée de toute la chaîne. Voir [vaultwarden-chain.md](../restoration/vaultwarden-chain.md).
