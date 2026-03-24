# Recovery Chain — Vaultwarden

Vaultwarden is the central vault. This page describes how to regain access even in the event of a total failure.

## Principle

ProtonMail is the only service accessible **without** Vaultwarden, thanks to:
- An offline passphrase (memorized or stored physically off-server)
- A verified phone (backup access)

It is the entry point for the entire recovery chain.

## Step 1 — Access ProtonMail without Vaultwarden

- Via offline passphrase (memorized or stored physically)
- Or via verified phone

## Step 2 — Retrieve the Vaultwarden export

An encrypted export is sent every Sunday at 8pm to ProtonMail.

Search for the most recent email with the `encrypted_json` export attached.

## Step 3 — Restore Vaultwarden

1. Deploy a fresh Vaultwarden instance
2. Import the encrypted export (`encrypted_json` format)
3. Use the master password to decrypt

## Step 4 — Retrieve all secrets

Once Vaultwarden is restored:
- SSH keys → reconnect to server / Git / cloud providers
- rclone key → access Borg backup on Hetzner
- Restic key → access Timeshift snapshots on Hetzner
- 2FA for DNS, cloud, Git providers → access to external services

## ⚠️ Critical points

- Never store ProtonMail recovery codes **only** in Vaultwarden
- Regularly test ProtonMail access without Vaultwarden
- Verify that the weekly export email is arriving (check every Sunday)
