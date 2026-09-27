# Security

## Data sensitivity

This system holds real, sensitive business data: mineral rights owners' names, home addresses, phone/email, decimal interests, and offer amounts — personally identifiable information (PII) and commercially sensitive deal terms. It is treated accordingly throughout this document, not as an afterthought.

## Current state (MVP)

- Data is stored via the platform's per-user private storage — accessible only to the authenticated account that created it, not shared or public.
- No file uploads of raw documents are stored in the MVP; document records hold metadata and external links (e.g., Google Drive), and access control for the underlying files is whatever Drive's sharing settings are set to — this repo's controls don't extend to files stored outside it.
- Single-user access model: there is currently no multi-user permission surface to secure, since only one account uses the system.

## Target state considerations

### Access control
- Role-based permissions per `USER_ROLES.md`, enforced server-side at the API layer (`API_DOCUMENTATION.md`), not just hidden in the UI.
- AI/MCP tool access is scoped to exactly what each feature needs (`modules/AI/MCP_INTEGRATION.md`) — no broad "AI service account" with more access than any human user has.

### Data protection
- PII fields (owner name, address, phone, decimal interest) encrypted at rest once a production database is in place.
- Transport encryption (HTTPS/TLS) for all API traffic — non-negotiable once there's a network hop involved, not deferred as a "later" item.
- Backups encrypted and access-restricted the same as production data — a backup is a copy of the same sensitive data, not a lesser-risk artifact.

### AI-specific security
- No model call has direct write access to the database — writes go through the same validated API path as a human UI action (`modules/AI/LLM_ARCHITECTURE.md`).
- Documents processed for extraction are not sent to any destination outside the approved Anthropic API integration; no AI feature browses the open web with access to CRM data in context.
- Prompt inputs are scoped to the specific record(s) relevant to a task, not the full dataset, limiting exposure if a single request were ever misused or logged improperly.

### Financial safeguards
- No AI tool can move funds, confirm a wire, or change payment status — these remain human-only actions (`Closing.md`, `Payments.md`).
- **Wire instructions are always verified by phone call before funds move**, independent of any system safeguard — this is a process control, not something software can substitute for.

### Audit trail
- Every AI-proposed change is logged with what was proposed, by which feature, and whether/when a human confirmed it (`modules/AI/MCP_INTEGRATION.md`, "Auditability").
- Every automated workflow run is logged with trigger, actions taken, and outcome (`modules/AI/AI_AUTOMATION.md`).

## Incident response (target)

Not yet formalized given single-user MVP status. Before multi-user production rollout, this section should define: who's notified on a suspected breach, how affected owners/deals are identified, and disclosure obligations given the PII involved (mineral rights owners are individuals, and several are affected by state-level breach notification laws depending on residence).

## What this document is not

This is a working set of principles for a small, actively-developed system — not a compliance certification (SOC 2, etc.). If the business reaches a scale or contractual context requiring formal compliance, that's a distinct effort building on these foundations, not something this document alone satisfies.
