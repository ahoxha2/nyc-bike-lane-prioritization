<<<<<<< Updated upstream
## Traffic Volume Processing

Traffic exposure was derived from the **NYC Automated Traffic Volume Counts** dataset for **2022–2026**. The analysis was restricted to **Manhattan, Bronx, Brooklyn, and Queens**, consistent with the project study area.

The source dataset contains traffic-volume observations recorded at **15-minute intervals**, with each observation associated with a `SegmentID`, date, direction, street location, and point geometry.

### Time-of-Day Aggregation

For this project, traffic observations were restricted to **07:00–23:59** and grouped into three time periods:

| Period | Time |
|---|---|
| Morning | 07:00–11:59 |
| Afternoon | 12:00–17:59 |
| Evening | 18:00–23:59 |

The 15-minute `Vol` observations were summed within each period for each roadway segment, date, and observed travel direction.

Expected interval counts were:

- Morning: 20 observations
- Afternoon: 24 observations
- Evening: 24 observations

This allowed incomplete monitoring periods to be identified before the traffic data were used for analysis.

### Directional Traffic

Traffic direction was preserved during the initial aggregation. For example, northbound and southbound observations for the same roadway segment were initially treated as separate records.

Observed directions were then summed for each `SegmentID`, date, and time period to create the traffic totals used in the daily dataset.

Because traffic-count coverage is not uniform across NYC, not every segment was monitored in both directions. Two QA fields were therefore retained:

- `directions_n` — number of observed traffic directions
- `direction_coverage` — identifies records with one, two, or three observed directions

This prevents a one-direction traffic count from being incorrectly interpreted as a complete two-way roadway count.

### Daily Traffic Dataset

The three time-period totals were pivoted into a single record for each segment and date:

- `morning` — observed traffic volume from 07:00–11:59
- `afternoon` — observed traffic volume from 12:00–17:59
- `evening` — observed traffic volume from 18:00–23:59
- `total_7_24` — sum of the three periods, representing observed traffic from 07:00–23:59

The raw observed traffic counts are retained in the processed dataset. Mean, median, percentile, normalization, or other statistical transformations can therefore be calculated separately for subsequent analysis without replacing the underlying observations.

### Quality Control

Several QA steps were performed before creating the final dataset:

1. Records were restricted to 2022–2026 and the four study-area boroughs.
2. Duplicate 15-minute records were removed.
3. Traffic volume (`Vol`) was converted to numeric format.
4. Morning, afternoon, and evening observations were aggregated separately.
5. Records missing one or more of the three required time periods were excluded.
6. Zero-volume daily records were inspected and excluded from the final analytical dataset as likely data-quality artifacts.

The daily aggregation initially produced **2,764 segment-date records**.

- **2,736** had all three required time periods.
- **28** incomplete segment-date records were excluded.
- **16** zero-volume records were subsequently excluded.
- **2,720** valid segment-date records remained in the final processed dataset.

### Final Output

The processed dataset is stored at:

`data/processed/traffic_daily_2022_2026.csv`

Final fields include:

| Field | Description |
|---|---|
| `RequestID` | Traffic-count request identifier |
| `Boro` | Borough |
| `Date` | Observation date |
| `Yr` | Year |
| `M` | Month |
| `D` | Day |
| `SegmentID` | NYC roadway segment identifier |
| `WktGeom` | Traffic-count point geometry |
| `street` | Street name |
| `fromSt` | Beginning cross street |
| `toSt` | Ending cross street |
| `morning` | Raw observed traffic volume, 07:00–11:59 |
| `afternoon` | Raw observed traffic volume, 12:00–17:59 |
| `evening` | Raw observed traffic volume, 18:00–23:59 |
| `total_7_24` | Total observed traffic volume, 07:00–23:59 |
| `directions_n` | Number of observed travel directions |
| `direction_coverage` | Directional coverage QA category |

> **Important:** `total_7_24` represents the sum of the traffic directions actually observed for that segment-date. It should not automatically be interpreted as a complete two-way roadway traffic count when `direction_coverage` indicates only one observed direction.
=======
\#

this folder contains processed ds

>>>>>>> Stashed changes
