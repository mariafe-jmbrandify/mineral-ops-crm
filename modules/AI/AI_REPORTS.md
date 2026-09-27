# AI Reports

## Overview

Generates plain-language summaries of pipeline, campaign, and financial activity — on a schedule (e.g. a Monday morning pipeline summary for the owner) or on demand ("summarize this week's closings"). Reports are generated from live CRM data, not a separate reporting database, so they're always current.

## Why it matters

Right now, understanding "how are we doing" means opening several tabs and eyeballing totals. A generated summary turns that into a two-minute read, and a consistent one — the same format every week, so trends are easy to spot.

## How it works

1. A report type and cadence is configured (e.g., "Weekly Pipeline Summary, every Monday 8am") or requested ad hoc.
2. The report generator queries the relevant modules for the period in question — new deals, stage movements, closings, campaign performance, flagged items needing attention.
3. Claude turns the raw numbers into a short narrative summary plus the supporting figures, following a fixed structure so every report is easy to scan regardless of what's in it that week.
4. The report is delivered as an in-app digest and, optionally, emailed.

## Example report types

- **Weekly Pipeline Summary** — deals moved, new offers made, deals closed, total value change, flagged items needing a decision
- **Campaign Performance** — recipients mailed, response rate, cost vs. profit per touch
- **Closed Deal Recap** — deals closed this period, total acquisition value, average days-to-close
- **Due Diligence Backlog** — count and age of red/yellow/blue flagged deals

## Inputs

- Report type/template
- Date range or trigger schedule
- Live data from all relevant modules for that range

## Outputs

- Narrative summary (a few short paragraphs)
- Supporting numbers/table, so the narrative can be checked against the underlying data
- Delivery via in-app digest, and optionally email

## Guardrails

- Reports are descriptive, not prescriptive — they summarize what happened, they don't make recommendations that read as decisions already made (that's the Deal Analyst's job, and even there, recommendations are clearly labeled as such).
- Every figure in a narrative summary is traceable back to the query that produced it, so a surprising number can be checked rather than taken on faith.

## Tech stack

- Scheduled queries against CRM data (no separate data warehouse needed at current scale)
- Claude for narrative generation from structured query results

## Future enhancements

- Custom report builder for ad hoc combinations beyond the fixed templates
- Trend charts alongside the narrative once enough historical data has accumulated to make trends meaningful
