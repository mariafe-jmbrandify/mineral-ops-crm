# FAQ

## General

**What is this system?**
A CRM built for a Texas oil & gas mineral rights acquisition company — deals, mailer lists, campaigns, assets, and documents in one place, with AI assistance layered on top. See `PROJECT_OVERVIEW.md`.

**Does this replace the spreadsheets?**
Yes, for day-to-day pipeline work. The spreadsheets were the source for the initial import (~15,600 records) but are not the system of record going forward — the CRM is.

**Is my data safe / who can see it?**
In the current MVP, data is private to the account that imported it — not shared with anyone else by default. See `SECURITY.md` for the full picture, including what changes once the system goes multi-user.

## Using the system

**I made an edit and it disappeared — what happened?**
Edits save automatically and instantly; there's no separate "save" step to forget. If something looks missing, check the search/filter state first — a filtered view can make a record look gone when it's just filtered out.

**Why didn't my data get re-imported when I reopened the file?**
The import is intentionally one-time (see `CHANGELOG.md`, v0.1.0) — it's guarded so reopening the app doesn't silently overwrite edits made since the import. If you genuinely need to re-import, that requires a manual step, not automatic behavior.

**Can I delete a record?**
Yes, with a confirmation prompt — it's a hard delete in the MVP, no undo. For a deal that's not proceeding, consider using the `Dead` stage instead of deleting, so the record (and the history of why it didn't work out) is preserved.

## AI features

**Will the AI ever send an email or text without me approving it?**
No. Every draft goes to a review queue; sending is always a separate, human-only action. This is a hard rule across the system, not a setting that can be turned off. See `modules/AI/AI_EMAIL_ASSISTANT.md`.

**Can the AI change a deal's stage or flag on its own?**
No — it proposes a change, and a person confirms it before it applies. See `modules/AI/MCP_INTEGRATION.md` for how this is enforced at the tool level, not just described in a prompt.

**What if the AI gets something wrong — a bad extraction, a wrong flag?**
Every AI output is either clearly marked as a proposal awaiting confirmation, or (for read-only answers) cites the specific records it used, so it can be checked. Nothing is presented as fact without a way to verify it against the source.

**Does the AI have access to information outside the CRM?**
No — AI features are scoped to the CRM's own data via the MCP tool layer. No feature browses the open web with CRM data in its context unless a future version explicitly adds and documents that capability.

## For the team

**Who do I ask if I'm not sure who owns a workflow step?**
See `USER_ROLES.md` for the current division of labor (owner, title lead, campaign lead, systems admin) and `BUSINESS_WORKFLOWS.md` for how a deal moves through each stage.

**What's the standing rule I most need to remember?**
Wire instructions are always verified by phone call before funds move — no exception, regardless of how complete the paperwork looks. See `Closing.md`.

## For developers

**Where do I start reading this repo?**
`PROJECT_OVERVIEW.md` → `SYSTEM_ARCHITECTURE.md` → `DATABASE_SCHEMA.md`, then the specific `modules/*.md` file for whatever you're working on.

**Is there a live API yet?**
Not yet — the MVP is fully client-side. `API_DOCUMENTATION.md` describes the target API design for when a server layer is introduced.

**How do I know what's built vs. just documented?**
`ROADMAP.md` tracks phase status. As a shortcut: Dashboard, Deals, Mailer Lists, Campaigns, Assets, and Documents are built; everything else in `modules/` is marked *(planned)*.
