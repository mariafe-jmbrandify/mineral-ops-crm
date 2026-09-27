# AI Search

## Overview

Natural-language and semantic search across every module — Deals, Mailers, Campaigns, Assets, Documents — so a question like "everything tied to the Broadus family in Fayette County" returns the right records even if they're spread across a deal, three mailer entries, and a linked deed, without the user needing to know which table to search or the exact spelling used in each one.

## Why it matters

With 11,000+ mailer records and hundreds of deals, keyword search on a single field (exact owner name spelling, for instance) misses a lot. Semantic search handles name variants, partial matches, and cross-module queries in one pass.

## How it works

1. User types a query into the global search bar (available from any module).
2. The query is interpreted for intent — is this an owner lookup, a county/asset lookup, a document lookup, or a combination?
3. Search runs across:
   - Exact/fuzzy match on structured fields (name, county, lease, API number)
   - Semantic match on free-text fields (notes, extracted document text)
4. Results are grouped by module (Deals / Mailers / Campaigns / Assets / Documents) with the matching field highlighted, so it's clear *why* each result matched.

## Inputs

- Free-text query
- Optional module scope, if the user wants to search within just one area

## Outputs

- Grouped, ranked results across modules with match context shown inline
- Direct links into each matched record

## Guardrails

- Search is read-only — it never modifies data, only retrieves it.
- Results show why they matched (which field, what kind of match) so a low-relevance result isn't mistaken for an exact hit.
- No cross-tenant or external data is searched; this is scoped strictly to the org's own CRM data.

## Tech stack

- Structured field search via standard indexed queries
- Semantic/embedding search over notes and extracted document text for fuzzy and cross-module matching
- Claude for interpreting ambiguous queries and routing them to the right search strategy

## Future enhancements

- Saved searches / smart views ("my active Fayette County deals") that stay live rather than being a one-time query
- Search-driven bulk actions ("select all these results and tag them")
