# 2026 NASCAR Chase Track Performance

This project analyzes how NASCAR Cup Series Chase drivers performed during the 2026 regular season at tracks that will appear again during the Chase.

Using race results from Racing-Reference, the project compares points scored by each Chase driver across eight tracks and visualizes the results using a heatmap and horizontal bar chart.

## Requirements

- `pandas`
- `matplotlib`
- `seaborn`
- `cup_rr_2026_results.csv` (provided in the repository)

## Getting Started

### Imports
Import the required libraries and load the provided 2026 NASCAR Cup Series results:

```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

df = pd.read_csv("cup_rr_2026_results.csv")
```

## Analysis
The analysis focuses on eight tracks that appeared during the 2026 regular season and will appear again during the Chase:

- Darlington
- Bristol
- Kansas
- Las Vegas
- Charlotte
- Phoenix
- Talladega
- Martinsville

Race results are filtered to the 16 Chase drivers and these eight tracks. The data is then reshaped into a driver-by-track matrix containing the points scored in each race.

```python
# List of Drivers and Tracks in the 2026 Chase

# Tracks in Chase (that were also run during the 2026 regular season)
chase_tracks = [
    "Darlington",
    "Bristol",
    "Kansas",
    "Las Vegas",
    "Charlotte",
    "Phoenix",
    "Talladega",
    "Martinsville"]

# Identify Chase drivers by total regular-season points
chase_drivers = df.groupby("Driver")["Pts"].sum().sort_values(ascending=False).head(16).index

# Order DF by race_number:
df = df.sort_values('race_number')

# Filter DF to chase_tracks
df = df[df['site'].isin(chase_tracks)]
df = df[df['Driver'].isin(chase_drivers)].copy()

# Inspect Shape of DF
df.shape  # (128,19) 8 Race Resutlts x 16 Drivers
```

## Chase Track Points Heatmap
The heatmap compares each driver's points scored at the eight Chase tracks during the regular season.
Drivers are ordered by their regular-season points ranking, while tracks are displayed in the order they appear on the Chase schedule.
```python
# View Three Pertinent Columns
# Driver | Site | Pts
print(f"DF Main Columns:\n\n{df[['Driver', 'site', 'Pts']]}\n\n\n")

# Pivot the table to wide Format - Index on the Driver
heatmap_df = df.pivot(
    index='Driver',
    columns='site',
    values='Pts')

# Sort columns by the sequential order of the 2026 Chase schedule
heatmap_df = heatmap_df[chase_tracks]

# Sort rows by driver regular-season points ranking
heatmap_df = heatmap_df.reindex(chase_drivers)

# View pivoted table DF (heatmap_df)
print(f"Pivoted Table:\n\n{heatmap_df}")
```

### Heatmap Preview
<img width="1152" height="738" alt="image" src="https://github.com/user-attachments/assets/e9f40549-b108-45c1-bb65-3b7eaecd8c7f" />

## Total Points at Chase Tracks
The second visualization aggregates each driver's results across all eight tracks and ranks the Chase field by total points scored.
Driver bars are color-coded using colors associated with their primary sponsors.

### Bar Chart Preview
<img width="1146" height="762" alt="image" src="https://github.com/user-attachments/assets/15301652-1ca6-4efe-9f66-09645d698497" />


## Reading the Visualizations
- **Heatmap:** Each cell represents the points scored by a driver at a specific Chase track during the 2026 regular season. Darker green cells indicate higher point totals.
- **Bar chart:** Each bar represents the driver's combined points across all eight Chase tracks.
