# SOP: Closing — Wire Verification

## When this applies

Every time a deal reaches the closing step and funds are about to move by wire. No exceptions — this applies regardless of deal size, how well the paperwork checks out, or how confident anyone is that the details are correct.

## Why this SOP exists

Wire fraud targeting real estate and mineral rights closings (intercepted or spoofed wire instructions) is a known, common attack pattern in this industry. Paperwork alone — even signed, even from a familiar-looking source — is not sufficient verification, because paperwork can be intercepted or altered without either party immediately noticing.

## Steps

1. **Complete the `Closing` checklist items that precede wire release** — PSA finalized, deed prepared, deed recorded (or recording confirmed in progress per local practice).
2. **Obtain wire instructions** through the deal's established channel (whatever was used earlier in the deal — do not accept new/changed wire instructions received only by email without independent verification, even if the email looks legitimate).
3. **Call the receiving party directly, using a phone number you already have on file for them — not a number provided in the same message as the wire instructions.** Confirm:
   - Bank name and routing number
   - Account number
   - Account holder name matches the expected recipient
4. **Log the verification** — who was called, when, and that the instructions were confirmed verbally — as part of the `Closing` record before marking `wireConfirmed` true.
5. **Only after verification is logged**, proceed with the wire.
6. **Confirm receipt** with the recipient after the wire is sent, and log that confirmation too.

## Red flags that should stop the process immediately

- Wire instructions that changed since they were first provided, especially close to closing.
- Any pressure to skip verification "to save time."
- A phone number for verification that only appears in the same document/email as the new instructions (always use a previously-known number).

## What not to do

- Don't treat a signed PSA or recorded deed as sufficient verification for wire instructions — they confirm the deal, not the destination of funds.
- Don't delegate this step without the same rigor — whoever executes it follows this exact checklist, no shortcuts based on deal size or familiarity with the counterparty.
- Don't mark `wireConfirmed` true before the call actually happens.

## Related

- `modules/Closing.md` — where this checklist item lives in the system
- `modules/Payments.md` — payment status tracking after the wire is sent
