# Roadmap

## Guiding approach

Built and rolled out incrementally, in the order that removes the most manual work first — not necessarily the order that looks most impressive. Each phase should be usable on its own before the next one starts.

## Phase 1 — Core CRM (shipped)

- Dashboard, Deals, Mailer Lists, Campaigns, Assets, Documents modules
- Full data import from existing spreadsheet trackers (~15,600 records across all modules)
- Auto-save, single-user private storage
- Deal stage and due-diligence flag mapping carried over from the existing manual workflow

## Phase 2 — Documentation and target architecture (shipped)

- Full `docs/` set defining the target system, API, data model, security, and testing approach
- `modules/` documentation for every planned module (Due-Diligence, Title-Review, Curative, Closing, Payments, Reports, Tasks)
- `modules/AI/` documentation for the full AI layer

## Phase 3 — AI layer, first features

Priority order, based on what already has a working precedent to build from:

1. **Document Analysis** — extends the existing Pipeline B invoice OCR pattern to PSAs, deeds, and title opinions.
2. **Email Assistant** — formalizes the existing `ClaudeDrafting.gs` workflow inside the CRM, same no-auto-send rule carried over unchanged.
3. **Due Diligence flag proposals** — builds on Document Analysis output; first version proposes flags for human confirmation only.
4. **AI Assistant (chat)** — read-first (Q&A across modules), write proposals added once read behavior is solid.

## Phase 4 — Multi-user and API layer

- Move from per-user client-side storage to a shared database behind a validated API (`SYSTEM_ARCHITECTURE.md`, `API_DOCUMENTATION.md`)
- Role-based access per `USER_ROLES.md`, onboarding the owner, title lead, and campaign lead as actual system users rather than spreadsheet/summary recipients
- Formal `DEPLOYMENT.md` pipeline (staging → production) replaces the current single-file distribution

## Phase 5 — Remaining workflow modules

- Due-Diligence, Title-Review, Curative, Closing, and Payments move from documented to built, in that order — each depends on the one before it being usable (curative work needs title-review output; closing needs curative resolved; payments follow closing)
- Tasks module, once there's enough cross-module activity that ad hoc follow-ups need a home outside notes fields

## Phase 6 — Deal Analyst, Title Analyst, Reports, Search, Automation

- Deal scoring and price comparables (`AI_DEAL_ANALYST.md`)
- Chain-of-title assembly (`AI_TITLE_ANALYST.md`) — highest-value once Title-Review module exists to house it
- Scheduled reporting (`AI_REPORTS.md`)
- Semantic search across the full dataset (`AI_SEARCH.md`) — most valuable once mailer volume and document count make keyword search clearly insufficient (already true today at 11,000+ mailer records)
- Full workflow orchestration (`AI_AUTOMATION.md`) replacing the standalone Apps Script/n8n scripts with CRM-native automation

## Explicitly not on the roadmap

- General accounting/bookkeeping (`Payments.md` stays a status tracker, not a ledger replacement)
- Replacing legal review of title work or attorney sign-off on closing
- Public-facing or owner-facing portal (this is an internal tool)

## How this roadmap changes

Priorities shift based on what's actually slowing the team down week to week — this order reflects what's true as of the current phase, not a fixed commitment. Updates to this roadmap should be logged in `CHANGELOG.md`.
