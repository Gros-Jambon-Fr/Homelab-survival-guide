# Restauration des données — Borg

## Depuis le backup local (`/mnt/backup`)

```bash
# Lister les archives disponibles
borg list /mnt/backup/borg

# Lister le contenu d'une archive
borg list /mnt/backup/borg::<nom-archive>

# Restaurer une archive complète
cd /
borg extract /mnt/backup/borg::<nom-archive>

# Restaurer un chemin spécifique
borg extract /mnt/backup/borg::<nom-archive> home/immich/
```

## Depuis Hetzner (backup offsite)

Si `/mnt/backup` est perdu, récupérer via rclone depuis Hetzner puis restaurer Borg.

```bash
# Synchroniser le repo Borg depuis Hetzner vers un disque local
rclone sync hetzner-crypt:borg /mnt/backup/borg --progress

# Puis restaurer comme ci-dessus
borg list /mnt/backup/borg
```

⚠️ La clé de chiffrement rclone est dans Vaultwarden.

## Sources sauvegardées

- `/home/immich`
- `/home/matthieu`
- `/home/Nextcloud`
