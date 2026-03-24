# Chaîne de récupération — Vaultwarden

Vaultwarden est le coffre central. Cette page décrit comment y accéder même en cas de panne totale.

## Principe

ProtonMail est le seul service accessible **sans** Vaultwarden grâce à :
- Une passphrase mémorisée / stockée hors ligne
- Un téléphone vérifié (accès de secours)

C'est le point d'entrée de toute la chaîne de récupération.

## Étape 1 — Accéder à ProtonMail sans Vaultwarden

- Via passphrase offline (mémorisée ou stockée physiquement hors du serveur)
- Ou via téléphone vérifié

## Étape 2 — Récupérer l'export Vaultwarden

Un export chiffré est envoyé chaque dimanche à 20h sur ProtonMail.

Chercher l'email le plus récent avec l'export `encrypted_json`.

## Étape 3 — Restaurer Vaultwarden

1. Déployer une instance Vaultwarden fraîche
2. Importer l'export chiffré (format `encrypted_json`)
3. Utiliser le mot de passe maître pour déchiffrer

## Étape 4 — Récupérer les secrets

Une fois Vaultwarden restauré :
- Clés SSH → reconnexion au serveur / Forgejo / Hetzner
- Clé rclone → accès au backup Borg sur Hetzner
- Clé Restic → accès aux snapshots Timeshift sur Hetzner
- 2FA OVH, Hetzner, Infisical, Forgejo → accès aux services externes

## ⚠️ Points critiques

- Ne jamais stocker les codes de récupération ProtonMail **uniquement** dans Vaultwarden
- Tester régulièrement l'accès ProtonMail sans Vaultwarden
- Vérifier que l'export hebdomadaire arrive bien (surveiller les emails du dimanche)
