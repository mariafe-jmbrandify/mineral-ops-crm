# Deployment

## Current state (MVP)

The MVP is a single static HTML file (`index.html`) with no build step and no server. "Deployment" today means:

1. The file is opened directly (locally, or hosted as a static file).
2. On first load, it checks its private per-user storage; if empty, it seeds itself once from the embedded import data (see `CHANGELOG.md`, v0.1.0).
3. All subsequent reads/writes go through the `window.storage` API — there is no separate backend to deploy or maintain.

There is no CI/CD pipeline at this stage because there's no build process to run.

## Target deployment model

Once the system moves to the API + database architecture described in `SYSTEM_ARCHITECTURE.md`, deployment involves three components:

```
Frontend (static build) ──▶ CDN / static hosting
API layer               ──▶ Application server (containerized)
Database                ──▶ Managed relational database
```

### Environments

| Environment | Purpose |
|---|---|
| Local | Individual development |
| Staging | Pre-release testing with anonymized/sample data — never real owner PII |
| Production | Live system, real deal and owner data |

### Release process (target)

1. Changes merged to `main` trigger a staging deploy automatically.
2. Manual verification against `TESTING.md` checklist on staging.
3. Production deploy is a manual promotion step, not automatic — given this handles real owner PII and deal financials, an unreviewed auto-deploy to production is treated as a non-negotiable risk, not just a nice-to-have gate.
4. `CHANGELOG.md` is updated as part of the release, not after the fact.

### Rollback

Target state keeps the previous production build available for immediate rollback; database migrations (once introduced) are written to be reversible where possible, and irreversible migrations require a documented backup step immediately before running.

## Secrets and configuration

- API keys (Anthropic API, any integration credentials for n8n/Make.com/Google Apps Script) are never committed to this repo.
- Target state: environment-specific secrets managed via the hosting platform's secret manager, not `.env` files checked into version control.

## Data migration from spreadsheets

The one-time import described in `CHANGELOG.md` (v0.1.0) is the model for any future one-time migrations: run once, guarded so it can't silently re-run and overwrite manual edits, and logged with exactly what was imported from where. Any future migration (e.g., MVP storage → production database) should follow the same pattern: idempotent, guarded, and logged.

## Related

- `SYSTEM_ARCHITECTURE.md` — the architecture being deployed
- `SECURITY.md` — security considerations that apply across all environments
- `TESTING.md` — what's verified before a production release
