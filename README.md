# Dortmund StadtRegion Indicators

**Live interactive map:**  
https://sophia-chehre.github.io/dortmund-stadtregion-indicators/

A small open-source Python GIS project for exploring indicator-based spatial monitoring in the Dortmund city-region context.

The project combines municipal-level indicator data from INKAR 2025 with official administrative boundaries from BKG VG250. The final output is an interactive web map with four toggleable indicator layers for 33 municipalities in and around Dortmund.

---

## Demo scope

For this demo, the Dortmund StadtRegion is defined pragmatically as:

- Stadt Dortmund
- Stadt Bochum
- Stadt Hagen
- Stadt Herne
- Kreis Unna
- Kreis Recklinghausen
- Ennepe-Ruhr-Kreis

This results in a study area of 33 Gemeinden.

This project is not intended to fully replicate the official ILS Monitoring StadtRegionen methodology. It is a compact and reproducible demo for cleaning, joining, mapping, and publishing municipal indicators using open spatial and statistical data.

---

## What the interactive map shows

The interactive map contains four 2023 indicators at Gemeinde level:

| Indicator column | INKAR indicator | Meaning |
|---|---|---|
| `population_change_5y_pct` | Bevölkerungsentwicklung (5 Jahre) | demographic change |
| `share_65plus_pct` | Einwohner 65 Jahre und älter | demographic ageing |
| `longterm_unemployed_share_pct` | Langzeitarbeitslose | labour-market exclusion |
| `employment_rate_pct` | Beschäftigtenquote | labour-market integration |

The map allows users to:

- switch between the four indicator layers,
- hover over each municipality to view all indicator values,
- explore spatial differences within the Dortmund StadtRegion,
- open the map as a standalone GitHub Pages web map.

---

## Key observation

The indicators show different levels of spatial variation.

The share of population aged 65+ varies relatively narrowly across the StadtRegion, from about 20.4% to 27.0%. In contrast, the share of long-term unemployed people varies much more strongly, from about 29.6% to 59.2%.

This suggests that demographic ageing is broadly present across the region, while labour-market exclusion is much more spatially uneven between municipalities.

---

## Method

The workflow follows these main steps:

1. Inspect raw INKAR 2025 data.
2. Select indicators available at Gemeinde level for 2023.
3. Convert INKAR from long format to wide format.
4. Preserve `AGS` as an 8-character string for reliable joins.
5. Load BKG VG250 Gemeinde geometries.
6. Filter geometries to NRW.
7. Join indicator values to Gemeinde geometries using `AGS`.
8. Save cleaned outputs as CSV and GeoPackage.
9. Create static choropleth maps with GeoPandas and Matplotlib.
10. Create an interactive web map with Folium and Branca.
11. Publish the interactive map through GitHub Pages.

---

## Technical details

- **Statistical data:** INKAR 2025, BBSR
- **Geometries:** BKG VG250, Gemeinde boundaries
- **Join key:** `AGS` / Amtlicher Gemeindeschlüssel
- **Analytical CRS:** EPSG:25832
- **Web map CRS:** EPSG:4326
- **Static maps:** GeoPandas, Matplotlib, Mapclassify
- **Interactive map:** Folium, Branca, Leaflet
- **Publishing:** GitHub Pages from `/docs`

The project keeps the processed GIS data in EPSG:25832 for analysis and only reprojects to EPSG:4326 at the final web-mapping step, because Folium and Leaflet expect latitude/longitude coordinates.

---

## Indicator selection notes

The initial candidate indicators included `Arbeitslosenquote` and SGB II-related indicators because they are relevant for labour-market and social disadvantage.

However, the INKAR 2025 metadata showed that some of these indicators were not available at Gemeinde level for the selected geography and year. To avoid mixing spatial levels, the final demo uses four indicators that are available for all 33 municipalities in 2023.

No composite index was created. Combining indicators with different meanings, directions, and scales would require explicit weighting decisions. For this demo, separate maps and a comparison table are more transparent.

---

## Repository structure

```text
.
├── data/
│   ├── raw/              # raw downloaded data
│   └── processed/        # cleaned CSV and GeoPackage outputs
├── docs/
│   └── index.html        # GitHub Pages version of the interactive map
├── notebooks/
│   ├── 01_setup_check.ipynb
│   ├── 02_data_inspection.ipynb
│   ├── 03_data_cleaning.ipynb
│   ├── 04_first_choropleth.ipynb
│   ├── 05_all_indicators.ipynb
│   └── 06_interactive_map.ipynb
├── output/
│   ├── 01_population_change_stadtregion.png
│   ├── 02_share_65plus_stadtregion.png
│   ├── 03_longterm_unemployed_stadtregion.png
│   ├── 04_employment_rate_stadtregion.png
│   ├── dortmund_stadtregion_map.html
│   └── ranking_table.csv
└── └── └── README.md
```

---

## Main outputs

- Interactive map: `docs/index.html`
- Local HTML map: `output/dortmund_stadtregion_map.html`
- Static PNG maps: `output/*.png`
- Ranking table: `output/ranking_table.csv`
- Joined StadtRegion GeoPackage: `data/processed/stadtregion_indicators_2023.gpkg`

---

## Limitations

This is a compact demo, not a full monitoring system.

Main limitations:

- It uses one reference year, 2023.
- It covers one defined demo region.
- It uses four indicators only.
- It does not include time-series animation.
- It does not create a composite index.
- The StadtRegion definition is pragmatic and based on selected AGS prefixes.

Possible extensions include adding more years, expanding the workflow to other StadtRegionen, adding environmental indicators, or developing a more formal indicator dashboard.

---

## Data sources

- **INKAR 2025** — Bundesinstitut für Bau-, Stadt- und Raumforschung (BBSR)
- **BKG VG250** — Bundesamt für Kartographie und Geodäsie

---

## Author

Sophia (Zahra) Chehreghan  
M.Sc. Spatial Planning, TU Dortmund  
Portfolio: https://sophia-chehre.github.io/  
GitHub: https://github.com/sophia-chehre