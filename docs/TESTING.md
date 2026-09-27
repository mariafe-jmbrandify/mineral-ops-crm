# Testing

## Current state (MVP)

No automated test suite yet — the MVP is a single-file client-side app validated manually against the imported real data (record counts checked post-import; syntax validated with `node --check` before each release). This document defines what testing should look like as the system grows past that stage.

## Testing layers (target)

### Unit tests
- Scoring logic (`AI_DEAL_ANALYST.md` priority formula) — deterministic, so fully testable without mocking an LLM call.
- Field validation logic in the API layer — every schema constraint in `DATABASE_SCHEMA.md` should have a corresponding test that a bad value is rejected.
- Status/stage transition rules (e.g., a deal can't move to `Closed` while it has open `Curative` items).

### Integration tests
- API endpoint behavior against a test database: create/read/update/delete for each resource, including permission checks per `USER_ROLES.md`.
- MCP tool calls against a test CRM instance, verifying that write tools stage proposals rather than applying directly, per `modules/AI/MCP_INTEGRATION.md`.

### AI/prompt testing
- A fixed, anonymized set of real-shaped examples (per document type, per flag scenario) run against each prompt in `modules/AI/PROMPT_LIBRARY.md` before deployment, checking output structure and — for due-diligence and title-related prompts specifically — that the model correctly flags low-confidence cases instead of guessing.
- Regression testing whenever a prompt template changes, since prompt edits can shift behavior in ways a human eyeballing a couple of examples might miss.

### End-to-end / manual QA
- Full walk-through of each core workflow in `BUSINESS_WORKFLOWS.md` (campaign → mailer response → deal → due diligence → curative → closing → payment) before any production release that touches those modules.
- Data import/migration scripts tested against a copy of real data in a non-production environment first, never run cold against production.

## What "done" looks like for a change

- New fields/modules: schema validated, covered by at least one create/update test.
- New AI features: prompt tested against the fixed example set, guardrails (confirmation-required behavior) verified with an integration test, not just manual spot-check.
- Anything touching financial fields (`totalOffer`, `Payments`) or communications (`AI_EMAIL_ASSISTANT.md`): manual review required regardless of automated test coverage, given the stakes.

## Non-goals

Not aiming for exhaustive UI test automation at current team size — manual QA against the workflow checklist above is the practical bar until the system and team are large enough to justify the investment in full UI test automation.

## Related

- `DEPLOYMENT.md` — where testing fits in the release process (staging verification gate)
- `SECURITY.md` — security-specific testing considerations (permission enforcement, PII handling)
