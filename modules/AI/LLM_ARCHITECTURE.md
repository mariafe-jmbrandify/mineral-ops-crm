# LLM Architecture

## Overview

Describes how model calls are structured across the AI layer: which model handles which kind of task, how context is assembled, how responses are validated before they touch CRM data, and how cost is kept proportional to task complexity.

## Model routing

Not every AI feature needs the same model. Requests are routed by task type:

| Task type | Example | Model tier | Why |
|---|---|---|---|
| Simple extraction / classification | Document type detection, field extraction on clean text | Lighter/faster model | High volume, well-defined output, low ambiguity |
| Reasoning over messy or legal text | Chain-of-title assembly, due-diligence flagging | Higher-capability model | Requires judgment across ambiguous, high-stakes documents |
| Conversational / drafting | AI Assistant chat, email drafting, report narratives | Higher-capability model | Needs to hold context, tone, and multi-turn coherence |
| Scoring / ranking | Deal Analyst priority score | No model call for the score itself — deterministic formula; model only explains the result | Keeps the number stable, auditable, and cheap to compute at scale |

## Context assembly

Each request assembles only the context relevant to the task, rather than dumping the entire record set into every call:

- **Record-scoped tasks** (drafting, flagging, extraction) get the specific record plus its directly linked records (e.g., a deal plus its linked documents and owner history) — not the whole Deals table.
- **Cross-module tasks** (assistant chat, search, reports) retrieve relevant records via query first, then pass only the matched subset into the model call.
- **Conversation history** is scoped to the current session; nothing persists across sessions unless the user explicitly saves something as a note on a record.

## Response handling

1. Model responses are parsed into structured fields where the task calls for one (e.g., extraction, scoring rationale) rather than left as free text the UI has to guess at.
2. Anything that will be written to a CRM record is staged, not written directly — see the per-module guardrails in each `AI_*.md` file for what requires human confirmation.
3. Low-confidence or malformed responses are retried once with clarified instructions before being surfaced to the user as "needs review" rather than silently failing.

## Guardrails baked into the architecture

- No model call has write access to the database directly; writes go through the same validated API path a human-driven UI action would use.
- Financial and communication outputs are structurally flagged as "requires confirmation" at the API level, not just the UI level, so a UI bug can't accidentally bypass the review step.
- All model calls are logged (input context reference, output, which record it affected) for auditability.

## Tech stack

- Anthropic API (Claude) for all reasoning, drafting, and extraction tasks
- MCP tool layer for controlled read/write access (see `MCP_INTEGRATION.md`)
- Structured output validation on every call that's expected to produce fields the CRM will store

## Future enhancements

- Prompt/response evaluation harness to catch regressions when prompts are updated (see `PROMPT_LIBRARY.md`)
- Cost/usage dashboard per feature, so the routing table above can be tuned against real usage instead of assumptions
