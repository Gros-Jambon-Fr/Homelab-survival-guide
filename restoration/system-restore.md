# Restauration système

## Timeshift — Rollback rapide

Idéal pour annuler une mise à jour ou une mauvaise manipulation.

```bash
# Lister les snapshots disponibles
sudo timeshift --list

# Restaurer un snapshot (remplacer <nom> par le nom du snapshot)
sudo timeshift --restore --snapshot <nom>
```

Le système redémarre automatiquement après la restauration.

## Restic — Restauration depuis Hetzner

Si le disque local `/mnt/timeshift` est perdu, restaurer depuis Hetzner.

```bash
# Lister les snapshots disponibles
restic -r <repo-hetzner> snapshots

# Restaurer vers un répertoire cible
restic -r <repo-hetzner> restore <snapshot-id> --target /
```

⚠️ La clé de chiffrement Restic est dans Vaultwarden.
