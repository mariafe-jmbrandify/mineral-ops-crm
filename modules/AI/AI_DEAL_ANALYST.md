# AI Deal Analyst

## Overview

Scores and triages the Deals pipeline so the team spends time on the deals most likely to close and most valuable, instead of working the list top-to-bottom. Also assists with offer-price consistency by comparing a proposed price-per-NRA against comparable recent deals.

## Why it matters

The pipeline currently runs into the hundreds of open deals at any time (roughly 300+ in the live Z - PSA Deals tracker). Prioritization is manual today — whoever looks at the sheet decides what to work next. A scoring pass surfaces the highest-value, most-actionable deals automatically.

## How it works

### 1. Deal scoring
On a schedule (nightly) and on-demand, each open deal is scored on:
- **Value** — total offer size relative to portfolio average
- **Momentum** — days since last contact, direction of stage movement
- **Risk** — presence and severity of due-diligence flags
- **Responsiveness** — owner engagement history (replied, called back, silent)

Score changes and their drivers are stored alongside the deal, not just a bare number — "Score dropped: no contact in 21 days" rather than an opaque figure.

### 2. Offer-price assist
When a new deal or lease/unit is added, the analyst pulls recent closed deals in the same county/formation and shows a price-per-NRA range, flagging if a proposed offer is a significant outlier (either direction) before it's sent.

## Inputs

- Deal records (stage, offer amount, NRI, county, operator, contact history)
- Closed-deal history for price comparables
- Due-diligence flags from `AI_DUE_DILIGENCE.md`

## Outputs

- A priority score and short rationale per open deal, visible as a sortable column on the Deals table
- Optional "Top 10 to work this week" digest, feeding into `AI_REPORTS.md`
- Price-comparable suggestion shown inline on the deal form, not auto-filled

## Guardrails

- The analyst never changes an offer price or deal stage on its own — it recommends, a person decides.
- Scoring is transparent: clicking the score shows exactly which factors contributed and by how much.
- Price comparables are informational only; final pricing authority stays with the deal owner (matches the existing manual-before-automation approach the owner requires for anything financial).

## Tech stack

- Claude for rationale generation and comparable summarization
- Deterministic scoring formula (not LLM-computed) for the numeric score itself, to keep it stable and auditable — the LLM explains the score, it doesn't invent it

## Future enhancements

- Predicted close probability, once enough closed/dead outcome history exists to train against
- Auto-suggested next action per deal ("call," "resend PSA," "escalate to owner") shown next to the score
