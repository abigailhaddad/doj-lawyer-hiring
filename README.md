# DOJ Attorney Hiring: Postings, Hires, and the Shrinking Workforce

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/abigailhaddad/doj-lawyer-hiring/blob/main/doj_attorney_hiring.ipynb)

Tracks DOJ series 0905 (General Attorney) attorneys using two public data sources: total headcount, actual hiring, departures, and job postings — before and after January 20, 2025.

## What the data shows

- **−2,871 attorneys** (~22%) since peak headcount in early 2024; still declining as of April 2026
- Early-2025 hiring freeze was severe (avg ~27 new hires/month Feb–Jun 2025 vs ~75 same period 2024)
- By 2026, both postings and accessions have recovered — but can't keep up with ongoing attrition
- October 2025 saw a single-month headcount drop of ~700, consistent with a RIF event

## How the data sources connect

All three use OPM's four-digit occupational series codes. Series **0905** (General Attorney) is the primary attorney classification at DOJ.

| Source | What it measures | Query method |
|--------|-----------------|--------------|
| OPM EHRI | Monthly headcount snapshots, new hires (accessions), and derived separations | DuckDB `hf://` — no download |
| USAJobs R2 mirror | Historical job postings | DuckDB HTTP — no download |

## Running the notebook

```bash
pip install -r requirements.txt
jupyter notebook doj_attorney_hiring.ipynb
```

No API keys required. All data is queried server-side — nothing is downloaded locally.

## Data sources

- **EHRI**: [`impactproject/opm-ehri-data`](https://huggingface.co/datasets/impactproject/opm-ehri-data) on HuggingFace
- **USAJobs**: R2 parquet mirror at `pub-317c58882ec04f329b63842c1eb65b0c.r2.dev`
- **Methodology reference**: [`abigailhaddad/federal-data-guide`](https://github.com/abigailhaddad/federal-data-guide)
