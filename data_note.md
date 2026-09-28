
**Question:** How many amenity points fall within the Tabata ward boundary?

**Answer: 65 of 65.** Every amenity point falls inside the boundary (`NUMPOINTS = 65`, 0 outside).

## Data
Everything is combined in a single GeoPackage, **`tabata_analysis.gpkg`**.

| Layer | Features | Geometry | Role |
|---|---|---|---|
| `amenity` | 65 | POINT | the points being counted |
| `tabata_boundary` | 1 | MULTIPOLYGON | the ward polygon (Ilala / Tabata) |
| `count` | 1 | MULTIPOLYGON | QGIS output: boundary plus the `NUMPOINTS` field |
| `roads` | 564 | MULTILINESTRING | map context, not used in the count |

## CRS
**EPSG:32737, WGS 84 / UTM zone 37S.** Correct zone for Dar es Salaam  and in metres. All four layers were already stored in it, so no reprojection was needed.

## How I did it

### 1. The operation: Count Points in Polygon
In QGIS: Processing Toolbox → Vector analysis → **Count points in polygon**.
- Polygons: `tabata_boundary` [EPSG:32737]
- Points: `amenity`
- Weight field and class field: empty
- Count field name: `NUMPOINTS`
- Output saved as `count`

The output is the boundary polygon with one added column, `NUMPOINTS`, holding the number of amenity points inside it.

### 2. What I expected before running it
- **Features:** 1 output feature, because there is a single ward polygon.
- **Value:** `NUMPOINTS` at or close to 65, the number of points in the amenity layer. The layer covers Tabata ward, and an earlier schools-only version of this question had every school inside the boundary, so I expected all or nearly all points inside, with at most a few near the edge.

### 3. Checking the result four ways
1. **Look at the map.** Boundary and points plotted together: all 65 points sit inside the outline, none on or across the edge.
2. **Row count against expectation.** The output has 1 row, as expected, and `NUMPOINTS = 65`, matching the 65 rows in `amenity`.
3. **Verify one feature by hand.** I picked the `college` point . A horizontal ray east from it crosses the boundary outline. An odd number of crossings means inside. Going west it crosses 3 times, also odd, so inside. Its position (north-central ward) matches where the college icon sits on the map.
4. **Look for empty geometry.** 0 empty or null geometries in `amenity` (65), `roads` (564), `tabata_boundary` (1) and `count` (1). The boundary ring is closed (297 vertices, first equals last) and `count` has the same geometry as `tabata_boundary`.

### 4. The map
Made in a QGIS print layout and exported as PNG (`tabata_amenity_count_map.png`): title, legend (amenity categories, road classes, `count(65)`, `tabata_boundary`), scale bar, north arrow, coordinate grid, datum and projection note.

## Files in the repository
`README.md`, `data_note.md`, `tabata_analysis.gpkg`, `tabata_amenity_count_map.png`
