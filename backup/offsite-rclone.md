# Backup offsite — Borg → Hetzner via rclone

## Rôle

Synchronisation du repo Borg local vers Hetzner Storage Box, chiffrée via rclone crypt.

## Fonctionnement

```
/mnt/backup/borg  →  rclone crypt  →  Hetzner Storage Box (hetzner-crypt:borg)
```

- Sync incrémental : seuls les nouveaux blocs sont transférés
- Chiffrement côté client : Hetzner ne voit que des données chiffrées
- Clé de chiffrement rclone : stockée dans Vaultwarden

## Configuration rclone

Fichier : `~/.config/rclone/rclone.conf`

Deux remotes configurés :
- `hetzner` : remote SFTP vers la Storage Box
- `hetzner-crypt` : remote crypt par-dessus `hetzner`

## Commandes utiles

```bash
# Lancer une sync manuelle
rclone sync /mnt/backup/borg hetzner-crypt:borg --progress

# Vérifier l'état
rclone ls hetzner-crypt:borg
```
