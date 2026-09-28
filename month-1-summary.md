# Month 1 summary

## My question, restated

Which settlements in Zaria Local Government Area, Kaduna State sit in low-lying land within 200 metres of a river or watercourse?

## Which operation I ran, and why

I ran a **buffer** on my clipped, reprojected waterways layer (`waterways_zaria_clipped`, 97 features, EPSG:32632), using a distance of 200 metres, with Dissolve result ticked. I chose buffer because my question is phrased as "within X metres of," which the course material identifies directly as a buffer operation. I dissolved the result because I care about the flood-risk zone as a single connected area, not about which individual watercourse each part of it came from.

## What I expected, and what I got

Before running the operation, I expected either roughly 97 separate buffer polygons (one per waterway feature) if I left Dissolve unticked, or a single merged polygon if I ticked it. I ticked Dissolve, and the output was exactly **1 feature** — a single unified 200m corridor following every river, stream, and drain in Zaria LGA. This matched my expectation once I confirmed which setting I had actually chosen.

Visually, the buffer traces through the built-up core of Zaria city (Tukur Tukur, Tudun Wada, Kwarbai A, Kwarbai B, Gyellesu) and extends well beyond it into rural wards to the southeast (Wuciciri) and northwest (Kufena and Dutsen Abba). The buffer is not confined to the city centre — it cuts across most of the wards in the LGA.

## What surprised me

The dissolved output's attribute table kept only one row, with attribute values (`osm_id`, `waterway=river`, etc.) copied arbitrarily from a single one of the 97 original input features. This was not a mistake in the buffer geometry, which is correct and covers the full merged corridor — but it is a genuine data-quality lesson: **a dissolved layer's attribute table does not describe or summarise the dissolved result, it just keeps leftover values from one input feature.** Any analysis that later relies on this buffer's own attributes (rather than just its geometry) needs to account for this rather than trust what's shown in the table.

I also did not expect the flood-risk corridor to reach as far as it does into wards like Wuciciri and Dutsen Abba, which I had thought of as more rural and lower-risk. This is worth investigating further once settlement data is available, since it may mean flood exposure is not limited to the urban core.

## What data I still need

I do not yet have **settlement extents** for Zaria LGA, which means I can identify *where* the 200m flood-risk zone falls, but not yet *which specific settlements* sit inside it. My next step is downloading GRID3 settlement extents, clipping and reprojecting them the same way as my other layers, and running a spatial join (settlements within the buffer) to answer the full question. I also still need an elevation layer (DEM) to add the "low-lying land" part of my original question, which the buffer alone does not capture — the buffer only measures distance to water, not elevation.

## Files in this repository

- `data/processed/waterway_buffer_200m.gpkg` — the dissolved 200m buffer, EPSG:32632
- `zaria_flood_buffer_200m.png` — exported map, showing the buffer against ward boundaries, with title, legend, north arrow, and scale bar
- `data-notes.md` — dataset quality notes for all layers used
