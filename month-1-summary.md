# Month 1 Summary

## Question

How many amenity points fall within the Tabata ward boundary (Ilala Municipality, Dar es Salaam)?

## Operation

Ran **Count points in polygon** in QGIS (Processing Toolbox → Vector analysis), with `tabata_boundary` as the
polygon layer and `amenity` as the point layer. All layers are in EPSG:32737 (WGS 84 / UTM zone 37S), so no
reprojection was needed.

I chose it because the question is a straight "how many points fall inside this polygon" count, which this tool
answers directly. It adds a `NUMPOINTS` field to a copy of the boundary, so the result is easy to check against
the input layer.

## Expected

One output feature (there is a single ward polygon), with `NUMPOINTS` at or close to 65, the number of points in
the amenity layer. An earlier schools-only version of the question had every school inside the boundary, so I
expected all or nearly all points inside, with at most a few near the edge.

## Got

* 1 output feature, `NUMPOINTS = 65`. All 65 of 65 amenity points are inside the boundary and 0 are outside.
* Four checks agreed with the result:
  * The map shows every point inside the outline, none on or across the edge.
  * The row count matches (1 row, and 65 points in the input layer).
  * A hand check on the `college` point (ray casting, odd number of crossings) confirmed it is inside.
  * There are 0 empty or null geometries in any layer, and the boundary ring is closed (297 vertices).

## What surprised me

* **No points near the edge at all.** I expected all or nearly all points inside, with "at most a few near the
  edge". I got 65 of 65 with none close to the boundary line in the map check, which is a cleaner result than I
  predicted.
* **Even the hand check was easy.** The `college` point gave an odd number of ray crossings in both directions
  (3 going west), so it was clearly inside. Nothing forced me to think about a borderline case.

## What I still need

* A confirmed full ward boundary. The boundary's `Subward` value is "Tabata", which may mean one subward's
  polygon is standing in for the whole ward. Tabata Ward has several subwards (Mandela, Matumbi, Msimbazi,
  Mtambani, Tenge, Kisiwani and others).
* An amenity layer pulled independently of the ward boundary (a wider bounding box, clipped afterwards), so the
  count can actually fail and test whether the ward is fully covered.
* The breakdown of the 65 points by amenity category, so the count says what kinds of amenities exist and not
  only how many.

## Sources

* Amenity points and roads: OpenStreetMap (https://www.openstreetmap.org/), QuickOSM; roads also from HOTOSM
  (https://export.hotosm.org/v3/)
* Ward boundary: HCMGIS plugin in QGIS
* Working file: `tabata_analysis.gpkg` (layers `amenity`, `tabata_boundary`, `count`, `roads`)
