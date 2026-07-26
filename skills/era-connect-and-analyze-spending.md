---
name: Connect a bank account and analyze spending
description: Link a financial institution to Era Context, then summarize balances, transactions, spending, and cash flow through the MCP server.
api: mcp/era-mcp.yml
server: https://context.era.app/mcp
operations:
  - connections__connect_bank_account
  - accounts__list_financial_accounts
  - accounts__check_account_balance
  - transactions__list_transactions
  - insights__analyze_spending
  - insights__get_cash_flow
scopes:
  - mcp:tools-basic
  - banking:read
---

# Connect a bank account and analyze spending

Use the Era Context MCP server (`https://context.era.app/mcp`, Streamable HTTP, OAuth 2.1) to onboard a bank account and produce a spending picture.

## Auth
Complete the OAuth 2.1 authorization-code + PKCE (S256) flow at `https://forge.era.app/oauth/authorize`. Request at least `mcp:tools-basic` and `banking:read`. Era never hands the agent bank credentials — linking happens in Era's own flow.

## Steps
1. Call `connections__connect_bank_account` to initiate the bank-linking flow. This hands control to Era's secure linking UI; wait for the user to finish and for the connection to report ready.
2. Call `accounts__list_financial_accounts` to enumerate connected accounts.
3. For each account of interest, call `accounts__check_account_balance` for the current balance.
4. Call `transactions__list_transactions` to pull the recent transaction records you want to reason over.
5. Call `insights__analyze_spending` to categorize and summarize expenses over the period.
6. Call `insights__get_cash_flow` to report net money movement.

## Rules
- These are read tools (`banking:read`); they do not modify data. No confirmation needed.
- Respect the daily agent-call quota for the user's plan (Basic 250 → Operate 25,000; see `rate-limits/era-rate-limits.yml`).
- Do not attempt to move money here — transfers require explicit per-action approval and are out of scope for analysis.
