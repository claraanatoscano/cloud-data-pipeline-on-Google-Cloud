# Wisconsin Schools BigQuery Pipeline

An end-to-end data pipeline on Google Cloud that loads Wisconsin public school data, models it with Dataform, analyzes it in BigQuery, and classifies school names with BigQuery's built-in AI.

Built for CS 544: Introduction to Big Data Systems at UW–Madison (Spring 2026).

## What it does

1. **Ingest:** Uploads raw school data (Parquet) to Google Cloud Storage using PyArrow.
2. **Model:** Defines three Dataform (SQLX) tables, `schools`, `wi_counties`, and `wi_county_schools`, then compiles and inspects their dependency graph through the Dataform API.
3. **Analyze:** Queries the modeled data in BigQuery, including a multi-step query written in BigQuery's pipe syntax that finds counties with at least two cities that each have three or more public high schools.
4. **Estimate cost:** Uses a BigQuery dry run to measure bytes scanned and estimate how many times a query could run per TiB.
5. **Classify with AI:** Uses `AI.CLASSIFY` in SQL to label Madison elementary school names by what they're named after (a person, a place, nature, or other).

## Sample results

- 72 Wisconsin counties and 2,116 public schools in the modeled data
- Counties meeting the high school criteria: Brown, Dane, Milwaukee, and Waukesha
- School names classified directly in SQL, for example "Lake View Elementary" as a place and "Cesar Chavez Elementary" as a person

## Tech stack

Python · Google Cloud Storage · BigQuery · Dataform (SQLX) · PyArrow · Jupyter

## Project structure

```
definitions/          Dataform SQLX table definitions
  schools.sqlx
  wi_counties.sqlx
  wi_county_schools.sqlx
p8.ipynb              Main notebook: ingestion, modeling, analysis, AI classification
requirements-dev.txt  Python dependencies
```

## Running it

Requires a Google Cloud project with BigQuery, Cloud Storage, and Dataform enabled.

```bash
pip install -r requirements-dev.txt
gcloud auth application-default login
```

Update `PROJECT`, `P8_BUCKET`, and `REGION` at the top of `p8.ipynb` to match your project, then run the notebook.
