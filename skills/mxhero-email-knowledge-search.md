---
name: Search company email knowledge with mxHERO
description: Use the mxHERO Mail2Cloud Advanced MCP server to search a company's captured email corpus and recover knowledge with secure links back to original messages.
api: mcp/mxhero-mcp.yml
mcp_server: mxhero-mcp-server
operations:
- email_search
generated: '2026-07-20'
method: generated
source: https://github.com/mxaiorg/mxmcp
---

# Search company email knowledge with mxHERO

mxHERO Mail2Cloud Advanced captures a company's email (in-flight or at-rest) from
Microsoft 365, Google Workspace, and Exchange, deduplicates threads, preserves
metadata (From/To/Cc/Date/Subject), links attachments to their parent messages,
and stores everything in an isolated per-tenant vector database. The MCP server
exposes that corpus to an agent so it can answer questions that live in email,
far beyond a model's context window.

## Prerequisites

- An mxHERO Vector Search access token. Obtain a personal token at
  `https://lab4-mcp.mxhero.com/tokens` (demo tokens preloaded with thousands of
  emails are available for exploration).
- The MCP server connected via one of:
  - stdio binary (`mxaiorg/mxmcp`, Go) with the token passed as the required
    `-t` argument; or
  - streamable HTTP at `https://lab4-mcp.mxhero.com/mcp2/connect`.

## Steps

1. Confirm the `mxhero-mcp-server` MCP server is connected and authenticated
   with a valid token.
2. Call the `email_search` tool with a natural-language or keyword `query`
   describing the information you need (sender, topic, contract, attachment, date
   range hints, etc.).
3. Read the returned JSON results. Each result preserves email metadata and
   includes a secure link back to the original message — cite that link rather
   than reproducing full message bodies.
4. Refine the `query` and repeat if the top results are not relevant; the vector
   store rewards specific, well-scoped queries.

## Notes

- The server searches only the caller's isolated tenant; results never cross
  tenant boundaries.
- Deduplication means one canonical hit stands in for repeated reply/forward
  copies — do not assume a single result implies the message was sent only once.
- mxHERO does not retain original attachment content; follow the secure link to
  reach the source of truth.
