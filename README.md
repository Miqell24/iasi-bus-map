# Iași Public Transport — interactive map

Interactive, poster-grade map of the public transport network of
**Iași**: Societatea de Transport Public Iași (SCTP)'s buses and trams — 53 lines drawn along the
real street and track geometry.

## Live

Not published — this map is built and reviewed locally.

One feed covers everything, split by `route_type` at build time:

| mode | route_type | lines | graph |
|---|---|---|---|
| buses | 3 | city and metropolitan lines | OSM roadways |
| trams | 0 | 1, 3, 5, 6, 7, 8, 9, 11 and 13 | `railway=tram` tracks |

Iași has **no metro**, so the engine's metro treatment stays unused.

Build quirks worth knowing:

* **Trams 1 and 13 are circular** — both start and end at Copou and the feed gives them a single direction. They are drawn once round, not twice; the shape trim searches each anchor in its own half of the shape so a loop cannot collapse onto its own start (the defect that cost Belgrade's line 67 fourteen kilometres).
* **Line numbers are unique across the modes**, so the line keys are the bare
  numbers printed on the vehicles — none of the mode prefixes the Sofia sibling
  needs. Re-check on every feed refresh.
* **Romanian is written in the Latin alphabet**, so this map runs without the
  second, transliterated label line its Greek, Bulgarian and Serbian siblings
  carry, and the stop names arrive properly cased and accented from the
  operator.
* **The feed's own `route_color` is ignored**, as everywhere in this family:
  colour means the MODE — navy bus, green trolleybus, red tram.

## Pipeline

`npm run download` fetches the GTFS, the OSM roadways, the tram tracks and
MapLibre GL. `npm run build` map-matches every line (HMM/Viterbi on the OSM
graphs) and writes GeoJSON to `data/out/`. `npm run serve` hosts the map at
http://localhost:8147.

Data: Societatea de Transport Public Iași (SCTP) · base map © OpenFreeMap / OpenMapTiles / OpenStreetMap
contributors.
