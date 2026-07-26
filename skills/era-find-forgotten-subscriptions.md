---
name: Find forgotten recurring subscriptions
description: Scan connected accounts through Era Context to surface every recurring charge and forgotten subscription.
api: mcp/era-mcp.yml
server: https://context.era.app/mcp
operations:
  - accounts__list_financial_accounts
  - transactions__list_recurring_charges
  - transactions__search_transactions
scopes:
  - mcp:tools-basic
  - banking:read
---

# Find forgotten recurring subscriptions

Surface recurring charges across all of the user's connected accounts using Era Context.

## Auth
OAuth 2.1 (PKCE S256) with `mcp:tools-basic` and `banking:read`.

## Steps
1. Call `accounts__list_financial_accounts` to confirm which accounts are connected and in scope.
2. Call `transactions__list_recurring_charges` to identify subscription patterns Era has detected across those accounts.
3. For any charge the user wants to investigate, call `transactions__search_transactions` with the merchant or amount criteria to pull the full history behind it.
4. Present the recurring charges grouped by merchant with cadence and amount, flagging any the user says they no longer recognize.

## Rules
- Read-only flow; no writes, no confirmations.
- Do not cancel anything from here — Era agents cannot pay bills or cancel third-party merchants; cancellation happens with the merchant or in Era's own UI.
- Mind the plan's daily agent-call quota (`rate-limits/era-rate-limits.yml`).
