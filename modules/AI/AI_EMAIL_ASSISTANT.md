# AI Email Assistant

## Overview

Drafts owner and operator correspondence — emails and SMS — based on deal context, so outreach is fast to produce and consistent in tone, without ever sending anything without a person's sign-off. This module formalizes, inside the CRM, the workflow already running today through the Apps Script drafting suite (`Config.gs`, `EmailProcessor.gs`, `ClaudeDrafting.gs`).

## Why it matters

Drafting individual follow-ups to hundreds of mailer contacts is repetitive but needs to stay personal — a form letter reads as one, and mineral rights owners respond better to something that references their specific lease and offer. This closes that gap without adding headcount.

## How it works

1. Trigger: a person selects a deal or mailer record (or a batch, e.g. "everyone with no contact in 21+ days") and requests a draft.
2. The assistant pulls context: owner name, lease/unit, county, offer amount, current stage, and any notes/history already on the record.
3. A draft is generated matching the situation — first outreach, follow-up, PSA reminder, response to a specific question — using the appropriate template from `PROMPT_LIBRARY.md`.
4. The draft is placed in a review queue, never sent automatically.
5. A person edits if needed and sends. Only once a send is confirmed does the record's Last Contact Date update — exactly the standing rule already in place.

## Inputs

- Deal/mailer record and its history
- Correspondence type (initial outreach, follow-up, PSA send, custom)
- Any specific instruction from the requester ("mention the deadline," "shorter, they've already said yes")

## Outputs

- Draft email or SMS text, held in a review queue
- Optional: suggested send time, based on prior response patterns

## Guardrails

- **The assistant never sends.** Every email and SMS requires explicit human review and send action — this is a hard rule carried over unchanged from the existing production workflow, not a new policy.
- **SMS requires explicit authorization** per the existing standing rule — no batch SMS goes out without someone approving that specific batch.
- Last Contact Date only updates on a confirmed send, never on draft creation, so the pipeline's "who have we actually talked to" view stays accurate.
- Drafts are logged with what record/context produced them, so a strange or off-tone draft can be traced back to what the assistant was given.

## Tech stack

- Claude API for drafting, reusing the same drafting logic as `ClaudeDrafting.gs`
- Google Apps Script integration for the existing Gmail/SMS send layer
- Review queue lives inside the CRM so drafts don't require switching tools to approve

## Future enhancements

- Suggested reply drafts when an owner responds, pulling the incoming message into context automatically
- A/B tracking on messaging approaches once enough send/response volume exists to compare them meaningfully
