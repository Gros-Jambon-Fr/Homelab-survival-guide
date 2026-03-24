# Backup local — Borg

## Rôle

Sauvegarde des données applicatives Docker. Borg offre déduplication, compression et chiffrement natifs.

## Sources sauvegardées

- `/home/immich` — bibliothèque photos Immich
- `/home/matthieu` — données utilisateur
- `/home/Nextcloud` — données Nextcloud

## Configuration

- Destination : `/mnt/backup` (HDD Seagate 4 To dédié)
- Compression : lz4
- Rétention : 7 jours

## Commandes utiles

```bash
# Lister les archives
borg list /mnt/backup/borg

# Voir le contenu d'une archive
borg list /mnt/backup/borg::nom-archive

# Infos sur le repo
borg info /mnt/backup/borg
```
