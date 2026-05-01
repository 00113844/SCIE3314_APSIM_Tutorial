---
title: DataStore
tags: [entity, core-component, output-database]
related: [[Report]]
source: [[APSIM_Internal_Guide]]
status: draft
---

# DataStore — Simulation Output Database

## Definition

The **DataStore** is an in-memory SQL database that collects output from the Report component each day. At the end of the simulation, DataStore exports data to Excel (`.xlsx`) or CSV (`.csv`) files for analysis.

**Key role**: DataStore is the bridge between APSIM simulation and external analysis (spreadsheets, Python, R, Jupyter).

---

## In APSIM

### Purpose

1. **Data collection**: Receives daily records from Report
2. **In-memory storage**: Maintains efficient SQL-like structure
3. **Export**: Convert to human-readable formats (Excel, CSV)
4. **Querying**: Optional; can query DataStore within APSIM UI

### Configuration

**Typical DataStore setup** (usually auto-created):

```
[DataStore]
├─ [Simulation Table 1]
│  └─ Daily records (one row per day)
├─ [Simulation Table 2]
│  └─ Daily records (if multiple simulations in file)
└─ Summary tables (optional; total yield, seasonal averages)
```

**No user configuration needed** in most cases; APSIM creates DataStore automatically when Report is present.

---

## Output File Format

### Excel Export (`.xlsx`)

**Structure**:
- One sheet per simulation
- Column headers = variables from Report
- Rows = daily timesteps
- Data types preserved (dates, numbers, text)

**Example** (`wheat_simulation_output.xlsx`):
```
Date       | Rainfall | ESW  | Wheat_Biomass | Wheat_Stage | Wheat_GrainMass
-----------|----------|------|---------------|-------------|----------------
2021-03-01 | 5.2      | 120  | 0             | 0           | 0
2021-03-02 | 0.0      | 124  | 0             | 0           | 0
2021-03-03 | 0.0      | 118  | 0.5           | 5           | 0
...        | ...      | ...  | ...           | ...         | ...
2021-11-30 | 0.0      | 45   | 12500         | 90          | 5200
```

### CSV Export (`.csv`)

**Structure**:
- Same as Excel, but comma-separated
- No formatting; plain text
- Easily imported into Python, R, or Jupyter

**Example** (`wheat_simulation_output.csv`):
```
Date,Rainfall,ESW,Wheat_Biomass,Wheat_Stage,Wheat_GrainMass
2021-03-01,5.2,120,0,0,0
2021-03-02,0.0,124,0,0,0
2021-03-03,0.0,118,0.5,5,0
...
```

---

## Export Workflow

### In APSIM UI

1. Run simulation
2. Click on Simulation node in tree
3. DataStore appears in results panel
4. Right-click DataStore → **Export**
5. Choose Excel (`.xlsx`) or CSV (`.csv`)
6. Save to file

### Programmatically

APSIM can export via command-line tools (see APSIM documentation).

---

## Data Analysis Workflow

### Typical Analysis in Excel

```
Raw output (ExcelFile):
Date | Rainfall | ESW | Biomass | Yield
     |   mm     | mm  |  kg/ha  | kg/ha
     |          |     |         |
Raw data → Calculations:
     → Seasonal total rainfall (sum)
     → Min/max ESW (min/max)
     → Peak biomass
     → Harvest grain yield
     → Agronomic Efficiency of N (if multi-N treatments)
```

### Typical Analysis in Python/Jupyter

```python
import pandas as pd

# Load DataStore export
df = pd.read_csv('wheat_output.csv', parse_dates=['Date'])

# Calculate seasonal totals
total_rain = df['Rainfall'].sum()
peak_esw = df['ESW'].max()
peak_biomass = df['Wheat_Biomass'].max()
final_yield = df['Wheat_GrainMass'].iloc[-1]

# Plot: ESW vs. date
df.plot(x='Date', y='ESW', title='Soil Water Over Time')
```

---

## Key Concepts

### SQL Structure (Behind the Scenes)

DataStore is internally a SQLite database with schema:

```sql
CREATE TABLE Simulation_Daily (
  Date TEXT,
  Rainfall REAL,
  ESW REAL,
  Wheat_Biomass REAL,
  Wheat_Stage REAL,
  Wheat_GrainMass REAL
);
```

**Why SQL?** Efficient storage; can query data; scales to large files (1000s of rows, 100s of columns).

### Multi-Simulation DataStore

If you run **multiple simulations in one `.apsimx` file** (e.g., 4 N-rate treatments):

```
[DataStore]
├─ Wheat_N0_Daily (rows: 300)
├─ Wheat_N80_Daily (rows: 300)
├─ Wheat_N160_Daily (rows: 300)
└─ Wheat_N240_Daily (rows: 300)
```

Each simulation table can be exported separately, or combined in analysis.

---

## Common Misconceptions

### 1. "I can view DataStore results while simulation is running"
**Clarification**: DataStore is built during simulation; it's not accessible until simulation completes. You must run the full simulation first, then export.

### 2. "DataStore automatically includes all simulation variables"
**Clarification**: DataStore only contains variables listed in Report. If Report doesn't include `[Wheat].LAI`, DataStore won't have it. You must configure Report before simulation to get the data you need.

### 3. "Exporting to Excel loses data"
**Clarification**: Export preserves all data; just changes format. Exporting from DataStore → Excel vs. DataStore → CSV gives the same data, just different file format.

### 4. "I can edit DataStore and re-import to modify simulation results"
**Clarification**: DataStore is read-only output. Modifications in Excel don't affect the simulation or APSIM. To change simulation, modify `.apsimx` configuration and re-run.

---

## Troubleshooting

### DataStore is empty or missing

**Checklist**:
1. Did simulation actually run? (Check `.log` file for errors)
2. Is Report component in the Simulation? (If no Report, DataStore won't populate)
3. Did you wait for simulation to complete? (DataStore is only readable after run finishes)
4. Is DataStore node visible in results panel? (May need to expand tree)

### Export fails or file is corrupted

**Checklist**:
1. Is another program holding the file open? (Excel, Python, Jupyter)
2. Is disk full? (Check available space)
3. Is filename valid? (No special characters like `\`, `:`, `*`?)
4. Is export path writable? (Check file permissions)

**Fix**: Close Excel file, try again, or export to different location.

### CSV export has special characters or encoding issues

**Checklist**:
1. Is your system using non-ASCII locale? (Can affect decimal separators)
2. Are date formats misaligned? (Some systems use DD-MM-YYYY vs. YYYY-MM-DD)

**Fix**: Re-export and manually check first row; adjust locale settings if needed.

---

## Exercise References

- **Module 3** (Reading Output): Export DataStore from `wheat_example.apsimx`
- **Module 4** (Reading Output): Analyze ESW, rainfall, yield patterns in Excel
- **Exercise A** (Fallow): Export 7-year water balance; plot ESW vs. rainfall
- **Exercise B** (N Response): Export 4-treatment DataStore; calculate AEN (Agronomic Efficiency of N)
- **Exercise C** (Sowing Date): Export 3-scenario DataStore; compare yields across dates
- **Exercise D** (20-Year Variability): Export multi-year DataStore; analyze yield distribution

---

## Related Concepts

- **[[Report]]** — Defines which variables populate DataStore
- **[[Clock]]** — DataStore has one row per Clock day

---

## Resources

- **APSIM Documentation**: https://apsimnextgeneration.netlify.app/ → DataStore export
- **Excel pivot tables**: Powerful for summarizing DataStore exports
- **Python pandas**: Load CSV exports into DataFrames for analysis
