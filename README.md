# Big Data Foundations (BDF) 2026 — Group Project

Course project for **Big Data Foundations** (Nova Information Management School, Nova IMS), 2026.
Case analysis: **MovieLens 32M**, Phase 1 — the local analytical layer.

DuckDB + Parquet analysis of [MovieLens 32M](https://grouplens.org/datasets/movielens),
enriched with [IMDb metadata](https://datasets.imdbws.com) (`title.basics`: type, year, runtime).
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

Then install the dependencies and run all cells of the notebook (safe to re-run):

```bash
pip install -r requirements.txt
jupyter lab group_project.ipynb
```

## Results

- **Section F** — six analytical questions on genres, rating distribution, top-rated movies, activity per year, tags and user long tail.
- **Section G** — IMDb enrichment via `links.csv` (`'tt' || imdbId`), 99.74% match rate.
- **Section H** — performance evidence: Parquet is ~5.6× smaller than the CSV and 7–20× faster to query.

## Course details

- Group project, 40% of the final classification
- Lecturer: Humberto Carreiro (hcarreiro@novaims.unl.pt)
- Deliverable: slides (PDF, max 30) + scripts, submitted through Moodle
