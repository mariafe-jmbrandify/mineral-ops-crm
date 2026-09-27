# API Documentation

## Status

This describes the **target API** for the system's next stage (see `SYSTEM_ARCHITECTURE.md`, "Target architecture"). The current MVP has no server-side API — the browser writes directly to per-user key-value storage. This document exists so the API is designed deliberately before it's built, not reverse-engineered from the client afterward.

## Design principles

- **REST-style, resource-based.** Each module (Deals, Mailers, Campaigns, Assets, Documents, and the planned modules) is a resource collection with standard verbs.
- **Validated server-side.** Every write is validated against the schema in `DATABASE_SCHEMA.md` before it's persisted — the client is not trusted to enforce data integrity alone, unlike the current MVP.
- **Role-scoped.** Every request is evaluated against the calling user's role (`USER_ROLES.md`) — the API is the actual enforcement point for permissions, not just the UI.
- **Additive for AI.** The MCP tool layer (`modules/AI/MCP_INTEGRATION.md`) calls this same API rather than a separate AI-only backend, so an AI-proposed write and a human-made write go through identical validation.

## Base structure

```
GET    /api/deals                 list deals (filterable: stage, county, ddFlag, updatedAfter)
GET    /api/deals/:id             get one deal
POST   /api/deals                 create a deal
PATCH  /api/deals/:id             update a deal
DELETE /api/deals/:id             delete a deal (hard delete, logged)

GET    /api/mailers                list mailers (filterable: status, campaign, county)
GET    /api/mailers/:id
POST   /api/mailers
PATCH  /api/mailers/:id

GET    /api/campaigns
GET    /api/campaigns/:id
POST   /api/campaigns
PATCH  /api/campaigns/:id

GET    /api/assets
GET    /api/assets/:id
POST   /api/assets
PATCH  /api/assets/:id

GET    /api/documents
GET    /api/documents/:id
POST   /api/documents             metadata only; file upload is a separate endpoint
PATCH  /api/documents/:id
POST   /api/documents/:id/upload  attach/replace the source file
```

Planned modules (`Due-Diligence`, `Title-Review`, `Curative`, `Closing`, `Payments`, `Tasks`) follow the same pattern once built; `Due-Diligence` and `Title-Review` are largely read/derived endpoints rather than independent resources, per `DATABASE_SCHEMA.md`.

## Request/response conventions

- All request and response bodies are JSON.
- Timestamps are ISO 8601.
- List endpoints support `?limit=`, `?offset=`, and field-specific filters; results are paginated by default (the MVP's "load everything into memory" pattern doesn't carry forward once this is a shared, server-backed API).
- Write endpoints (`POST`/`PATCH`) that touch a field flagged as "requires confirmation" per a module's guardrails (e.g., a due-diligence flag, a payment status) return the change as `status: "proposed"` rather than applying it immediately, when the caller is the AI/MCP layer. Human-originated requests (from the authenticated UI session) apply directly, since the confirmation already happened in the UI.

## Example: proposing a due-diligence flag (AI-originated)

```
PATCH /api/deals/{id}
Authorization: Bearer <ai-agent-token, scoped to acting-on-behalf-of user>
{
  "ddFlag": "Red — Title/PSA issue",
  "_proposal": {
    "reason": "Grantee in 2019 deed does not appear as grantor in any later conveyance",
    "sourceDocumentId": "doc_8f2a1c"
  }
}

→ 202 Accepted
{
  "status": "proposed",
  "dealId": "...",
  "pendingChange": { "ddFlag": "Red — Title/PSA issue" },
  "requiresConfirmationBy": "any user with Deals write access"
}
```

## Authentication

Target state: per-user session tokens (not yet implemented in the MVP, which is single-account). AI agent calls are scoped as acting-on-behalf-of a specific user session, never as an independent elevated identity — see `modules/AI/MCP_INTEGRATION.md`, "Permissioning."

## Error handling

Standard HTTP status codes; validation errors return a structured body identifying the specific field and constraint that failed, so both the UI and the AI layer can surface a precise, actionable message rather than a generic failure.

## Versioning

Not yet applicable at MVP stage. Once this API is live, breaking changes will be versioned via URL prefix (`/api/v2/...`) rather than in-place changes to `/api/v1/...`.
