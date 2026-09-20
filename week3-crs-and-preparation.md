# Week 3 — Coordinate systems and data preparation

## Project
Zaria flood exposure — which settlements in Zaria LGA, Kaduna State sit in low-lying land within 200 metres of a river or watercourse.

## CRS chosen, and why

All source data (GRID3 wards, OSM waterways, OSM highways) arrived in **EPSG:4326 (WGS 84)**, the standard geographic system almost everything downloads in.

I reprojected everything to **EPSG:32632 (UTM zone 32N)**. Zaria LGA's extent falls between roughly 7.49°E–7.83°E and 10.92°N–11.14°N, which sits inside UTM zone 32 (6°E–12°E), so 32632 is the correct projected zone for this area — matching the pack's guidance that centre and eastern Nigeria use EPSG:32632.

A geographic CRS (EPSG:4326) was not usable for the rest of this project because its units are degrees, not metres. Week 4's analysis depends entirely on distance (buffering settlements or wards against nearby watercourses), and a buffer distance in degrees is meaningless — a "500" buffer in EPSG:4326 would be interpreted as 500 degrees, far larger than the planet.

## What I reprojected and clipped

| Layer | Original CRS | Reprojected to | Clipped to study area | Feature count before → after |
|---|---|---|---|---|
| GRID3 Zaria Wards | EPSG:4326 | EPSG:32632 | N/A — this layer *is* the study area boundary | 13 → 13 |
| OSM Waterways (waterway=*) | EPSG:4326 | EPSG:32632 | Yes, clipped to study_area_wgs32632 | 113 → 97 |
| OSM Highways (highway=*) | EPSG:4326 | EPSG:32632 | Yes, clipped to study_area_wgs32632 | 9,010 → 6,920 |

Both layers lost features after clipping (highways: 2,090 fewer, ~23%; waterways: 16 fewer, ~14%), which shows the original QuickOSM "Layer Extent" query pulled in some features beyond Zaria LGA's actual ward boundary — a bounding-box extent is a rectangle, not the true LGA shape, so it always includes some overshoot at the corners. The clip step removed that overshoot and left only features genuinely inside Zaria LGA.

Reprojection was done with Vector → Data Management Tools → Reproject Layer. Clipping was done with Vector → Geoprocessing Tools → Clip, using the reprojected ward layer as the overlay. Both operations were run only after confirming all layers shared the same CRS, since a clip between mismatched systems returns nothing or something wrong.

## The five quality checks

### Completeness
- **Wards**: All 13 expected wards are present (Dambo, Dutsen Abba, Gyellesu, Kaura, Kufena, Kwarbai A, Kwarbai B, Limancin Kona, Tudun Wada, Tukur Tukur, Unguwar Fatika, Unguwar Juma, Wuciciri), matching what I could see against the settlement/road layout in QGIS.
- **Waterways**: 113 features returned before clipping, 97 after — includes named rivers (Galma, Saye) and many unnamed drains/streams. Not yet checked against a satellite basemap to confirm nothing significant is missing.
- **Highways**: 9,010 features before clipping, 6,920 after — visually dense in the built-up core, visibly sparse toward rural edges (Dutsen Abba, Wuciciri).

### Currency
- **Wards**: dated 2026-06-30 in the data itself (source: CIESIN) — current.
- **Waterways/Highways**: extracted 12 September 2026 directly from live OSM via QuickOSM, so as current as OSM's own editing history for this area allows. Individual feature edit dates not yet checked.

### Positional accuracy
- Not yet checked against satellite imagery for any layer. Flagged as outstanding.

### Attribute accuracy
- **Wards**: no nulls in the `ward` column; `ward_alt_names` records legitimate spelling variants (e.g. "D/Abba" for Dutsen Abba) — useful for future name-based joins.
- **Waterways**: found a naming inconsistency — the same river appears tagged as both "River Galma" and "Galma River" in two separate features. Not yet fixed; flagged as a known issue for any analysis that groups or counts by name.
- **Highways**: `surface` tag present on ~82% of features (7,355 of 9,010); attributes like `bridge`, `junction`, `name`, `lanes` are null in the large majority of rows — expected for Nigerian OSM data per the pack's own note on inconsistent tagging.

### Fitness for purpose
- **Wards**: fit for defining the study area and for any ward-level aggregation.
- **Waterways**: fit for proximity-based flood exposure analysis, with the caveat that some watercourses (e.g. Saye River) are tagged `intermittent=yes` — meaning they only carry water seasonally, which matters directly for a flood-timing question.
- **Highways**: not one of the project's four core datasets; retained as a possible later extension (e.g. emergency access to flood-exposed settlements), not used in the current analysis.

## Problems found, and whether fixed or flagged

1. **Ward area discrepancy** — summing `area_sqkm` across all 13 wards gives ≈422 km², lower than some published figures for "Zaria" (~560+ km²). Likely explanation: those figures may describe the broader four-LGA Zaria metropolitan area (Zaria, Sabon Gari, Giwa, Soba) rather than Zaria LGA alone. **Flagged, not resolved** — noted as an open question rather than assumed either way.
2. **Inconsistent river naming** ("River Galma" vs "Galma River") — **flagged, not yet fixed**. Will need correcting before any name-based grouping in later analysis.
3. **Sparse attribute tagging on highways** (bridge, junction, name, lanes mostly null) — **flagged as a known limitation**, not fixed, since highways aren't a core dataset for this question.
4. **CRS measurement behaviour** — area calculated on the ward layer while still in EPSG:4326 gave correct real-world values (matching the reprojected result almost exactly), rather than the meaningless "square degrees" result the course material describes. This is because this QGIS project has ellipsoidal (geodesic) area measurement enabled at the project level, which computes true ground area directly from lat/long. **Noted, not a problem to fix** — but a reminder that this protection does not extend to distance-based operations like buffering, which still require a projected CRS. Reprojection was carried out regardless, since it is required for Week 4.

## Where the analysis-ready files live

- `data/processed/study_area_wgs32632.gpkg` — Zaria LGA wards, reprojected to EPSG:32632
- `data/processed/Water_ways_Reprojected.gpkg` — OSM waterways, reprojected to EPSG:32632
- `data/processed/waterways_zaria_clipped.gpkg` — waterways, clipped to study area and reprojected
- `data/processed/Zaria_Highway.gpkg` — OSM highways, reprojected to EPSG:32632
- `data/processed/highway_zaria_clipped.gpkg` — highways, clipped to study area and reprojected

All raw downloads remain untouched in `data/raw/`.
