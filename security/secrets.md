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

## Rules

- Never hardcode secrets in versioned files
- Never share secrets in chat or plain-text email
- System config files versioned in the configs repo have sensitive values replaced with `REDACTED`
