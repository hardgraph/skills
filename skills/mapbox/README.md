# mapbox

![mapbox cover](./assets/readme-cover.png)

Reference skill for [Mapbox](https://www.mapbox.com/) — the developer platform
for maps, location search, and navigation. It steers an agent through access
tokens, the Maps APIs (Tiles, Styles, Static Images, Tilequery), the Navigation
APIs (Directions, Matrix, Isochrone, Optimization, Map Matching), the Search
APIs (Geocoding, Search Box), and data management (Uploads, Datasets, Mapbox
Tiling Service), without relying on stale version recall.

## Install

```bash
npx skills add hardgraph/skills --skill mapbox
```

## Use this skill for

- Adding an interactive map to a web or mobile app (GL JS, Maps SDKs)
- Requesting Vector/Raster Tiles or a server-rendered Static Image
- Routing between waypoints with the Directions API and picking a profile
- Geocoding an address or reverse-geocoding coordinates
- Computing a Matrix, Isochrone, or optimized stop order
- Snapping a GPS trace to the road network with Map Matching
- Publishing a custom tileset via Mapbox Tiling Service or the Uploads API

## What is included

- [`SKILL.md`](./SKILL.md) — the agent procedure and the API-selection decision criteria.
- [`references/vendor/llms.txt/`](./references/vendor/llms.txt/) — a reproducible
  verbatim mirror of the Mapbox API reference pages the seed index links to,
  used for exact endpoint paths, parameters, and rate limits.

## Source

Reference material is reproduced from the
[Mapbox API documentation](https://docs.mapbox.com/api) via its official
[llms.txt index](https://docs.mapbox.com/llms.txt).
