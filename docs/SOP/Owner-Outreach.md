# SOP: Owner Outreach

## When this applies

Any correspondence to a mineral rights owner — initial mailer follow-up, PSA send, response to an owner question, or a check-in on a stalled deal.

## Steps

1. **Identify the record.** Open the relevant `Deal` or `Mailer` record. Confirm you're looking at the current owner/contact info, not a stale duplicate (check `A - Mailers v1 (archive)`-style duplicates if the CRM ever shows more than one record for the same owner).
2. **Determine correspondence type.** Initial outreach, follow-up (no response yet), PSA reminder, or a reply to something the owner said.
3. **Draft.** Use the AI Email Assistant (`modules/AI/AI_EMAIL_ASSISTANT.md`) to generate a first draft, or write one directly if the situation is unusual enough that a template won't fit well.
4. **Review before sending — every time, no exceptions:**
   - Owner name, lease/unit, county, and offer amount are correct for *this* owner (not carried over from a template incorrectly).
   - Tone matches the situation — a first outreach reads differently than a fourth follow-up.
   - Nothing is promised that isn't actually confirmed (e.g., don't imply a closing date that hasn't been set).
5. **Send.** Sending is always a deliberate, separate action from drafting — the system will never send on its own.
6. **Confirm the record updates.** Last Contact Date should update only after the send is confirmed, never at draft time.

## SMS-specific rule

SMS requires explicit authorization for that specific batch or message — a standing send-all approval does not exist. If in doubt about whether a given SMS send has been authorized, ask before sending, don't assume.

## What not to do

- Don't batch-send without reviewing each draft individually — a template that reads fine in aggregate can read badly for a specific owner's situation.
- Don't mark a mailer or deal as contacted before the message actually goes out.
- Don't improvise offer numbers in correspondence — pull them from the record, don't recalculate from memory.

## Related

- `modules/AI/AI_EMAIL_ASSISTANT.md` — the drafting tool this SOP assumes you're using
- `docs/BUSINESS_WORKFLOWS.md` §2 — where outreach fits in the overall process
