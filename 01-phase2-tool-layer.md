# Phase 2 — Build the Tool Layer (Kanban, E-commerce, Finance)

## Goal
Stand up your Kanban board, e-commerce data source, and finance data source as MCP-connected tools — each isolated by domain (see `00-data-isolation.md`) — so Claude Code can query and act on them without merging their data.

## 1. Kanban Board
- **Recommended tools:** Planka, Kanboard, or Focalboard (self-hosted, open-source, each exposes an API).
- Deploy locally or on a small VPS; keep it isolated to Business or Personal depending on what you're tracking (consider two boards if tasks span both).
- Wrap its API as a small MCP server exposing only: `create_card`, `move_card`, `list_cards`, `get_board_status`. Do not expose admin/delete-all endpoints.

## 2. E-commerce Ops
- Wrap your store platform's API (orders, inventory, supplier data) as a dedicated `business-mcp` server.
- Scope tools narrowly: `get_orders`, `get_inventory`, `update_stock_note` — avoid a tool that can directly modify pricing or process refunds without a human approval step.
- Store this MCP server's credentials separately from Kanban/finance credentials — no shared secrets file.

## 3. Finance Tool
- Extend your existing portfolio scripts into a `finance-mcp` server exposing read-only tools: `get_positions`, `get_price`, `get_portfolio_summary`.
- Never let this MCP server also access business or personal vault data — it should have credentials only for your brokerage/market-data API.
- Given your job's clearance/compliance context, treat this as your most sensitive domain: fully isolated vault + fully isolated MCP server + no cross-domain default queries.

## 4. Build Order
1. Deploy Kanban board, verify API access manually (curl/Postman).
2. Build minimal MCP wrapper for Kanban; test Claude Code can create/move a card.
3. Build e-commerce MCP wrapper; test read-only queries first before adding any write actions.
4. Build finance MCP wrapper; keep it strictly read-only initially.
5. Confirm each MCP server only starts when its domain's Claude Code session is active — no global auto-load of all three.

## 5. Verification Checklist
- Each MCP server has its own credentials, its own scope, and cannot see another domain's data.
- Test: open a Business-domain Claude Code session and try to query finance data — it should fail/be unavailable.
- Every write action (create card, update stock note) is logged with timestamp and source.
