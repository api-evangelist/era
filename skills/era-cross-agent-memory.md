---
name: Set up portable cross-agent financial memory
description: Store durable financial facts and goals in Era Context so any connected MCP client shares the same portable memory.
api: mcp/era-mcp.yml
server: https://context.era.app/mcp
operations:
  - knowledge__remember
  - knowledge__get_financial_context_and_overview
  - knowledge__recall_history
  - knowledge__forget
scopes:
  - mcp:tools-basic
  - mcp:tools-write
  - mcp:resources-read
---

# Set up portable cross-agent financial memory

Era's differentiator is a portable memory layer any MCP-compatible agent can read. Use these tools to seed and manage it.

## Auth
OAuth 2.1 (PKCE S256). Reading context needs `mcp:tools-basic`/`mcp:resources-read`; storing facts needs `mcp:tools-write`.

## Steps
1. Call `knowledge__get_financial_context_and_overview` to read what Era already knows about the user before adding anything.
2. Call `knowledge__remember` to store new durable facts, goals, or preferences the user states (e.g. "saving for a house down payment by 2027").
3. Call `knowledge__recall_history` to review the history of stored facts and verify what has been captured over time.
4. Call `knowledge__forget` to remove any fact the user no longer wants stored — this is destructive, so confirm the exact fact before removing it.

## Rules
- Only store facts the user actually stated; never infer-and-store sensitive facts silently.
- `knowledge__forget` and `knowledge__reset_pack_questions` are destructive — require explicit confirmation.
- The memory is portable across every connected agent (Claude, ChatGPT, Cursor, etc.); tell the user that a stored fact becomes visible to all their connected clients.
- Mind the plan memory-fact ceiling (Basic has none; Organize 200 facts; Automate/Operate unlimited — see `plans/era-plans.yml`).
