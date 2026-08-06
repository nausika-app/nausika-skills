# Nausika agent skills

[![skills.sh](https://skills.sh/b/nausika-app/nausika-skills)](https://skills.sh/nausika-app/nausika-skills)

Agent skills for [Nausika](https://nausika.app) — real Mediterranean marine data for AI agents: sea-aware routing, wind and wave forecasts, tides, marinas and anchorages.

The data comes from the Nausika MCP server at `https://mcp.nausika.app`. These skills are the procedural knowledge that makes an agent use it correctly — tool chaining, units, coverage boundaries and safety framing.

| Skill | What it does |
|---|---|
| [`nausika-setup`](./skills/nausika-setup) | Connects the MCP server to your agent and verifies it answers. |
| [`nausika-passage-planning`](./skills/nausika-passage-planning) | Plans a passage with the Nausika tools and delivers a passage plan, not a paragraph. |

## Install

**As a Claude Code plugin** — this also installs and configures the MCP connector, so there is nothing left to wire up:

```
/plugin marketplace add nausika-app/nausika-skills
/plugin install nausika@nausika
```

Then run `/mcp`, select `nausika`, and sign in.

**As skills, on any agent** — Claude Code, Cursor, Codex, Copilot, Zed and [70+ others](https://github.com/vercel-labs/skills#supported-agents):

```
npx skills add nausika-app/nausika-skills
```

Straight from the domain, equivalently:

```
npx skills add https://nausika.app
```

Installed this way, the skills do not bring the connector with them — run the `nausika-setup` skill, or add the server by hand:

```bash
claude mcp add --transport http nausika https://mcp.nausika.app
```

## What the data covers

Curated places, ratings and sea routing cover the **Mediterranean basin**. Forecasts, tides, geocoding and coastline checks work worldwide. `nausika_sea_route` errors outside the Mediterranean by design rather than guessing.

Authentication is OAuth 2.1 via GitHub, Google or Microsoft. No API key to paste. Nausika is in public beta.

## Safety

Nausika is data, not advice. It does not replace charts, pilot books or notices to mariners, and a forecast is not an observation. The skipper decides.

## Elsewhere

- [nausika.app](https://nausika.app) — what Nausika is, and how to connect it
- [`nausika-mcp-examples`](https://github.com/nausika-app/nausika-mcp-examples) — connecting from Claude Desktop, Python, TypeScript
- [`nausika-feedback`](https://github.com/nausika-app/nausika-feedback) — bugs, feature requests, coverage gaps

## License

MIT
