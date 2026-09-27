# User Roles

## Overview

Access and responsibility in the CRM follow the same informal division of labor that already exists in the business — this document just makes it explicit so the system's permission model (once multi-user, per `SYSTEM_ARCHITECTURE.md`) matches how the team actually works.

## Roles

### Owner
- **Sees:** everything — full pipeline, all modules, all reports.
- **Approves:** closings, curative sign-off where legal judgment is needed, anything requiring a financial decision.
- **Needs from the system:** a one-sentence outcome summary before being asked to approve anything — not a request to read full history first. Manual-before-automation: prefers to confirm the human process works before it's automated.
- **Primary modules:** Dashboard, Deals (approvals), Closing, Payments, Reports.

### Title/Curative Lead
- **Sees:** Deals, Assets, Documents, Due-Diligence, Title-Review, Curative.
- **Owns:** chain-of-title work, curative item resolution, due-diligence flag review and confirmation.
- **Primary modules:** Due-Diligence, Title-Review, Curative.

### Campaign/Mailer Lead
- **Sees:** Mailers, Campaigns, Deals (pipeline visibility).
- **Owns:** campaign planning, mailer list generation and maintenance, RTS/dead-mailer triage.
- **Primary modules:** Mailers, Campaigns.

### Operations/Automation (Maria, JM Brandify)
- **Sees:** everything, plus the underlying automation and system configuration.
- **Owns:** the CRM itself, the automation layer (RRC verification, drafting workflows, AI layer), data integrity, and onboarding new modules.
- **Primary modules:** all, plus system-level configuration not exposed to other roles.

## Permission model (target state)

| Capability | Owner | Title Lead | Campaign Lead | Systems Admin |
|---|---|---|---|---|
| View all modules | Yes | Yes | Yes | Yes |
| Edit Deals | Yes | Yes (DD/title fields) | View only | Yes |
| Edit Mailers/Campaigns | View only | View only | Yes | Yes |
| Edit Due-Diligence/Title-Review/Curative | Approve only | Yes | View only | Yes |
| Approve Closing | Yes | Propose only | No | Propose only |
| Confirm Payments | Yes | No | No | Propose only |
| System/automation config | No | No | No | Yes |

## MVP note

The current MVP is single-user (Maria's account, personal storage). This document describes the target permission model for when the system moves to shared, multi-user storage (see `SYSTEM_ARCHITECTURE.md`, "Scaling path").

## Guiding principle

Every role can **see** more than they can **write** — visibility across the whole pipeline is cheap and useful for everyone; write access stays scoped to who's actually doing that work, so accountability for a given record stays clear.
