# Data Note — How Many Amenity Points Fall Within the Tabata Ward Boundary?

## Question and answer
**Question:** How many amenity points fall within the Tabata ward boundary?

**Answer: 65 of 65.** Every amenity point in the layer falls inside the boundary (`NUMPOINTS = 65`, 0 outside).

## Data
- `amenity.gpkg` — 65 amenity points (pharmacy, school, restaurant, church, clinic, bus stop, etc.), EPSG:32737
- `count.gpkg` — Tabata ward boundary (1 MULTIPOLYGON feature, Ilala / Tabata) with the `NUMPOINTS` field added by the count operation, EPSG:32737
- `tabata_analysis_ready.gpkg` — both layers in one file: `amenity` (65 points) and `boundary` (1 polygon)
- Original source of the amenity points and boundary: *(add here: e.g. OSM export / ward boundary provider)*

## CRS
**EPSG:32737 — WGS 84 / UTM zone 37S.** Correct zone for Dar es Salaam (central meridian 39°E) and in metres. Both layers were already stored in it, so no reprojection was needed. QGIS project CRS is also EPSG:32737.

## How I did it

### 1. The operation: Count Points in Polygon
In QGIS: Processing Toolbox → Vector analysis → **Count points in polygon**.
- Polygons: `tabata_boundary` [EPSG:32737]
- Points: `amenity`
- Weight field and class field: left empty
- Count field name: `NUMPOINTS`
- Output saved as `count.gpkg`

The output is the boundary polygon with one added column, `NUMPOINTS`, holding the number of amenity points inside it.

### 2. What I expected before running it
- **Features:** 1 output feature, because there is a single ward polygon.
- **Value:** `NUMPOINTS` at or close to 65, the number of points in the amenity layer. The layer covers Tabata ward, and an earlier schools-only version of this question had every school inside the boundary, so I expected all or nearly all points to fall inside, with at most a few stragglers near the edge.

### 3. Checking the result four ways
1. **Look at the map.** Boundary and points plotted together (`tabata_amenity_count_map.png`, and the QGIS layout `task04.png`): all 65 points sit inside the outline, none on or across the edge.
2. **Row count against expectation.** The output has 1 row, as expected, and `NUMPOINTS = 65.0`, matching the 65 rows in `amenity`. I also re-ran the count independently with a point-in-polygon script (`run_analysis_all_amenities.py`): 65 inside, 0 outside, same as QGIS.
3. **Verify one feature by hand.** I picked the single `college` point (fid 676) at E 524,920.9 / N 9,245,716.8. Drawing a horizontal ray east from the point, it crosses the boundary outline once (at E 526,441.3): an odd number of crossings means the point is inside. Going west it crosses 3 times, also odd, so inside. Its position (north-central part of the ward) also matches where the college icon sits on the map.
4. **Look for empty geometry.** 0 empty or null geometries in `amenity` (65 rows) and in the boundary (1 row). The boundary ring is closed (297 vertices, first vertex equals last).

### 4. The map
Made in a QGIS print layout and exported as PNG (`task04.png`): title, legend (amenity categories, road classes, `count(65)` and `tabata_boundary`), scale bar, north arrow, coordinate grid, datum and projection note. A simpler version showing only the boundary and the 65 points, also with title, legend and scale bar, is `tabata_amenity_count_map.png`. Both are in the repository.

## Data quality checks
1. **Coordinate system:** both layers tagged 32737 and the coordinate values are consistent with UTM 37S. ✅
2. **Empty values:** `amenity` category populated for all 65; `Name` empty for 30 of 65; `No label` empty for 57 of 65. ⚠️
3. **Duplicates:** 0 duplicate `fid`s and 0 duplicate coordinate pairs. ✅
4. **Geometry validity:** no empty geometries, boundary ring closed. ✅
5. **Coverage:** the amenity points' extent (523,876–525,702 E / 9,243,712–9,246,204 N) lies inside the boundary extent (523,596–526,475 E / 9,243,649–9,246,428 N), and all points fall inside the polygon itself. ✅

## Problems found, and whether fixed or flagged
- **`xcoord` / `ycoord` columns don't match the point geometry.** Every one of the 65 rows is offset by the same amount (geometry minus attribute = +95.93 E, −301.80 N), so those columns look stale. **Flagged, not fixed.** The count used the geometry, so the result is unaffected, but don't use these two columns for coordinate work without recomputing them.
- **Inconsistent category values:** `dispensary` and `Dispensary` are separate values, `kindergaten` is misspelled, one value is Swahili ("Ofisi ya serikali ya mtaa"), and "Industry" is vague. **Flagged, not fixed.** The total count is unaffected, but a count by category would need these cleaned first.
- **`No label` is mostly empty (57 of 65).** **Flagged, not fixed.**

## Files in the repository
`README.md`, `data_note.md`, `tabata_analysis_ready.gpkg`, `amenity.gpkg`, `count.gpkg`, `gpkg_lib.py`, `run_analysis_all_amenities.py`, `tabata_amenity_count_map.png`, `task04.png`
