# Project Overview

## What this is

A CRM built for a Texas oil & gas mineral rights acquisition company: tracking deals from first mailer to closed acquisition, managing the mailer lists and campaigns that generate those deals, and keeping the assets and documents tied to each deal in one searchable place.

## Who it's for

- **Owner** — needs pipeline visibility and outcome summaries without digging through spreadsheet tabs; prefers manual-before-automation and one-sentence outcome summaries before approving anything.
- **Title/curative lead** — chain-of-title and curative work.
- **Campaign lead** and other team members — shared campaign ownership, deal work.
- **Maria (JM Brandify)** — builds and maintains the system, runs the automation layer underneath it.

## The problem this solves

Before this system, the business ran on a set of spreadsheet tabs: a live deals tracker, a separate closed-deals tracker, an active mailer list, a returned-to-sender list, a dead-mailer list, an archived mailer list, a campaign tracker, and an invoice tracker — each maintained separately, with no cross-linking. Answering "what's our current pipeline value" or "which deals need attention" meant manually cross-referencing multiple tabs. Nothing was flagged automatically; a deal's due-diligence status depended on someone remembering to note it in the right column.

## What the system does

1. **Consolidates** deals, mailer lists, campaigns, assets, and documents into one data model, so a deal, its mailer history, its linked documents, and its asset details are all reachable from one record instead of five.
2. **Preserves the existing workflow logic** — deal stages and the red/yellow/blue due-diligence flag system are carried over from the spreadsheet process, not reinvented, so the transition doesn't require the team to learn a new mental model.
3. **Surfaces what needs attention** — flagged deals and upcoming campaign touches show up on a dashboard instead of requiring someone to go looking for them.
4. **Adds AI assistance without removing human control** — drafting, extraction, and flagging are AI-assisted, but every send, every financial change, and every flag confirmation stays a human decision. See `modules/AI/README.md` for the full design principle.

## Scope boundaries

- This is a CRM and workflow tool for the acquisition side of the business, not a full accounting or land management system. Payments and financial reconciliation live in the `Payments.md` module at a tracking level, not as a general ledger replacement.
- The AI layer assists with drafting, extraction, and flagging — it does not replace legal review of title work or an attorney's sign-off on closing documents.

## Where to go next

- `PRODUCT_ARCHITECTURE.md` — how the modules fit together and how a user moves through them
- `SYSTEM_ARCHITECTURE.md` — the technical stack and data flow
- `DATABASE_SCHEMA.md` — the actual fields on every record type
- `BUSINESS_WORKFLOWS.md` — the real acquisition process this system supports, end to end
