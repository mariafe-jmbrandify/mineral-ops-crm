# SOP: Due Diligence Review

## When this applies

Whenever a deal has an active or newly-proposed due-diligence flag (Red, Yellow, or Blue) that needs human review.

## Steps

1. **Open the flagged deal** from the Dashboard's "Flagged Deals" panel or the `Due-Diligence` module.
2. **Read the flag reason and source.** If the flag was AI-proposed (`modules/AI/AI_DUE_DILIGENCE.md`), it will cite the specific document and passage that triggered it — open that document and confirm the reasoning actually holds up before accepting the flag as-is.
3. **Classify what's actually needed:**
   - **Red (title/PSA issue)** — does this need a full `Title-Review` chain-of-title pass, or is it a smaller, resolvable issue?
   - **Yellow (royalty/title follow-up)** — is this a quick confirmation with the owner or operator, or does it need to escalate to Red?
   - **Blue (AOH needed)** — confirm the owner is in fact deceased and no probate/AOH is already on file before creating a curative item; don't duplicate work already done.
4. **Decide: confirm, escalate, or dismiss.**
   - **Confirm** — the flag is accurate; leave it active and, if it requires dedicated work, create a `Curative` item.
   - **Escalate** — e.g., a Yellow that turns out to be a real title gap becomes Red.
   - **Dismiss** — the flag doesn't hold up on review (e.g., AI mis-cited a document). Clear it with a one-line reason — never clear a flag silently.
5. **Log the resolution note.** Every flag clear or confirm gets a short note on what was checked and what was decided — this is what makes the flag history useful later, not just a color that changed.

## Escalation

If a Red flag reveals a chain-of-title issue significant enough to affect deal viability (not just delay it), raise it with the owner directly rather than letting it sit in the queue — a one-sentence summary of the issue and its likely impact on the deal, per the standing practice for anything needing the owner's decision.

## What not to do

- Don't accept an AI-proposed flag without checking the cited source — the point of citing it is so it's checked, not skipped.
- Don't clear a flag just to reduce the count on the dashboard — a cleared flag should mean resolved, not ignored.
- Don't let a Blue flag sit without at least confirming probate status — this is usually the fastest of the three to move forward once someone actually looks at it.

## Related

- `modules/Due-Diligence.md` — the module this SOP operates in
- `modules/AI/AI_DUE_DILIGENCE.md` — how flags get proposed in the first place
- `modules/Curative.md` — where confirmed issues needing dedicated work go next
