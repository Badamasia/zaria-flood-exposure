## GRID3 NGA Operational Wards v3.0 — Zaria LGA
- Source: https://data.grid3.org
- Downloaded: 12 September 2026
- 13 features, polygons — one per ward in Zaria LGA (Dambo, Dutsen Abba, Gyellesu, Kaura, Kufena, Kwarbai A, Kwarbai B, Limancin Kona, Tudun Wada, Tukur Tukur, Unguwar Fatika, Unguwar Juma, Wuciciri)
- Columns: OBJECTID, country, iso3, state, statecode, lga, lga_alt_names, ward, ward_alt_names, ward_v1_grid3, rd_in_grid3_ward_l, multipart_count, source, date, area_sqkm
- No nulls in ward column
- ward_alt_names records spelling variants (e.g. "D/Abba" for Dutsen Abba) — worth using this if joining any external spreadsheet by ward name later
- Source: CIESIN, dated 2026-06-30
- Sum of area_sqkm across all 13 wards ≈ 422 km² — lower than some published figures for "Zaria" (~560 km²), which may describe the broader four-LGA Zaria metropolitan area rather than Zaria LGA alone. Noted as an open question, not yet resolved.
- Covers Zaria LGA fully — ward count and names match what I expected
  
## OSM waterways (waterway=*), extracted via QuickOSM
- Query: waterway=* within Zaria LGA extent
- Extracted: 12 September 2026
- 113 features, lines
- Mix of types: dam, drain (mostly tagged tunnel=culvert), river, stream
- name tag present on very few features; where present, inconsistent — "River Galma" and "Galma River" appear to be the same watercourse tagged two different ways
- intermittent=yes on at least one named river (Saye River), meaning it only flows seasonally — directly relevant to flood-risk timing
- surface tag is present on 97 features out of 113
- Coverage against known rivers in Zaria looks good against a satellite basemap
