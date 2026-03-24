# Restauration — Par où commencer ?

## Identifier le scénario

### Scénario 1 — Rollback après une mise à jour / mauvaise manip système
→ [Restauration Timeshift](system-restore.md#timeshift)

### Scénario 2 — Perte de données applicatives (Immich, Nextcloud, etc.)
→ [Restauration Borg](data-restore.md)

### Scénario 3 — Disque système mort
1. Réinstaller Debian
2. Restaurer les configs système depuis `homelab-config` → [Restauration configs](configs-restore.md)
3. Restaurer les données depuis Borg → [Restauration Borg](data-restore.md)
4. Remonter les conteneurs Docker depuis les compose

### Scénario 4 — Serveur complètement irrécupérable (vol, incendie, etc.)
1. Nouveau matériel + Réinstaller Debian
2. **Récupérer l'accès à Vaultwarden** → [Chaîne de récupération](vaultwarden-chain.md)
3. Récupérer les clés de chiffrement (rclone, Restic) depuis Vaultwarden
4. Restaurer depuis Hetzner (Borg via rclone / Timeshift via Restic)
5. Restaurer les configs système depuis Forgejo (`homelab-config`)
6. Remonter les conteneurs Docker

### Scénario 5 — Perte d'accès à Vaultwarden uniquement
→ [Chaîne de récupération Vaultwarden](vaultwarden-chain.md)

## Priorité de récupération

1. ProtonMail (point d'entrée de tout)
2. Vaultwarden (contient tous les autres secrets)
3. Infisical (secrets applicatifs)
4. Services Docker
