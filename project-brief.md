# My project brief

## The question

Which settlements in Zaria Local Government Area, Kaduna State sit in low-lying land within 200 metres of a river or watercourse?

## Why it matters

Zaria sits on the Kaduna River system and floods periodically during the rainy season (roughly June to September), affecting settlements near watercourses. A local government planning office, an NGO doing flood preparedness work, or a resident association could use a map of exposed settlements to prioritise where to focus drainage improvement, early warning, or relocation conversations. I am curious about this because Zaria is an area I know, which means I can sanity-check the data against what I actually know about the ground.

## The data I need

- Ward/LGA boundaries for Zaria — to define the study area
- Settlement extents for Zaria LGA — to identify which populated places to assess
- Elevation (DEM) covering Zaria LGA — to identify low-lying land
- Rivers and streams (waterways) for Zaria LGA — to measure proximity

## Where each dataset comes from

- Ward boundaries — GRID3 NGA Operational Wards v3.0 — https://data.grid3.org — GeoPackage/shapefile — Kaduna State is included in this release (24-state coverage)
- LGA boundaries (fallback if ward-level proves too coarse to work with) — GRID3 NGA Operational LGA Boundaries — https://data.grid3.org — GeoPackage/shapefile
- Settlement extents — GRID3 NGA Settlement Extents — https://data.grid3.org — GeoPackage/shapefile
- Elevation — Copernicus DEM 30m or SRTM 30m — https://portal.opentopography.org — GeoTIFF, clipped to Zaria LGA extent
- Rivers and streams — OpenStreetMap, via QuickOSM plugin in QGIS — tag `waterway=river`, `waterway=stream`, extracted for the Zaria LGA extent

## What I would build

A map showing Zaria LGA's settlements shaded by flood exposure risk, based on elevation and distance to the nearest watercourse. Longer term (as the programme progresses into later months), I'd like this to become something that updates automatically when new settlement or rainfall data becomes available, rather than a one-off map.

## Known limitation, noted honestly

This uses straight-line distance to watercourses and static elevation, not an actual flood model or historical flood extent data. It's a proxy for exposure, not a hydrological simulation. I may want to bring in CHIRPS rainfall data later to strengthen this.
