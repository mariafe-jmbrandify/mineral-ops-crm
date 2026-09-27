# AI Document Analysis

## Overview

Turns uploaded PDFs and scans — PSAs, deeds, invoices, title opinions — into structured, searchable fields attached to the right deal, asset, or document record, instead of a file sitting in a folder with no data behind it. This module is the extraction layer that `AI_DUE_DILIGENCE.md` and `AI_TITLE_ANALYST.md` build on top of.

## Why it matters

This has a working precedent already: the Pipeline B invoice ingestion system (Drive OCR + Claude API) does exactly this for invoices today. This module generalizes that pattern across every document type the CRM handles.

## How it works

1. A document is uploaded (or synced from Google Drive) and linked to a record.
2. OCR runs if the file is a scan rather than native text.
3. The extraction step runs a document-type-specific prompt (see `PROMPT_LIBRARY.md`) to pull structured fields:
   - **PSA** → owner, property, offer amount, signature date, expiration
   - **Deed** → grantor, grantee, county, legal description, recording info
   - **Invoice** → invoice #, amount, campaign, payment status
   - **Title opinion** → chain-of-title entries, curative notes (handed to `AI_TITLE_ANALYST.md`)
4. Extracted fields populate the Documents record automatically; the original file remains attached as the source of truth.
5. Low-confidence extractions are flagged for manual review rather than silently accepted.

## Inputs

- Uploaded file (PDF, image) or Drive-linked file
- Document type (auto-detected or user-selected)

## Outputs

- Structured document record (type, linked deal/owner, key fields, status)
- Extraction confidence score
- Source file remains viewable/downloadable alongside the extracted data

## Guardrails

- Extracted fields are marked as AI-populated until a person confirms them, so a misread number doesn't quietly become "the record."
- Financial fields (offer amounts, invoice totals) always show the extracted value next to a "verify" prompt before they feed into anything downstream (payments, reporting).
- Original documents are never modified or deleted — extraction is additive.

## Tech stack

- OCR (existing Drive OCR pipeline) for scanned documents
- Claude API for field extraction and document classification
- Same base pattern as the existing Pipeline B invoice ingestion, extended to additional document types

## Future enhancements

- Duplicate detection (same document uploaded twice, or a near-duplicate with different terms)
- Auto-classification of unlabeled uploads into the right document type before extraction runs
