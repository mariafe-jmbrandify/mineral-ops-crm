# AI Layer — Overview

The AI layer sits on top of the core CRM modules (Deals, Mailers, Campaigns, Assets, Documents) and adds intelligence, automation, and natural-language access without changing the underlying data model. Every AI feature reads from and writes back to the same records a human would edit by hand — there is no shadow data store.

## Design principles

1. **Human-in-the-loop by default.** No AI feature sends an email, SMS, offer, or payment on its own. Every outbound action is drafted, then held for explicit approval — the same standing rule already used in the client's email/SMS drafting workflow (`ClaudeDrafting.gs`): the assistant never auto-sends, and status only updates once a send is confirmed by a person.
2. **Traceable, not magic.** Every AI-generated suggestion (a stage change, a flag, a drafted message, a price recommendation) is stored with the reasoning that produced it, so a team member can see *why* before acting on it.
3. **Model-right-sized.** Not every task needs the same model — see `LLM_ARCHITECTURE.md` for how requests are routed by complexity and cost.
4. **CRM-native.** AI features are thin layers over existing modules; they don't introduce a parallel workflow a user has to learn separately.

## Module index

| File | Covers |
|---|---|
| `AI_ASSISTANT.md` | The in-app conversational assistant (chat sidebar across all modules) |
| `AI_DEAL_ANALYST.md` | Deal scoring, offer-price suggestions, pipeline triage |
| `AI_DUE_DILIGENCE.md` | Automated red/yellow/blue flagging from title and ownership data |
| `AI_TITLE_ANALYST.md` | Chain-of-title extraction and curative issue detection |
| `AI_DOCUMENT_ANALYSIS.md` | OCR + structured-field extraction from PSAs, deeds, invoices |
| `AI_EMAIL_ASSISTANT.md` | Drafting owner/operator correspondence (SMS + email), human-approved sends |
| `AI_SEARCH.md` | Natural-language and semantic search across all modules |
| `AI_REPORTS.md` | Scheduled and on-demand summaries (pipeline, campaign, financial) |
| `AI_AUTOMATION.md` | Background workflow orchestration (n8n / Make.com / Apps Script) |
| `LLM_ARCHITECTURE.md` | Model routing, context management, prompt/response contracts |
| `MCP_INTEGRATION.md` | Tools exposed to LLM agents via Model Context Protocol |
| `PROMPT_LIBRARY.md` | Canonical prompt templates used across the above features |

## Relationship to core modules

```
Deals ──┬── AI Deal Analyst (scoring, pricing)
        └── AI Due Diligence (flagging)

Mailers ── AI Email Assistant (drafting) ── AI Automation (campaign triggers)

Assets ── AI Title Analyst (chain-of-title, curative)

Documents ── AI Document Analysis (extraction) ── AI Title Analyst

All modules ── AI Assistant (chat) ── AI Search ── AI Reports
```

## Status

This folder documents the target architecture for the AI layer. The MVP CRM (Deals / Mailers / Campaigns / Assets / Documents) is already built and in use; AI features are being layered in incrementally, starting with Document Analysis and the Email Assistant, since both have working precedents in the existing Apps Script automation suite.
