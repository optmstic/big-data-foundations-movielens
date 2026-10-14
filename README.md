# Big Data Foundations Group Project

Course project for **Big Data Foundations** (Nova Information Management School, Nova IMS), 2026.
Case analysis: **MovieLens 32M**, Phase 1 — the local analytical layer.

DuckDB + Parquet analysis of [MovieLens 32M](https://grouplens.org/datasets/movielens),
enriched with [IMDb metadata](https://datasets.imdbws.com) for release year and runtime.
Only MovieLens ratings are analysed, as they are (0.5–5 scale).

## Structure

| Path | Content |
|---|---|
| `group_project.ipynb` | Notebook (sections A–G, same pattern as the Week 3 lab). All outputs are saved in the file. |
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

Then install the dependencies and run all cells of the notebook (safe to re-run):

```bash
pip install -r requirements.txt
jupyter lab group_project.ipynb
```

## Project details

- Deliverable: slides (PDF, max 30) + scripts, submitted through Moodle
