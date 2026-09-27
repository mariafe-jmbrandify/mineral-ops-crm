# Product Architecture

## Module map

```
Dashboard
   │
   ├── Deals ──────────────┬── Due-Diligence
   │                       ├── Title-Review
   │                       ├── Curative
   │                       ├── Closing
   │                       └── Payments
   │
   ├── Mailers ──────────── Campaigns
   │
   ├── Assets ──────────────── (linked from Deals + Title-Review)
   │
   ├── Documents ──────────── (linked from every module above)
   │
   ├── Reports ──────────── (reads across all modules)
   │
   ├── Tasks ──────────────── (assignable follow-ups, linked to any record)
   │
   └── AI/ (see modules/AI/README.md — cross-cutting layer, not a single screen)
```

## Navigation model

Left sidebar, one item per primary module (Dashboard, Deals, Mailer Lists, Campaigns, Assets, Documents — the built MVP; Due-Diligence, Title-Review, Curative, Closing, Payments, Reports, Tasks as the system grows). Each module is a table-first view: search, status filter, add/edit via modal, no full-page navigation required for record-level edits. This keeps the interaction model consistent across modules — a user who's learned Deals already knows how Assets works.

## Core UI patterns

- **Table + filter bar** — every module's primary view is a searchable, filterable table, not a kanban or calendar-first view. This matches how the team already works (scanning rows in a spreadsheet) rather than forcing a new mental model.
- **Modal edit, not full-page edit** — adding or editing a record opens a modal over the current table, so context (search term, filter, scroll position) isn't lost.
- **Status pills, not free text** — stage and status fields are constrained to a fixed set of values shown as color-coded pills, so filtering and reporting stay reliable (no "Closed" vs "closed" vs "CLOSED" drift).
- **Dashboard as a surface, not a destination** — the dashboard exists to answer "what needs attention today," pulling from every module, rather than being a separate reporting tool a user has to remember to check.

## Record relationships

- A **Deal** links to one **Asset** (the lease/unit), one or more **Mailer** records (the outreach history that produced it), and any number of **Documents** (PSA, deed, title opinion).
- A **Mailer** record links to the **Campaign** touch that generated it.
- A **Document** always links back to the Deal or Owner it belongs to — no orphaned uploads.
- **Due-Diligence**, **Title-Review**, and **Curative** are sub-views scoped to a Deal's linked documents and asset — they don't hold independent records, they're structured views over the Deal + Document + Asset data.
- **Tasks** can attach to any record type, for follow-ups that don't belong to a single module (e.g., "call owner back Thursday" on a Deal, or "verify RTS address" on a Mailer record).

## Cross-cutting layers

- **AI layer** (`modules/AI/`) — available from every module via the assistant panel, plus module-specific AI features (extraction on Documents, flagging on Due-Diligence, drafting on Mailers/Deals).
- **Automation layer** — background workflows (campaign scheduling, RRC verification, invoice ingestion) that write into the same modules a human would edit directly; see `modules/AI/AI_AUTOMATION.md`.

## Roles

The MVP is single-user (Maria's account). As the team scales into the tool, role-based access is expected to follow the existing informal division of labor: the owner (visibility, approvals), a title lead (title/curative focus), and a campaign lead (campaign/mailer focus) — each scoped to see everything but write primarily within their area, rather than hard module locks.
