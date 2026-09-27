# System Architecture

## Current implementation (MVP)

The MVP is a single-file web application (HTML/CSS/JS) with client-side rendering and a key-value persistence layer:

```
┌─────────────────────────────────────────────┐
│              Browser (client)                │
│  ┌─────────────────────────────────────┐    │
│  │  UI layer (vanilla JS, no framework) │    │
│  │  — table rendering, filters, modals  │    │
│  └───────────────┬───────────────────────┘   │
│                  │                            │
│  ┌───────────────▼───────────────────────┐   │
│  │  State layer (in-memory JS object)     │   │
│  │  { deals, mailers, campaigns,          │   │
│  │    assets, documents }                 │   │
│  └───────────────┬───────────────────────┘   │
│                  │                            │
│  ┌───────────────▼───────────────────────┐   │
│  │  Storage layer (window.storage API)    │   │
│  │  — one key per module, JSON-encoded    │   │
│  │  — personal (per-user), not shared     │   │
│  └─────────────────────────────────────────┘  │
└─────────────────────────────────────────────┘
```

This is intentionally simple: no server to run, no database to provision, works entirely client-side. It's a good fit for a single-user MVP but has real limits — see "Scaling path" below.

## Target architecture (as the system grows)

```
┌──────────────┐     ┌───────────────────┐     ┌──────────────────┐
│   Frontend    │────▶│   API layer        │────▶│   Database        │
│  (web app)    │◀────│  (validated writes,│◀────│  (structured,     │
│               │     │   role-scoped)     │     │   relational)     │
└──────────────┘     └─────────┬──────────┘     └──────────────────┘
                                │
                     ┌──────────▼──────────┐
                     │   AI / MCP layer     │
                     │  (see modules/AI/)   │
                     └──────────┬──────────┘
                                │
                ┌───────────────┼───────────────┐
                ▼               ▼               ▼
        ┌───────────┐   ┌─────────────┐  ┌─────────────┐
        │  Google    │   │  n8n /      │  │  Anthropic   │
        │  Drive/    │   │  Make.com   │  │  API         │
        │  Apps      │   │  (workflow  │  │  (Claude)    │
        │  Script    │   │  automation)│  │              │
        └───────────┘   └─────────────┘  └─────────────┘
```

Key shift from MVP: a real API layer between the frontend and the data store, so writes are validated server-side (not just in client JS), multiple users can work concurrently without last-write-wins conflicts, and the AI/MCP layer talks to the API rather than directly to client-side storage.

## Data flow: a document upload, end to end (target state)

1. User uploads a document (or it syncs from Google Drive) via the Documents module.
2. API layer stores the file and creates a Document record linked to the relevant Deal/Owner.
3. Document Analysis (AI layer) is triggered, extracts structured fields, and proposes them back onto the record.
4. If the document is a title opinion, Title Analyst is triggered to update the chain-of-title view.
5. If new information changes the due-diligence picture, a flag is proposed on the linked Deal.
6. All of the above are visible immediately in the UI, tagged as AI-proposed until confirmed.

## Scaling path from MVP to target

| Concern | MVP (current) | Target |
|---|---|---|
| Data store | Per-user key-value storage, client-side | Shared relational database |
| Concurrency | Single user, no conflict handling | Multi-user, API-validated writes |
| AI access to data | N/A (not yet implemented) | Scoped MCP tools against the API, not direct storage access |
| File storage | N/A (metadata only in MVP) | Google Drive, referenced by Document records |
| Automation | External scripts (Apps Script, n8n) writing to spreadsheets | Same tools, writing to the CRM API instead |
| Auth | Single account | Role-based access per user (owner, title lead, campaign lead, etc.) |

## Integration points

- **Google Apps Script** — existing RRC spacing verification and email/SMS drafting/send workflows; target state has these read/write against the CRM API instead of the spreadsheet.
- **n8n / Make.com** — campaign scheduling and other multi-step automations; same integration pattern as the existing job-matching automation.
- **Anthropic API (Claude)** — all AI reasoning tasks; see `modules/AI/LLM_ARCHITECTURE.md`.
- **Google Drive** — document storage and OCR source.

## Non-goals

- Not building a general-purpose accounting system — payment tracking is scoped to what's needed for deal/campaign visibility, not full bookkeeping.
- Not replacing the Texas Railroad Commission's public system — the RRC integration queries it, it doesn't mirror or replace it.
