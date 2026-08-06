---
name: nausika-setup
description: Connect Nausika, the marine data MCP server at mcp.nausika.app, to this agent and verify it responds. Use when the nausika_* tools are missing or failing, or when the user asks to set up, install, connect or authenticate Nausika.
license: MIT
compatibility: Requires an MCP-capable agent and network access. Connects to the Nausika MCP server at https://mcp.nausika.app over Streamable HTTP, authenticating with OAuth 2.1 via GitHub, Google or Microsoft. No API key.
---

Connect this agent to Nausika's MCP server, then prove it works.

## 1. Check before you install

If tools named `nausika_*` are already available, call `nausika_geocode` with `query: "Portofino"`. If it returns coordinates, say so and stop — there is nothing to install.

## 2. Connect

The server is `https://mcp.nausika.app` — Streamable HTTP, OAuth 2.1 via GitHub, Google or Microsoft. No API key to paste.

**Installed via the Nausika plugin?** The connector ships with it and is already configured. Skip to `/mcp`, select `nausika`, and sign in.

**Claude Code:**

```bash
claude mcp add --transport http nausika https://mcp.nausika.app
```

Then run `/mcp`, select `nausika`, and complete the browser sign-in.

**Claude.ai:** profile menu → Customize → Connectors → Add custom connector → paste `https://mcp.nausika.app` → Add → Connect.

**Any other MCP client:** add a remote server with URL `https://mcp.nausika.app` and transport `http` (Streamable HTTP). The client runs the OAuth handshake; there is nothing to configure by hand.

If the tools still do not appear, restart the session — most clients load MCP servers only at startup.

## 3. Verify

Ask for one concrete thing and report the result: `nausika_geocode` for Portofino, then `nausika_marine_forecast` at those coordinates. An authentication error means the OAuth handshake never completed — repeat step 2.

## Scope

Curated places, ratings and sea routing cover the **Mediterranean**. Forecasts, tides, geocoding and coastline checks work worldwide. `nausika_sea_route` errors outside the Mediterranean by design.

To actually plan with the tools, use the `nausika-passage-planning` skill.
