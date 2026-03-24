# Homelab Survival Guide

Documentation de ma stratégie de résilience pour mon homelab : backup, sécurité, et procédures de restauration.

## Principe général — Stratégie 3-2-1

| Copie | Emplacement | Technologie |
|---|---|---|
| Production | Disques internes (SSD + HDD) | — |
| Backup local | Disques internes dédiés | Timeshift + Borg |
| Backup offsite | Hetzner Storage Box (Allemagne) | rclone crypt + Restic |

## Structure

```
backup/         → Ce qui est sauvegardé, où, et comment
security/       → Accès, secrets, 2FA
restoration/    → Procédures de restauration par scénario
scripts/        → Scripts utilitaires (à venir)
```

## Infrastructure

- Serveur : HP EliteDesk 800 G2 SFF — Intel i5-6500, 16 Go RAM, Debian 13
- ~40 conteneurs Docker
- Accès distant : VPN WireGuard
- Reverse proxy : Traefik

## Navigation rapide

- [Vue d'ensemble backup](backup/overview.md)
- [Par où commencer en cas de panne](restoration/overview.md)
- [Chaîne de récupération des secrets](restoration/vaultwarden-chain.md)
