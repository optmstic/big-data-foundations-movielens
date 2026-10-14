# Big Data Foundations Group Project

Course project for **Big Data Foundations** (Nova Information Management School, Nova IMS), 2026.
Case analysis: **MovieLens 32M**, Phase 1 — the local analytical layer.

DuckDB + Parquet analysis of [MovieLens 32M](https://grouplens.org/datasets/movielens),
enriched with [IMDb metadata](https://datasets.imdbws.com) for release year and runtime.
Only MovieLens ratings are analysed, as they are (0.5–5 scale).

## Structure

| Path | Content |
|---|---|
| `group_project.ipynb` | Notebook (sections A–H, same pattern as the Week 3 lab). All outputs are saved in the file. |
| `raw/` | Input files (not committed — see below) |
| `parquet/` | CSV/TSV converted to Parquet, ZSTD (not committed) |
| `movielens.duckdb` | Catalog only: views over the Parquet files (not committed) |

## Reproduce the data

The datasets are public, so they are not versioned here. To download them:

```bash
mkdir -p raw && cd raw

# MovieLens 32M (unzip, then the CSVs land in raw/)
curl -O https://files.grouplens.org/datasets/movielens/ml-32m.zip
unzip ml-32m.zip && mv ml-32m/* . && rmdir ml-32m

# IMDb metadata (title basics only)
curl -O https://datasets.imdbws.com/title.basics.tsv.gz
gunzip title.basics.tsv.gz
```

The notebook ingests only feature films and the two IMDb columns it uses (year, runtime), so the
1.1 GB IMDb file becomes a 3.4 MB Parquet file.

To try the small dataset first (100K ratings), put the CSVs of `ml-latest-small` in `raw/` instead and
set `MIN_RATINGS_TOP = 50` (in Q3) and `MIN_RATINGS = 10` (in section G). Nothing else changes.

IMDb refreshes its files daily, so counts can differ slightly from the saved outputs, which come from the
snapshot downloaded on 13 October 2026. MovieLens 32M is fixed.

Then install the dependencies and run all cells of the notebook (safe to re-run):

```bash
pip install -r requirements.txt
jupyter lab group_project.ipynb
```

## Scalability

- **Same code, 300× more data.** The notebook runs unchanged on `ml-latest-small` (100K ratings) and on MovieLens 32M; only the two rating thresholds change.
- **Columnar files, queried in place.** About 2 GB of CSV/TSV becomes about 176 MB of Parquet, and DuckDB reads those files through views, so nothing has to be loaded into memory first.
- **New sources plug in.** IMDb (12.9M titles) was added with one ingest step and one join, without touching the rest of the pipeline.
- **Fast to rebuild.** The full pipeline, conversion included, ran in about 20 seconds on a laptop.

Not tested yet: datasets larger than 32M, and the cloud layer (S3 and Athena), which would read the same Parquet files.

## Project details

- Deliverable: slides (PDF, max 30) + scripts, submitted through Moodle
