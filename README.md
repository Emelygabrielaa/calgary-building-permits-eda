# Calgary Building Permits: Descriptive Analysis and EDA

Project 1 of 11 in a statistician path. It uses the City of Calgary's building permit application records to answer five practical questions: how long applications take, how much the projects are worth, how activity changes over time, where it happens, and how complete the data is. Part A answers them with descriptive statistics, and Part B goes back to the same questions with formal tests and models.

**Author:** Emely Rodriguez
**Languages:** Python (lead) and R (cross-check)

## Status

| Phase | Content | Status |
|---|---|---|
| 0. Data audit | Structure, quality and definitions of the source data | Done |
| 1. Analysis dataset | Handling of audit issues, selected columns, derived variables, dated snapshot | In progress |
| 2. Part A | Descriptive answers to the five questions | Pending |
| 3. Part B | Formal tests and models | Pending |
| 4. Report | Findings and conclusions | Pending |

## The five questions

1. **Processing time:** how long do applications take?
2. **Project cost:** how much are the projects worth?
3. **Volume over time:** how does activity change over time?
4. **Geography:** where does it happen?
5. **Missingness:** how complete is the data?

## Data

- **Source:** City of Calgary Open Data portal, dataset "Building Permits" (ID c2es-76ed)
- **Provider:** The City of Calgary, Dev Business and Bldg Srvc
- **Licence:** City of Calgary Open Data Terms (https://data.calgary.ca/d/Open-Data-Terms/u45n-7awa)
- **Attribution:** Contains information licensed under the Open Government Licence - City of Calgary.

The table is updated frequently, so from Phase 1 onward all results use a dated snapshot stored in `data/raw/`.

## Repository structure

- `notebooks/`: one notebook per phase
- `data/raw/`: the dated snapshot, as published by the portal
- `data/processed/`: the analysis dataset (Phase 1)

## What Phase 0 found

- One row per application, with a unique permit number: about 503,000 rows and 30 columns, from June 1999.
- The main gaps are the estimated cost (7.8% missing), the area (`totalsqft`, recorded for one permit type only) and issue and completion dates, whose gaps depend on the application status.
- The audit lists 22 findings in an issues log. Their handling is decided in Phase 1.

## Reproducing

Open the notebooks in Google Colab and run them in order. Phase 0 queries the portal directly. Later phases read the snapshot in `data/raw/`.
