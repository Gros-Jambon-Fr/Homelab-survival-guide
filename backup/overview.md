# Backup — Vue d'ensemble

## Stratégie 3-2-1

- **3** copies des données
- **2** supports différents
- **1** copie hors site

## Ce qui est sauvegardé

| Données | Outil | Local | Offsite |
|---|---|---|---|
| Snapshots système (OS + config) | Timeshift | ✅ `/mnt/timeshift` | ✅ Restic → Hetzner |
| Données applicatives Docker | Borg | ✅ `/mnt/backup` | ✅ rclone crypt → Hetzner |
| Configs système (fichiers hôte) | Git | ✅ Forgejo | — |
| Passwords / secrets | Vaultwarden | ✅ (conteneur) | ✅ export chiffré ProtonMail |

## Ce qui n'est PAS sauvegardé

- Les images Docker (reconstruites depuis les compose)
- Les fichiers temporaires et caches

## Fréquence

| Outil | Fréquence | Rétention |
|---|---|---|
| Timeshift | Quotidien (timer systemd) | 7 jours |
| Borg | À définir | 7 jours |
| Restic (Timeshift → Hetzner) | Après chaque snapshot | `--keep-daily 7` |
| rclone (Borg → Hetzner) | Après chaque backup Borg | Sync incrémental |
| Vaultwarden export | Hebdomadaire (dimanche 20h) | Email ProtonMail |

## Détail par outil

- [Timeshift](local-timeshift.md)
- [Borg](local-borg.md)
- [rclone → Hetzner](offsite-rclone.md)
- [Restic → Hetzner](offsite-restic.md)
