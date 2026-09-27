# Business Workflows

The real-world acquisition process this system supports, end to end. Each stage below maps to a module in this repo.

## 1. Campaign planning and mailer generation

- A campaign is planned for a set of counties/coverage area (`Campaigns` module).
- A mailer list is pulled or refreshed for that coverage area (`Mailers` module), including owner name, address, decimal interest, and a computed offer based on price-per-NRA for the area.
- Mailers ship on a target date; status moves from `Not Sent` → `Sent`.

## 2. Owner response and initial triage

- Responses come back three ways: the owner mails back a signed PSA, the owner calls/emails with questions, or the mail is returned undeliverable (RTS).
- Signed PSAs and engaged owners create or advance a `Deal` record, moving it from `Prospecting`/`Mailer Sent` into `Negotiating` or `PSA Sent`.
- RTS mail updates the `Mailer` status to `Returned (RTS)` and flags the owner for address research before re-mailing.
- Non-responders past a set window get flagged for follow-up (`AI_EMAIL_ASSISTANT.md` drafts the follow-up; a person sends it).

## 3. PSA and due diligence

- Once a PSA is out or signed, the deal moves into `Due Diligence`.
- Title documents, deeds, and any probate/AOH records are gathered and linked to the deal (`Documents` module).
- The due-diligence flag is set based on what's found:
  - **Red** — a title or PSA issue needs resolution before proceeding.
  - **Yellow** — a royalty or title item needs follow-up but isn't blocking.
  - **Blue** — an Affidavit of Heirship is needed (owner is deceased, no probate on file).
- See `Due-Diligence.md` and `Title-Review.md` for how this is tracked in detail.

## 4. Curative work

- Any issue found in due diligence that needs to be resolved before closing becomes a curative item — commonly probate/AOH work, or clearing up a gap in the chain of title.
- Curative items are tracked per deal until resolved, at which point the deal can move to `Closing`.
- See `Curative.md`.

## 5. Closing

- Once curative is clear, the deal moves to `Closing`: PSA finalized, deed prepared, wire instructions verified.
- **Important standing practice:** wire instructions are always verified by phone call before funds move, regardless of how confident the paperwork looks — this is a hard rule, not a suggestion.
- Deed is recorded, and the deal moves to `Closed`.
- See `Closing.md`.

## 6. Payment

- Payment (typically wire) is issued once closing is confirmed.
- Payment status is tracked against the deal so "closed but not yet paid" is visible, not assumed.
- See `Payments.md`.

## 7. Invoicing and profit tracking (campaign level)

- Campaign costs (mailer production, postage) and resulting deal value roll up to a per-campaign profit figure.
- Invoices related to a campaign are tracked in `Documents` and summarized in `Reports`.

## 8. Reporting

- Pipeline, campaign performance, and closed-deal recaps are generated on a schedule or on demand — see `Reports.md` and `modules/AI/AI_REPORTS.md`.

## Roles in this process

| Stage | Primary owner |
|---|---|
| Campaign planning, mailer generation | Campaign lead / team |
| Owner response, triage | Whoever's working the pipeline that day |
| Due diligence, title, curative | Title/curative lead |
| Closing approval | Owner |
| Payment | Owner / accounting |

## Standing rules that apply across every stage

- No email or SMS goes out without a human review and explicit send action.
- Last Contact Date only updates on a confirmed send, never on a draft.
- Wire instructions are always verified by phone before funds move.
- Anything requiring the owner's approval gets a one-sentence outcome summary first — not a request to read the full history before deciding.
