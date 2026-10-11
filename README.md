# Country GDP ETL

A Python learning project that extracts country GDP figures from an archived
Wikipedia table, normalizes formatted values from USD millions to USD billions,
and loads the result into CSV and SQLite.

```text
Archived HTML → BeautifulSoup → pandas / NumPy → CSV + SQLite → SQL filter
```

## Run locally

Use Python 3. Run from the repository root; the source fetch requires Internet
Archive access.

```bash
git clone https://github.com/siva-fy/ETL-Project-GDP.git
cd ETL-Project-GDP
python3 -m venv .venv
source .venv/bin/activate
python -m pip install requests beautifulsoup4 pandas numpy
python etl_project_gdp.py
```

On Windows, activate with `.venv\Scripts\activate`.

The script overwrites `Countries_by_GDP.csv`, replaces the `Countries_by_GDP`
table in `World_Economies.db`, and appends progress to `etl_project_log.txt`.
It then prints countries whose GDP is at least USD 100 billion.

## Repository guide

| File | Purpose |
| --- | --- |
| [`etl_project_gdp.py`](etl_project_gdp.py) | Extraction, unit conversion, loading and query execution |
| [`Countries_by_GDP.csv`](Countries_by_GDP.csv) | Historical sample output |
| [`World_Economies.db`](World_Economies.db) | SQLite output |
| [`etl_project_log.txt`](etl_project_log.txt) | Historical execution log |

```sql
SELECT Country, GDP_USD_billions
FROM Countries_by_GDP
WHERE GDP_USD_billions >= 100
ORDER BY GDP_USD_billions DESC;
```

## Limitations

The source URL is a September 2023 snapshot, so the checked-in data is historical.
Extraction relies on HTML table positions and does not include a timeout, retries,
schema checks or automated tests. Dependencies are not pinned and the script runs
at import time. The live extraction has not been revalidated in this documentation
update.

See [Banking ETL](https://github.com/siva-fy/ETL-Banks-Project) for currency
conversion, or [Event Warehouse](https://github.com/siva-fy/event-warehouse) for
a tested offline pipeline with validation and replay handling.
