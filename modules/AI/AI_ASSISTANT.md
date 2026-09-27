# AI Assistant

## Overview

A conversational assistant available from every screen in the CRM (side panel, toggle open/closed). It answers questions about the data currently in view, performs lookups across modules, and — with confirmation — makes edits. It is the general-purpose entry point; specialized tasks (title review, document extraction) hand off to the dedicated modules described elsewhere in this folder.

## Why it matters

Not every question a team member has maps to a saved filter or report. "Which Harrison County deals haven't been touched in 30 days?" or "What's our total exposure on deals still in Due Diligence?" are one-off questions today that require manually scanning tables. The assistant answers these directly, in seconds, without leaving the page.

## How it works

1. User opens the chat panel and types a question or request in plain language.
2. The request, plus lightweight context (current module, current filters, current record if one is open), is sent to the LLM along with a set of read/write tools scoped to the CRM data (see `MCP_INTEGRATION.md`).
3. The model decides whether it can answer from a read-only query, or whether the user is asking for a change.
4. **Read requests** (lookups, counts, summaries) are answered directly, with the underlying records linked so the user can jump straight to them.
5. **Write requests** (change a stage, update a flag, add a note) are staged as a proposed change and shown as a diff — nothing is written until the user clicks Confirm.

## Example interactions

- "Show me every deal in Fayette County over $50k that's still Prospecting." → filtered table + summary.
- "Move the Sheppard deal to Closing and clear the red flag." → proposed change card, requires confirm.
- "Summarize what's changed on the Broadus deal this week." → pulls from record history/notes.
- "Draft a follow-up to owners we haven't heard back from in 3+ weeks." → hands off to `AI_EMAIL_ASSISTANT.md`.

## Inputs

- Free-text user message
- Current UI context (module, filters, open record)
- Read access to Deals, Mailers, Campaigns, Assets, Documents

## Outputs

- Natural-language answer, optionally with linked records/tables
- Proposed write operations (never auto-applied)

## Guardrails

- No destructive action (delete, bulk stage change, send) executes without an explicit human confirmation click — consistent with the no-auto-send rule used elsewhere in the system.
- The assistant states its confidence and cites which records it used, so answers aren't presented as unqualified fact when the underlying data is thin or ambiguous.
- Scope is limited to the CRM's own data; it does not browse the open web unless a future version explicitly adds that tool.

## Tech stack

- Claude (chat + tool use) via the Anthropic API
- MCP tool layer scoped per-module (read/write permissions match user's CRM role)
- Session-scoped conversation history; no persistent chat log tied to records unless explicitly saved as a note

## Future enhancements

- Voice input for on-the-go use
- Proactive nudges ("3 deals have had no contact in 30+ days — want me to draft follow-ups?") surfaced on the dashboard rather than only on request
