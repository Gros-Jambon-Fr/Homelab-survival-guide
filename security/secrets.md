# Gestion des secrets

## Vaultwarden (passwords & secrets personnels)

Instance auto-hébergée. Stocke :
- Passwords de tous les services
- Clés SSH
- Clés de chiffrement (rclone, Restic)
- Codes 2FA de récupération
- Export chiffré hebdomadaire envoyé par email (ProtonMail)

⚠️ Point critique : si Vaultwarden est inaccessible, voir la [chaîne de récupération](../restoration/vaultwarden-chain.md).

## Infisical (secrets applicatifs)

Secrets injectés au runtime dans les conteneurs Docker via machine identity.
Utilisé dans les workflows CI/CD Forgejo Actions.

Variables disponibles dans les workflows :
- `INFISICAL_CLIENT_ID`
- `INFISICAL_CLIENT_SECRET`
- `INFISICAL_PROJECT_ID`
- `INFISICAL_DOMAIN`

## Règles

- Jamais de secret en dur dans un fichier versionné
- Jamais de secret partagé dans le chat ou par email en clair
- Les configs système versionnées dans `homelab-config` ont les valeurs sensibles remplacées par `REDACTED`
