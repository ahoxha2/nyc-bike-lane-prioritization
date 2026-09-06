# Notebook workflow

Run the notebooks in order:

1. **01_data_cleaning.ipynb** — cleans automated traffic counts and NYPD crash records.
2. **02_crash_segment_metrics.ipynb** — resolves nearest-LION crash matches, applies the 50-ft cutoff, and creates segment-level crash metrics.
3. **03_candidate_segments.ipynb** — identifies on-street non-protected bicycle-route candidates and joins them to unique LION geometry.
4. **04_spatial_metrics_connectivity.ipynb** — adds crash, supplementary traffic, school, subway, Vision Zero, length, and protected-network endpoint-proximity metrics.
5. **05_scoring_ranking.ipynb** — calculates capped/normalized indicators, the final six-factor balanced score, and the ranked Top 50.
6. **06_sensitivity_final_outputs.ipynb** — runs Balanced, Safety First, and Access/Network First sensitivity scenarios and writes final tables/layers.

## Required local inputs

The workflow assumes the repository structure already used by the project.

Raw CSVs:
- `data/raw/Automated_Traffic_Volume_Counts_20260905.csv`
- `data/raw/Motor_Vehicle_Collisions_-_Crashes_20260902.csv`

Prepared EPSG:2263 spatial layers:
- `data/processed/bike_routes_2263_nyc.gpkg`
- `data/processed/lion_2263.gpkg`
- `data/processed/schools_2263.gpkg`
- `data/processed/subway_enterances_2263.gpkg`
- `data/processed/vzv_priority_corridors_2263.gpkg`

Crash-to-LION nearest-join export:
- preferred: `data/interim/crashes_with_segment_ids.csv`
- development fallback: `docs/crashes_with_segment_ids.csv`

The crash-to-LION join was produced in QGIS. The notebook documents and reproduces the tie-resolution and 50-ft filtering performed after that join.

## Main final model

Balanced score:
- 30% crash density
- 25% vulnerable-road-user injury density
- 15% school access
- 10% subway access
- 10% Vision Zero context
- 10% protected-network endpoint proximity

Traffic is retained as supplementary context and is not included in the citywide score because candidate coverage is sparse.
