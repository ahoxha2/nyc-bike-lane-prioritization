# NYC Protected Bike Lane Prioritization

A GIS and Python project that ranks existing non-protected bicycle-route segments across Manhattan, the Bronx, Brooklyn, and Queens for potential protected bike-lane upgrades.

## Project Goal

Identify bike-route segments where protected infrastructure may have the strongest combined need based on safety, access, Vision Zero context, and proximity to the existing protected bike network.

## Final Maps

### 1. Final Priority Map
![Final Priority Map](outputs/maps/01_final_priority_map.png)

Shows the final Top 50 priority segments and the most robust segments identified across sensitivity scenarios.

### 2. Citywide Priority Scores
![Citywide Priority Scores](outputs/maps/02_citywide_priority_scores.png)

Shows the distribution of final priority scores across all 7,569 eligible non-protected bicycle-route segments.

### 3. Priority Ranking Robustness
![Priority Ranking Robustness](outputs/maps/03_priority_ranking_robustness.png)

Shows how consistently segments appeared in the Top 50 across the three weighting scenarios.

## Data

Sources used:

- NYC LION street centerlines
- NYC bicycle routes
- NYPD Motor Vehicle Collisions
- NYC school locations
- MTA subway entrances
- NYC Vision Zero Priority Corridors
- NYC borough boundaries

Spatial analysis CRS: **EPSG:2263 — NAD83 / New York Long Island (ftUS)**

## Workflow

```text
Source data
    ↓
Clean and standardize spatial data
    ↓
Identify existing non-protected bike-route segments
    ↓
Match crashes to LION street segments
    ↓
Apply candidate filters
    ↓
Calculate segment metrics
    ↓
Normalize metrics
    ↓
Calculate weighted priority score
    ↓
Rank 7,569 eligible segments
    ↓
Select Top 50
    ↓
Sensitivity analysis
```

## Candidate Rules

Segments were retained when they were:

- existing bicycle-route segments
- on-street
- non-protected
- linked to a valid LION SegmentID
- at least 150 ft long
- located in Manhattan, the Bronx, Brooklyn, or Queens

Final eligible segments: **7,569**

## Priority Score

| Factor | Weight |
|---|---:|
| Crash density | 30% |
| Pedestrian + cyclist injury density | 25% |
| School access | 15% |
| Subway access | 10% |
| Vision Zero priority | 10% |
| Protected-network connectivity | 10% |

Crash and vulnerable-road-user injury metrics were calculated per mile and capped at the 99th percentile before normalization.

### Metric definitions

- **Crash density:** crashes per mile of candidate segment
- **VRU injury density:** pedestrian + cyclist injuries per mile
- **School access:** schools within 500 ft
- **Subway access:** subway entrances within 500 ft
- **Vision Zero priority:** segment within 100 ft of a Vision Zero Priority Corridor
- **Protected-network connectivity:** number of segment endpoints within 100 ft of the existing protected bike network

## Results

- **7,569** eligible segments ranked
- **50** final priority segments selected
- **28** segments appeared in the Top 50 under all three weighting scenarios
- **21** appeared in two scenarios
- **24** appeared in one scenario

The 28 segments selected under all three scenarios form the most robust priority set.

## Sensitivity Analysis

Three scoring scenarios were compared:

- **Balanced**
- **Safety First**
- **Access / Network First**

| Scenario consistency | Segments |
|---|---:|
| Top 50 in all 3 scenarios | 28 |
| Top 50 in 2 scenarios | 21 |
| Top 50 in 1 scenario | 24 |

## Outputs

Main processed layers:

```text
bike_candidates_ranked_v2_2263.gpkg
top50_protected_bike_lane_priority_v2_2263.gpkg
robust_priority_segments_v2_2263.gpkg
top50_v2_vz_intersections_2263.gpkg
```

Main tables:

```text
top50_priority_segments_v2.csv
top10_priority_segments_v2.csv
sensitivity_summary_v2.csv
```

## Notes

- Crash events were matched to the nearest LION roadway segment with a maximum matching distance of **50 ft**.
- Protected-network connectivity is based on endpoint proximity within **100 ft** and is not a full network-topology measure.
- Subway access counts nearby entrances rather than unique station complexes.
- Automated traffic-volume coverage was too sparse for consistent citywide scoring, so traffic was kept out of the final weighted model.
- The crash heatmap is used for **visual context only** and is not part of the score.
- The ranking is a prioritization screen, not an engineering or construction recommendation.

## Repository Structure

```text
nyc-bike-lane-prioritization/
├── data/
│   ├── raw/
│   └── processed/
├── notebooks/
│   ├── 01_data_cleaning.ipynb
│   ├── 02_crash_segment_metrics.ipynb
│   ├── 03_candidate_segments.ipynb
│   ├── 04_spatial_metrics_connectivity.ipynb
│   ├── 05_scoring_ranking.ipynb
│   └── 06_sensitivity_final_outputs.ipynb
├── outputs/
│   ├── maps/
│   └── tables/
├── qgis/
├── .gitignore
└── README.md
```

## Tools

**Python:** pandas, NumPy, GeoPandas, Matplotlib  
**GIS:** QGIS  
**Spatial reference:** EPSG:2263
