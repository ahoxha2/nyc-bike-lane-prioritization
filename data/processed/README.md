## Data Preparation

### Automated Traffic Volume Counts

Traffic exposure was derived from the **NYC Automated Traffic Volume Counts** dataset.

The raw dataset contains 15-minute traffic observations with fields including:

- `RequestID`
- `Boro`
- `Yr`
- `M`
- `D`
- `HH`
- `MM`
- `Vol`
- `SegmentID`
- `WktGeom`
- `street`
- `fromSt`
- `toSt`
- `Direction`

The analysis was limited to:

- Manhattan
- Bronx
- Brooklyn
- Queens
- Years 2022–2026

Staten Island was excluded from the study area.

Duplicate 15-minute records were removed using the combination of:

`RequestID + SegmentID + date + Direction + HH + MM`

Traffic counts were grouped into three time periods:

- **Morning:** 07:00–11:59
- **Afternoon:** 12:00–17:59
- **Evening:** 18:00–23:59

For each segment, date, street location, and direction, the 15-minute observations were summed into period totals.

Observed directions were then combined for each segment-date-time period. The number of observed directions was retained so that one-direction counts are not interpreted as complete two-way traffic totals.

The final traffic dataset contains:

- `morning`
- `afternoon`
- `evening`
- `total_7_24`
- `directions_n`
- `direction_coverage`
- street and location information
- `SegmentID`

Records missing one or more complete time periods were excluded.

Additional quality control identified zero-volume records that were likely data artifacts. Sixteen zero-volume segment-date records were removed.

The final processed traffic dataset contains **2,720 segment-date records**.

Processed file:

`data/processed/traffic_daily_2022_2026.csv`

---

### Motor Vehicle Collision Data

Crash data were derived from the **NYC Motor Vehicle Collisions – Crashes** dataset.

The raw crash dataset contains individual collision events with information on:

- crash date and time
- borough
- latitude and longitude
- street location
- persons injured and killed
- pedestrians injured and killed
- cyclists injured and killed
- motorists injured and killed
- collision ID

The crash dataset was limited to:

- Manhattan
- Bronx
- Brooklyn
- Queens
- Years 2022–2026

To make the crash analysis consistent with the traffic-volume analysis, crashes were also restricted to the same daily analysis window:

**07:00–23:59**

Each crash event was assigned to one of three time groups:

- **Morning:** 07:00–11:59
- **Afternoon:** 12:00–17:59
- **Evening:** 18:00–23:59

Unlike the traffic dataset, crashes were not aggregated during this cleaning step. Each row remains an individual collision event so that crash-level injury, fatality, location, and time information is preserved.

After the temporal and borough filters, the cleaned dataset contains **237,003 crash events**.

### Coordinate Quality Control

Crash coordinates were checked before spatial matching.

Two types of unusable coordinates were identified:

- missing latitude/longitude
- latitude/longitude values equal to zero

A new Boolean field, `valid_coords`, was created.

Results:

- **229,527 crashes** have valid coordinates
- **7,476 crashes** have missing or invalid coordinates

Crashes with invalid coordinates were retained in the cleaned dataset because many still contain usable street or address information.

The cleaned crash dataset retains:

- `COLLISION_ID`
- `CRASH DATE`
- `CRASH TIME`
- `time_group`
- `BOROUGH`
- `ZIP CODE`
- `LATITUDE`
- `LONGITUDE`
- `LOCATION`
- `valid_coords`
- `ON STREET NAME`
- `CROSS STREET NAME`
- `OFF STREET NAME`
- injury and fatality counts for persons, pedestrians, cyclists, and motorists

Processed file:

`data/processed/crashes_clean_2022_2026.csv`


