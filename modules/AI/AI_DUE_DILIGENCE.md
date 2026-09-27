# AI Due Diligence

## Overview

Automates the first pass of the red / yellow / blue flagging system already used across the Deals pipeline — red for a title or PSA issue, yellow for a royalty or title follow-up, blue for an Affidavit of Heirship (AOH) needed. Instead of a person reading each document and manually setting the flag, the system proposes it based on what's found in the linked documents, and a person confirms or overrides.

## Why it matters

Flags currently get set by whoever reviews the deal, which means the trigger for "needs attention" depends on someone remembering to look. Automating the first pass means nothing sits unflagged just because no one has gotten to it yet.

## How it works

1. When a document is linked to a deal (via `AI_DOCUMENT_ANALYSIS.md` or manual upload), the due-diligence engine scans it for known issue patterns:
   - Missing or unclear chain of title → **Red**
   - Deceased owner with no probate/AOH on file → **Blue**
   - Outstanding royalty question, unconfirmed decimal interest, or a title item still in progress → **Yellow**
2. A proposed flag (with the specific reason and the source document/passage it came from) is attached to the deal.
3. The flag shows as "AI-proposed" until a team member reviews and confirms it — at which point it becomes an active flag exactly like one set manually today.

## Inputs

- Linked documents (title opinions, deeds, PSAs) and their extracted text/fields
- Existing deal and owner records (to catch inconsistencies, e.g. two documents naming different owners for the same interest)

## Outputs

- Proposed due-diligence flag (Red / Yellow / Blue / None) with a one-line reason and source citation
- Flag appears on the Dashboard's "Flagged Deals" panel once confirmed, same as today

## Guardrails

- Flags are proposals, not final states, until a human confirms — this preserves the existing review step rather than silently auto-flagging.
- If the engine can't find enough signal to propose a flag confidently, it says so explicitly rather than defaulting to "None," so a genuinely unreviewed deal isn't mistaken for a clean one.
- Every proposed flag links back to the exact document and excerpt that triggered it, so review is fast rather than a re-read of the whole file.

## Tech stack

- Document text extraction feeding into Claude for pattern/issue detection
- Rule-based checks (name matching, date gaps) layered alongside the LLM read for things that don't need judgment, just comparison

## Future enhancements

- Trend view: which counties/operators generate the most red flags, to inform which mailer campaigns to prioritize or avoid
- Auto-drafted curative task list per red-flagged deal (see `Curative.md`)
