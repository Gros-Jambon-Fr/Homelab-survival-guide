# Secret Management

## Vaultwarden (passwords & personal secrets)

Self-hosted instance. Stores:
- Passwords for all services
- SSH keys
- Encryption keys (rclone, Restic)
- 2FA recovery codes
- Weekly encrypted export sent by email

⚠️ Critical point: if Vaultwarden is unavailable, see the [recovery chain](../restoration/vaultwarden-chain.md).

## Infisical (application secrets)

Secrets injected at runtime into Docker containers via machine identity.
Used in CI/CD workflows.

### Bootstrap exceptions

Two stacks cannot use Infisical due to circular dependencies — they use a local `.env` file instead (`chmod 600`, outside of Git):

- **Infisical itself** — cannot pull its own secrets from itself
- **Traefik** — if Traefik is down, Infisical is unreachable; the OVH DNS credentials needed to restart Traefik would be inaccessible if stored in Infisical

These are the only two exceptions. All other stacks use Infisical exclusively.

## Rules

- Never hardcode secrets in versioned files
- Never share secrets in chat or plain-text email
- System config files versioned in the configs repo have sensitive values replaced with `REDACTED`
