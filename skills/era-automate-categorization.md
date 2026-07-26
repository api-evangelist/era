---
name: Automate transaction categorization with rules
description: Create pattern-based automation rules in Era Context that categorize and tag transactions automatically.
api: mcp/era-mcp.yml
server: https://context.era.app/mcp
operations:
  - transactions__list_spending_categories
  - transactions__manage_categories
  - transactions__manage_automation_rules
  - transactions__manage_transaction_tags
scopes:
  - mcp:tools-write
  - banking:write
---

# Automate transaction categorization with rules

Turn a plain-English rule ("tag all Whole Foods as Groceries") into a persistent Era automation rule.

## Auth
OAuth 2.1 (PKCE S256). This flow WRITES, so request `mcp:tools-write` and `banking:write` in addition to the basic read scopes.

## Steps
1. Call `transactions__list_spending_categories` to read the existing category taxonomy.
2. If the target category does not exist, call `transactions__manage_categories` to create or adjust it.
3. Call `transactions__manage_automation_rules` to create the rule (match pattern → category/tag action). Confirm the match criteria with the user before creating.
4. Optionally call `transactions__manage_transaction_tags` to apply or remove labels on the existing matching transactions so history is consistent with the new rule.

## Rules
- These are write-capable tools — request the user's confirmation before creating or modifying rules and categories.
- Any tool marked destructive (e.g. batch edits) requires explicit per-action confirmation.
- Never fabricate a match pattern the user did not describe.
- Respect the daily agent-call quota for the plan (`rate-limits/era-rate-limits.yml`).
