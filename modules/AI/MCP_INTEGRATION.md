# MCP Integration

## Overview

Defines the tools exposed to LLM agents (the AI Assistant, and any future agent) via the Model Context Protocol — what the model is allowed to read, what it's allowed to write, and what stays entirely out of its reach. This is the enforcement layer behind the guardrails described in every other `AI_*.md` file: those guardrails are only real if they're implemented here, not just described in prompts.

## Why MCP

Rather than giving the model a general-purpose database connection, MCP tools are scoped narrowly — one tool per capability, each with its own input schema and permission boundary. The model can only do what a specific tool lets it do; it can't write arbitrary queries against the underlying data.

## Tool categories

### Read tools
| Tool | Scope |
|---|---|
| `get_deal` / `list_deals` | Deal records, filterable by stage, county, flag, date range |
| `get_mailer` / `list_mailers` | Mailer records, filterable by status, campaign, county |
| `get_campaign` / `list_campaigns` | Campaign records and their metrics |
| `get_asset` / `list_assets` | Asset/lease records |
| `get_document` / `list_documents` | Document records and their extracted fields (not raw file bytes, unless explicitly requested for an extraction task) |

### Write tools (staged, not immediate)
| Tool | Scope | Confirmation required |
|---|---|---|
| `propose_deal_update` | Stage, flag, notes on a deal | Yes — always |
| `propose_mailer_update` | Status on a mailer record | Yes — always |
| `draft_message` | Creates a draft email/SMS in the review queue | Yes — send is a separate, human-only action |
| `create_document_record` | Attaches extracted fields to a document | Yes, if financial fields are involved; extraction metadata otherwise auto-saves with an "AI-populated" tag |

### Explicitly excluded
- No tool sends email or SMS directly — sending is a UI action only, never exposed to the model as a tool.
- No tool issues payments or modifies financial totals directly.
- No tool deletes records. Deletion stays a manual, human-only UI action.
- No tool has access outside the CRM's own data (no general file system, no arbitrary web access) unless a future integration explicitly adds and scopes it (e.g., a scoped RRC lookup tool).

## Permissioning

Tool access is scoped per user role, the same as UI access — an agent acting on behalf of a given user can't see or do more than that user could through the interface. There is no elevated "AI service account" with broader access than any human user has.

## Auditability

Every tool call is logged with: which user's session initiated it, what the model requested, what the tool returned or staged, and — for write tools — whether and when a human confirmed it. This makes any AI-driven change traceable back to a specific request and a specific approval.

## Future enhancements

- Additional scoped tools as new integrations are added (e.g., a read-only GoHighLevel lookup tool, a scoped RRC query tool replacing the current Apps Script call)
- Per-tool rate limiting to prevent a runaway agent loop from generating excessive draft/proposal volume
