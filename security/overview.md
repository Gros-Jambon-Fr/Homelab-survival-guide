# Sécurité — Vue d'ensemble

## Exposition réseau

- Aucun service exposé sur internet
- Tout accessible en LAN uniquement, ou via VPN WireGuard (Freebox Delta)
- Reverse proxy Traefik avec certificats Let's Encrypt (OVH DNS challenge)

## Accès au serveur

- SSH par clé uniquement (pas de mot de passe)
- Clé `id_ed25519` sauvegardée dans Vaultwarden
- Accès physique : serveur chez soi

## Gestion des secrets

- [Vaultwarden](secrets.md) — passwords, clés SSH, exports chiffrés
- [Infisical](secrets.md) — secrets applicatifs injectés au runtime dans les conteneurs
- Jamais de secrets en dur dans les fichiers de config versionnés

## Authentification forte

Voir [stratégie 2FA](2fa.md).

## Points de vigilance acceptés

| Service | Risque | Justification |
|---|---|---|
| Portainer | Pas de 2FA | LAN/VPN uniquement, risque acceptable |
| 2FA Forgejo/OVH/Hetzner dans Vaultwarden | Dépendance VW | Chaîne de récupération ProtonMail validée |
