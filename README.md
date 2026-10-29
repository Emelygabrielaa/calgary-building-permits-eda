# Calgary Building Permits: Descriptive Analysis and EDA

Project 1 of 11 in a statistician path. It uses the City of Calgary's building permit application records to answer five practical questions: how long applications take, how much the projects are worth, how activity changes over time, where it happens, and how complete the data is. Part A answers them with descriptive statistics, and Part B goes back to the same questions with formal tests and models.

**Author:** Emely Rodriguez
**Languages:** Python (lead) and R (cross-check)

## Status

| Phase | Content | Status |
|---|---|---|
| 0. Data audit | Structure, quality and definitions of the source data | Done |
| 1. Analysis dataset | Handling of audit issues, selected columns, derived variables, dated snapshot | Done |
| 2. Analysis | Part A (descriptive) and Part B (formal tests and models), one notebook per question | In progress |
| 3. Report | Findings and conclusions | Pending |

## The five questions

1. **Processing time:** how long do applications take?
2. **Project cost:** how much are the projects worth?
3. **Volume over time:** how does activity change over time?
4. **Geography:** where does it happen?
5. **Missingness:** how complete is the data?

## Phase 2 notebooks

| Question | Notebook | Status |
|---|---|---|
| 1. Processing time | [`02_q1_processing_time.ipynb`](notebooks/02_q1_processing_time.ipynb) | Done |
| 2. Project cost | `02_q2_project_cost.ipynb` | Pending |
| 3. Volume over time | `02_q3_volume_over_time.ipynb` | Pending |
| 4. Geography | `02_q4_geography.ipynb` | Pending |
| 5. Missingness | `02_q5_missingness.ipynb` | Pending |

## Data

- **Source:** City of Calgary Open Data portal, dataset "Building Permits" (ID c2es-76ed)
- **Provider:** The City of Calgary, Dev Business and Bldg Srvc
- **Licence:** City of Calgary Open Data Terms (https://data.calgary.ca/d/Open-Data-Terms/u45n-7awa)
- **Attribution:** Contains information licensed under the Open Government Licence - City of Calgary.

The table is updated frequently, so from Phase 1 onward all results use a dated snapshot stored in `data/raw/`.

## Repository structure

- `notebooks/`: one notebook per phase (`00_data_audit.ipynb`, `01_analysis_dataset.ipynb`)
- `data/raw/`: the dated snapshot of the portal table, as received (21 columns, stored as text)
- `data/processed/`: the analysis dataset built from the snapshot in Phase 1 (46 columns, Parquet)

## What Phase 0 found

- One row per application, with a unique permit number: about 503,000 rows and 30 columns, from June 1999.
- The main gaps are the estimated cost (7.8% missing), the area (`totalsqft`, recorded for one permit type only) and issue and completion dates, whose gaps depend on the application status.
- The audit lists 23 findings in an issues log (the 23rd, very small estimated costs, was found in Phase 1). Their handling is decided in Phase 1.

## What Phase 1 produced

- **Snapshot:** `data/raw/building_permits_2026-10-27.csv.gz`, a copy of the portal table as received on 2026-10-27: 502,814 rows and 21 of the 30 columns (the names and addresses were left out), all stored as text.
- **Processed dataset:** `data/processed/building_permits_processed_2026-10-27.parquet`, built only from the snapshot: 502,814 rows and 46 columns, with the types of each column kept. No row was removed.
- **Handling of the audit issues:** each decision is written as code in the Phase 1 notebook, with its reasons. Values that cannot be real are set to missing in a new `_clean` column and marked with a flag, and the raw column stays next to it, so every decision can be reversed. They cover applications with dates in an impossible order (33), missing issue and completion dates in closed applications (292 and 25), negative or very small estimated costs (517), impossible `totalsqft` values (668), negative housing units (2) and applications without a location (921).
- **Derived variables:** six status groups, time to issue, time to close, year and month of application, and flags for the complete years and months.
- **Data dictionary:** the 25 new columns are described in section 1.5 of the Phase 1 notebook.

## Reproducing

Open the notebooks in Google Colab and run them in order. Phase 0 queries the portal directly. In the Phase 1 notebook, section 1.1 is the one-time download of the snapshot and is not run again: to rebuild the processed dataset, run section 1.0.1 and then sections 1.2.0 to 1.4.2. Do not use "Run all" in that notebook.

To use the processed dataset in Python:

```python
import io, requests, pandas as pd

url = ("https://raw.githubusercontent.com/Emelygabrielaa/calgary-building-permits-eda/"
       "main/data/processed/building_permits_processed_2026-10-27.parquet")
df = pd.read_parquet(io.BytesIO(requests.get(url).content))
```

To use it in R (needs the `arrow` package):

```r
library(arrow)

url <- paste0("https://raw.githubusercontent.com/Emelygabrielaa/calgary-building-permits-eda/",
              "main/data/processed/building_permits_processed_2026-10-27.parquet")
download.file(url, "processed.parquet", mode = "wb")
df <- read_parquet("processed.parquet")
```
