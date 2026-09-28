# Counting Amenities Within the Tabata Ward Boundary

All data for this analysis is in `tabata_analysis.gpkg`, in EPSG:32737 (WGS 84 / UTM zone 37S).

**Research question:** How many amenity points fall within the Tabata ward boundary?

**Answer:** 65 of 65. Every amenity point is inside the boundary (`NUMPOINTS = 65`, 0 outside), confirmed by QGIS's Count Points in Polygon 

**Layers in `tabata_analysis.gpkg`:**
- `amenity`: 65 amenity points (POINT)
- `tabata_boundary`: Tabata ward boundary, the polygon used for the count (MULTIPOLYGON)
- `count`: QGIS output, the boundary with the `NUMPOINTS` field added (MULTIPOLYGON)
- `roads`: 564 road segments in the ward, used for map context (MULTILINESTRING)

**Other files:**
- `data_note.md`: CRS, how the count was run, the prediction, the four result checks, the map, data quality checks and problems found
- `04.qgz`: QGIS project, with its layers pointed at `tabata_analysis.gpkg`
- `run_analysis_all_amenities.py`, `gpkg_lib.py`: independent Python re-run of the count (`python3 run_analysis_all_amenities.py`)
- `tabata_amenity_count_map.png`: simple result map (title, legend, scale bar, north arrow)
- `task04.png`: full QGIS layout map (title, legend, scale bar, north arrow, coordinate grid)

**Sources:** amenity points, derived from OpenStreetMap ( https://www.openstreetmap.org/ ), roads from a HOTOSM export ( https://export.hotosm.org/v3/ ) and ward boundary i got it from HCMGIS plugin in QGIS
