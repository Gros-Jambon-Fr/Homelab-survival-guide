# Restauration des configs système

Les configs système sont versionnées dans le repo `homelab-config` sur Forgejo.

## Cloner le repo

```bash
git clone git@forgejo.example.com:Homelab/homelab-config.git
cd homelab-config
```

## Restaurer un fichier

Le chemin relatif dans `configs/` correspond au chemin absolu sur le système.

```bash
# Exemple
cp configs/etc/ssh/sshd_config /etc/ssh/sshd_config
cp configs/etc/docker/daemon.json /etc/docker/daemon.json
```

## Restaurer tous les fichiers d'un coup

```bash
cd homelab-config
find configs/ -type f | while read src; do
  dest="/${src#configs/}"
  mkdir -p "$(dirname "$dest")"
  cp "$src" "$dest"
  echo "Restauré : $dest"
done
```

⚠️ Les valeurs `REDACTED` dans les fichiers copiés doivent être remplacées manuellement par les vraies valeurs (disponibles dans Vaultwarden).

## Fichiers couverts

Voir `manifest.md` dans le repo `homelab-config` pour la liste complète.

Principaux : `sshd_config`, `docker/daemon.json`, `fstab`, `hosts`, `resolv.conf`, `sysctl.d/`, `systemd/system/` (services custom), `stunnel/`, `timeshift/`, crontabs, sources APT.
