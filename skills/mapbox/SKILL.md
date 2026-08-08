---
name: mapbox
description: Mapbox — developer platform for maps, location search, and navigation REST APIs plus GL JS and mobile SDKs. Use when adding an interactive map to web or mobile, requesting Vector/Raster Tiles or Static Images, rendering a Mapbox Style, geocoding an address or reverse-geocoding coordinates, routing turn-by-turn Directions, computing a Matrix or Isochrone, optimizing stops, map-matching GPS traces, or uploading and managing custom map data with the Uploads API and Mapbox Tiling Service. Published by HardGraph, a curated graph of provenance-backed knowledge for AI agents.
---

# Mapbox

> **What is HardGraph?** HardGraph publishes curated, provenance-backed agent skills grounded in reproducible vendor documentation.

Mapbox is a location platform exposed almost entirely as REST APIs over one
access token. The same token drives the tile servers, the routing engine, the
geocoder, and the data-management endpoints; a client library (Mapbox GL JS for
the web, Maps SDKs for iOS/Android/Flutter) renders the tiles into a pannable
map. Everything is metered against the token's usage.

The model is: get a token, call an endpoint, get JSON or tiles back. The map you
see in a browser is just a client consuming the same endpoints a server can call
directly.

## Mental model

| Concept            | What it is                                                                          |
| ------------------ | ----------------------------------------------------------------------------------- |
| **Access token**   | A `pk.*` (public), `sk.*` (secret), or `ck.*` (custom) token. Scopes and URL restrictions gate what each can call. |
| **Tiles**          | Raster or vector map tiles fetched by `z/x/y` coordinate from the Tiles APIs, or rendered client-side by GL JS from a style. |
| **Style**          | A JSON document (Mapbox Style Spec) describing the sources, layers, and sprites that compose a map. |
| **Directions**     | Routing between waypoints, returning geometry, turn-by-turn steps, and ETA.         |
| **Geocoding**      | Forward (place → coordinates) and reverse (coordinates → place) search.             |

A request is `https://api.mapbox.com/<endpoint>?access_token=<token>`. Public
tokens are safe to embed in client apps; secret tokens belong on a server. URL
restrictions on a token lock it to specific domains, which is how a public token
is kept from being replayed elsewhere.

## The Maps APIs

The Maps surface renders and serves map data.

| API                | Purpose                                                          |
| ------------------ | ---------------------------------------------------------------- |
| **Vector / Raster Tiles** | Map tiles by `z/x/y`, the raw material GL JS and mobile SDKs render. |
| **Styles**         | Read published styles; create and edit styles with the Styles API. |
| **Static Images**  | A single, server-rendered map image at a URL — no client needed.  |
| **Tilequery**      | Query features from a tileset at a point.                        |
| **Fonts**          | Glyph ranges for style text layers.                              |

`Static Images` is the shortcut when you need a map in an `<img>`, an email, or
a report: one URL, one image, no JavaScript.

## Navigation APIs

Turn location into movement. All take coordinates and return routed geometry.

| API             | Purpose                                                                 |
| --------------- | ----------------------------------------------------------------------- |
| **Directions**  | Routes between 2–25 waypoints with steps, annotations, and alternatives. |
| **Matrix**      | Travel times/distances between many origins and destinations.           |
| **Isochrone**   | The area reachable within a time or distance from a point.              |
| **Optimization**| The optimal ordering of stops (a traveling-salesman solve).             |
| **Map Matching**| Snaps noisy GPS traces onto the road network.                           |
| **EV Charge Finder** | Charging stations along or near a route.                          |

Directions profiles are `driving`, `walking`, `cycling`, and
`driving-traffic`; pick by vehicle and whether live traffic matters. The Matrix
API caps matrix size — for large OD matrices, chunk by destination count.

## Search APIs

| API             | Purpose                                                          |
| --------------- | ---------------------------------------------------------------- |
| **Geocoding**   | Forward and reverse geocoding of addresses and POIs, batchable. |
| **Search Box**  | Interactive, autocomplete-forward search with category filters. |

Use Search Box for type-ahead in a UI; use the Geocoding API for one-shot,
batch, or reverse lookups from a server.

## Data management: Uploads, Datasets, Mapbox Tiling Service

To show your own data on a map, get it into a tileset.

- **Uploads API** — turn a GeoJSON/Shapefile/KML into a tileset in one job.
- **Datasets API** — manage editable feature collections.
- **Mapbox Tiling Service (MTS)** — define a tileset recipe and publish custom
  vector tiles with full control over zoom ranges and layers.

Uploads is the simplest path for a one-off GeoJSON; MTS is the production path
for large, frequently updated datasets.

## Client libraries

Mapbox GL JS and the iOS/Android/Flutter Maps SDKs render tiles into an
interactive map, handle user gestures, and style layers in the browser or app.
They read a Style and request tiles from the same endpoints above using the
token. Reach for a client library when you need pan/zoom and interactivity;
reach for the REST APIs directly when you need a computation (a route, a
geocode, an image) without a map UI.

## Current vs deprecated

- Resolve the **current API path and rate limits** from the API reference, not
  memory — Mapbox revises endpoint URLs and adds versioned surfaces over time.
- Prefer **Mapbox Tiling Service** over the legacy Uploads pipeline for
  production custom tilesets; Uploads remains supported but is the simpler, less
  controllable path.
- Access tokens are versioned by scope and URL restriction; rotate a leaked
  token rather than restrict it after the fact.

## References

- [Access tokens](https://docs.mapbox.com/api/accounts/tokens)
- [Vector Tiles API](https://docs.mapbox.com/api/maps/vector-tiles)
- [Styles API](https://docs.mapbox.com/api/maps/styles)
- [Static Images API](https://docs.mapbox.com/api/maps/static-images)
- [Directions API](https://docs.mapbox.com/api/navigation/directions)
- [Geocoding API](https://docs.mapbox.com/api/search/geocoding)
- [Mapbox Tiling Service](https://docs.mapbox.com/api/maps/mapbox-tiling-service)
