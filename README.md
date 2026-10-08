# Zaria flood exposure project - Badamasi Annas, pod 4

Which settlements in Zaria Local Government Area, Kaduna State sit in
low-lying land within 200 metres of a river or watercourse?

Built over twelve months with GeoDev Lab Africa, Cohort One.

## Answer, so far

A 200m buffer around every mapped river, stream, and drain in Zaria LGA
forms a continuous flood-exposure corridor that passes through **11 of
the LGA's 13 wards**: Dambo, Dutsen Abba, Gyellesu, Kaura, Kufena,
Kwarbai A, Kwarbai B, Tudun Wada, Tukur Tukur, Unguwar Fatika, and
Wuciciri. Only Limancin Kona and Unguwar Juma fall entirely outside the
200m zone, despite sitting in the dense cluster of wards at the centre
of the LGA. This is a real result, produced by intersecting the ward
boundaries with the buffer in QGIS, not a demonstration — see Week 4
below for the full method and map.

It answers *where* the flood-risk zone falls. It does not yet answer
*which settlements* sit inside it, since settlement extent data is
still outstanding — see "What's still missing" below.

## Month 1 — Orientation and GIS foundations

- **Week 1 — Project brief**: [project-brief.md](./project-brief.md) —
  the question, why it matters, and a source link for every dataset used.
- **Week 2 — Data notes**: [data-notes.md](./data-notes.md) — what was
  downloaded, feature counts, columns, and known issues for each dataset.
- **Week 3 — CRS and preparation**:
  [week3-crs-and-preparation.md](./week3-crs-and-preparation.md) — CRS
  chosen and why, what was reprojected and clipped, and the five quality
  checks run on every layer.
- **Week 4 — Analysis**:
  [month-1-summary.md](./month-1-summary.md) — the buffer operation run,
  what was expected versus what came out, and what surprised me. Map
  image: [zaria_flood_buffer_200m.png](Zaria_Waterways_Buffer.png)

## What's still missing

- **Settlement extents** for Zaria LGA — needed to say which specific
  settlements, not just which wards, fall inside the flood-exposure zone
- **Elevation (DEM)** — needed to add the "low-lying land" part of the
  original question; the buffer alone only measures distance to water,
  not elevation

## Data

All processed, analysis-ready files are in `data/processed/`. Raw
downloads are untouched in `data/raw/`.

[README.md](https://github.com/user-attachments/files/31837391/README.md)
  
   ## Month 2: development environment and early Python
   - Week 5: set up Python, VS Code and the terminal. hello.py runs.
   -  Week 6: set up the project with uv and added pandas. check.py prints the pandas version.
