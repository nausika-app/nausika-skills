---
name: nausika-passage-planning
description: Plan a passage, crossing or day sail with real marine data — sea-aware routing, wind and wave forecasts, tides, marinas and anchorages. Use for any question about sailing somewhere, a weather window, an ETA, distance by sea, where to moor or shelter, or conditions along a coast. Requires the Nausika MCP tools (nausika_*).
license: MIT
compatibility: Requires the Nausika MCP server connected to the agent — install the nausika-setup skill first, or the Nausika plugin, which ships the connector. Detailed sea routing and curated places cover the Mediterranean, the European Atlantic with the Canaries, the Caribbean and South-East Asia; forecasts, tides and geocoding are worldwide.
---

Plan passages with Nausika's tools. Never answer a maritime question from memory when a tool can answer it.

If no `nausika_*` tool is available, say so and use the `nausika-setup` skill. Do not improvise a substitute.

## Units, always

Distance in **nautical miles**, speed in **knots**, depth and height in **meters**, coordinates as **decimal degrees, (latitude, longitude)**. Wind and wave directions are the direction the weather comes **FROM**.

## The chain

1. **`nausika_geocode`** — every place name, first. Never write coordinates from memory. Check `source` on each result: `local` is the curated marine database, `osm` is the global fallback and may not be a marine feature at all.
2. **`nausika_get_boat_profile`** — if the user has a boat saved, use its cruising speed and consumption instead of the defaults. Skip silently when there is none.
3. **`nausika_marine_forecast`** — at origin, at destination, and at any waypoint where the route turns a cape. `preset` sets the horizon: `now`, `today`, `tactical` (3 days, default), `planning` (8 days), `extended` (14 days). Past roughly 3 days, present a trend, not a forecast. When the call is marginal, set `include_percentiles: true` and quote the p10–p90 spread instead of a single number.
4. **`nausika_sea_route`** — the only acceptable way to get a distance by sea. It runs Dijkstra over the MARNET sea-lane graph, so it goes around land; a straight line between two ports does not. Inside the covered areas (Mediterranean, European Atlantic and Canaries, Caribbean, South-East Asia) it follows the coast in detail; outside them it uses the main ocean lanes, and it errors when an endpoint is far from any lane — that is a boundary, not a bug to work around. It snaps land coordinates to the nearest port or anchorage. Pass the boat's real speed; the default is 6 kn.
5. **`nausika_search_places`** — marinas, anchorages and shelter near the destination and along the route. Every result carries an 8-char `[xxxxxxxx]` short-id; pass it to **`nausika_get_place`** for depth, holding, exposure, ratings and OSM detail.
6. **`nausika_calculator`** — fuel, range, conversions. It accepts only `+ - * / ^ %`, parentheses, `sqrt abs ceil floor round sin cos tan log exp min max`, `PI` and `E`. Anything else is rejected — decompose the expression instead.

## Tides, before you quote one

`nausika_tides` uses the nearest NOAA station, and falls back to a global tide model elsewhere with a horizon of about 15 days. **Across most of the Mediterranean the range is under 0.3 m: negligible for recreational navigation, and you must not dress it up as a tide window.** Tides genuinely matter on the Atlantic coasts, in the Gulf of Gabès, in the northern Adriatic, and in the tidal streams of the Messina and Gibraltar straits. Everywhere else, say the range is negligible and move on.

## Two forecasts, two questions

`nausika_marine_forecast` answers "what will it do this week". `nausika_seasonal_forecast` is a 1–7 month ECMWF SEAS5 ensemble mean with **no waves** — it answers "is late September usually workable here". Never use the seasonal one to decide a specific day.

Use `nausika_is_water` to sanity-check any coordinate the user supplies before routing from it.

## Deliver a passage plan, not a paragraph

| Leg | From → To | Bearing | Distance (nm) | ETA @ boat speed | Wind / sea | Shelter |

Then state, in this order: the weather window and what closes it; the go/no-go call and the one thing that would change it; the fallback ports, with their short-ids.

## Safety

Nausika is data, not advice. Never present it as an official source. It does not replace charts, pilot books or notices to mariners; a forecast is not an observation, and coverage gaps are real. Say what the data does not cover instead of filling the gap. The skipper decides.
