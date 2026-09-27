# AI Title Analyst

## Overview

Reads title opinions, deeds, and probate/AOH documents to reconstruct the chain of title for a lease or unit, and identifies specific curative items standing in the way of a clean close. This is the deeper, document-level counterpart to `AI_DUE_DILIGENCE.md`, which handles deal-level flagging.

## Why it matters

Chain-of-title review is one of the most time-consuming manual steps in mineral rights acquisition — tracing ownership through multiple deeds, deaths, and conveyances. A first-pass reconstruction cuts the manual review down to checking the AI's chain against source documents, rather than building it from scratch.

## How it works

1. All documents linked to an asset/lease (deeds, title opinions, probate records) are pulled together.
2. The analyst extracts, per document: grantor, grantee, interest conveyed, date, and instrument type.
3. These are assembled into a chronological chain-of-title, with gaps or breaks (e.g., an owner who appears as grantee but never as grantor in a later conveyance) explicitly called out.
4. Curative items are listed individually — e.g. "Probate needed for [owner], deceased [date], no AOH on file" — each linkable to the specific supporting document.

## Inputs

- Deeds, title opinions, probate/AOH documents (via `AI_DOCUMENT_ANALYSIS.md` extraction)
- Asset/lease records for context (county, legal description)

## Outputs

- Chain-of-title summary (chronological list of conveyances)
- Curative item list, each with a description, the affected owner/interest, and required next step
- Confidence flag per link in the chain — "confirmed by document" vs. "inferred, needs verification"

## Guardrails

- Every claim in the chain links to its source document — no chain-of-title conclusion is presented without a citable instrument behind it.
- Gaps and low-confidence inferences are surfaced, not smoothed over — the tool is built to make problems visible, not to make the chain look cleaner than the documents actually show.
- Final title opinion authority remains with the person/attorney responsible for sign-off; this tool accelerates the read, it does not replace legal review.

## Tech stack

- Claude for document reading and chain assembly, given its ability to reason over structured extraction plus unstructured legal language
- Structured storage of the assembled chain (not just a paragraph) so it can be queried and diffed as new documents come in

## Future enhancements

- Automatic re-run when a new document is added to the asset, updating the chain rather than requiring a manual re-trigger
- Cross-deal pattern detection (e.g., a county/operator combination that consistently needs probate work) feeding back into `AI_DEAL_ANALYST.md` risk scoring
