# Backup local — Timeshift

## Rôle

Snapshots du système (OS, configs, paquets). Permet un rollback rapide après une mise à jour ou une mauvaise manipulation système.

Ne sauvegarde **pas** les données utilisateur (géré par Borg).

## Configuration

- Destination : `/mnt/timeshift` (SSD SanDisk 240 Go dédié)
- Type : RSYNC
- Rétention : 7 snapshots quotidiens
- Déclenchement : timer systemd custom (`timeshift-daily.timer`)

## Déclenchement manuel

```bash
sudo timeshift --create --comments "avant mise à jour" --tags D
```

## Lister les snapshots disponibles

```bash
sudo timeshift --list
```
