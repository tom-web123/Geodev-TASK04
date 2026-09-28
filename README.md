# Counting Amenities Within the Tabata Ward Boundary

This repository contains Tabata ward's amenity points (schools, pharmacies, clinics, and more) and ward boundary, both in EPSG:32737 (WGS 84 / UTM zone 37S), with a point-in-polygon count run against the boundary.

**Research question:** How many amenity points fall within the Tabata ward boundary?

**Contents:**
- `tabata_analysis_ready.gpkg` — analysis-ready GeoPackage (`amenity` point layer, `boundary` polygon layer carrying the `NUMPOINTS` result)
- `data_note.md` — CRS decisions, what was run and why, and the five quality checks
- `run_analysis_all_amenities.py` / `gpkg_lib.py` — independent Python re-run of the point-in-polygon count, for cross-checking the QGIS result
- `tabata_amenity_count_map.png` — result map (title, legend, scale bar, north arrow)
- `task04.png` — full printed QGIS layout map (all amenity categories, roads, streets)

**Result:** 65 of 65 amenity points fall inside the boundary — confirmed by both QGIS's own *Count Points in Polygon* algorithm and an independent Python check. Full write-up, prediction, and four-way verification (map / row count / hand check / empty-geometry check) in `data_note.md`.

**Sources:** amenity points and ward boundary derived from OpenStreetMap and locally digitized ward-level administrative data for Tabata, Ilala Municipality, Dar es Salaam.
