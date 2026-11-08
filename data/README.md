# Data

All files are CC BY 4.0 (see `../LICENSE-DATA` and `../NOTICE`). Coordinates are WGS 84 longitude and latitude.

## geometry/

The City of Melbourne's 603 CLUE blocks, exported from Plotlay. `key` is the block's `block_id`. Each carries its
`area_name` and `region_name`, `area_m2`, `source`, and the formula columns worked out in Plotlay:
`office_2002`, `office_2024`, `office_change`, `apartments_*`, `hotels_*`, `student_*`, `total_*`,
`homes_hotels_students_change` and `office_to_living` (1 where office fell and homes, hotels or student rooms rose).
An empty change means a year's figure is held back by the census.

- `blocks.geojson`: for Azure Maps, QGIS and Python. `blocks.topojson`: for the Power BI Shape Map. `blocks_wkt.csv`: one row per block with its outline as WKT and its centre.

## clue/

- `floor_space_by_use.csv`: the census columns Plotlay linked (one row per block and year, 2002 to 2024): `office`,
  `residential_apartment`, `commercial_accommodation` (hotels and serviced apartments), `student_accommodation`,
  `total_floor_space_in_block`. Blank cells are held back by the census (two businesses or fewer).
- `block_names.csv`: a name for each block from the streets round it (the council's road segments).

The census table was linked in Plotlay from this address:

```
https://data.melbourne.vic.gov.au/api/explore/v2.1/catalog/datasets/floor-space-by-use-by-block/exports/csv?select=block_id%2Ccensus_year%2Cclue_small_area%2Coffice%2Cresidential_apartment%2Ccommercial_accommodation%2Cstudent_accommodation%2Ctotal_floor_space_in_block&delimiter=%2C
```
