---
name: shop-inventory
description: How to answer questions about the woodworking shop's tools from this Sprite. Use for what tools the shop has, their specs (kerf, capacities, sizes), which tool can do a job, and what is missing.
---

# The shop's inventory through the `shop` MCP server

This Sprite reaches the shop's inventory through its MCP server, registered in Claude Code
as `shop`. It is reached through a connector: the Sprites gateway adds the shop's token,
and the Sprite never holds it.

- The server's own instructions, sent when Claude Code connects, describe its tools.
  Follow them. Find items with `search_items` or `list_unknowns`, and read one with
  `get_item` before relying on it. A spec marked verify has not been checked: say so
  before designing to it.
- The token reads only. The server leaves out purchase records, serial numbers,
  locations and photos, and refuses every change. Don't try a tool that changes the
  inventory; if a task needs one, say what it would change and stop.
- Never ask the user for the shop's token, never create one, and never set or write one.
