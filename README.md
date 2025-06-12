# Database Blending Utility

A small Python utility for “blending” table-based time-series data from one SQLite database into another.  
It detects missing year-columns or “gaps” in the target database, backfills from the source, and keeps track of the earliest and most recent non-null values per row.

Note that this is designed for tables under IFsHistSeries & IFsDataImport, with the assumption of data structure being 188 rows and following columns Country, FIPS_CODE, ...YEAR COLUMNS..., Earliest, MostRecent. These can be easily updated in the code to adapt other data structures.

---

## Features

- **Automatic year-column detection**: identifies any column whose name consists solely of digits.
- **Full time-range alignment**: builds the union of all year-columns across both databases, so you never lose years.
- **Gap filling**: wherever the “new” database has `NaN` but the “old” has a value, it backfills automatically.
- **Metadata tracking**: recomputes two helper columns:
  - `Earliest`: first non-null year-value in the row
  - `MostRecent`: last non-null year-value in the row
- **Safe overwrite**: drops and recreates target tables to ensure a clean blend.

