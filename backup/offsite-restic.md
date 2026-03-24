# Backup offsite — Timeshift → Hetzner via Restic

## Rôle

Envoi des snapshots Timeshift vers Hetzner Storage Box via Restic. Restic ajoute déduplication et chiffrement natifs.

## Fonctionnement

```
/mnt/timeshift  →  Restic  →  Hetzner Storage Box (rclone/borg/)
```

- Chiffrement natif Restic (clé dans Vaultwarden)
- Rétention : `--keep-daily 7`
- Stockage actuel : ~9 Go sur Hetzner

## Commandes utiles

```bash
# Lister les snapshots
restic -r <repo> snapshots

# Vérifier l'intégrité
restic -r <repo> check
```
