# AI Automation

## Overview

The background workflow layer that connects the CRM to outside systems and runs recurring processes — mailer campaign scheduling, RRC (Texas Railroad Commission) spacing verification, invoice ingestion, and other multi-step jobs that shouldn't require someone to manually kick off each step. This module documents orchestration; the AI reasoning steps within each workflow are documented in their own files (Document Analysis, Due Diligence, etc.).

## Why it matters

Several of these processes already run as standalone automations (RRC spacing verification via Google Apps Script, the AI SDR/job-matching pipeline pattern, invoice ingestion). Bringing them under one orchestration layer means they read and write directly to CRM records instead of living in disconnected scripts and sheets.

## How it works

Automations are built as triggered workflows, each with a clear trigger, steps, and output:

| Workflow | Trigger | Steps | Writes to |
|---|---|---|---|
| RRC Spacing Verification | New asset added, or manual re-check | Query RRC public system → parse spacing/unit data → compare to recorded acreage | Assets |
| Campaign Touch Scheduling | Campaign reaches its target ship date | Pull recipient list → generate mail merge data → mark as Shipped | Campaigns, Mailers |
| Invoice Ingestion | New invoice file added to Drive | OCR → extract fields (see `AI_DOCUMENT_ANALYSIS.md`) → link to campaign | Documents |
| Due Diligence Sweep | New document linked to a deal | Run extraction + flag proposal | Deals, Documents |
| Stale Deal Check | Nightly | Find deals with no contact in 21+ days → queue for Deal Analyst scoring and optional follow-up draft | Deals |

## Inputs

- Trigger events (new record, schedule, external system webhook)
- Existing CRM data needed to complete each step

## Outputs

- Updated CRM records (never external-only side effects — everything a workflow does is reflected back in the CRM so it's visible to the team)
- A run log per workflow execution: what triggered it, what it did, what it changed

## Guardrails

- Automations that touch communications (email/SMS) or money (offers, invoices, payments) stop at a human-review step rather than completing end-to-end — consistent with the no-auto-send, manual-before-automation approach used throughout the system.
- Every automated run is logged with enough detail to reconstruct what happened and why, so a bad run can be traced and undone rather than just noticed after the fact.
- Failed runs alert rather than fail silently — a broken RRC query or a parsing failure surfaces as a task, not a gap no one notices.

## Tech stack

- n8n and/or Make.com for orchestration, matching the tools already used for the RemoteOK/job-matching automation pattern
- Google Apps Script for the existing RRC and email/SMS integrations
- Claude API for any reasoning step inside a workflow (extraction, drafting, flagging)

## Future enhancements

- A visual run history inside the CRM (not just logs) so non-technical team members can see what automation did without reading a script log
- Configurable triggers so new workflows can be added without a code change for simple cases
