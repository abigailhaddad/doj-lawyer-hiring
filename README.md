# DOJ Attorney Hiring: Postings vs. Actual Hires

Connects two public federal data sources to ask: do DOJ attorney job postings on USAJobs track actual hiring? And what has happened to both since January 20, 2025?

## How the data sources connect

Both OPM EHRI and USAJobs use OPM's four-digit occupational series codes. Series **0905** (General Attorney) is the primary attorney series at DOJ. Filtering both sources to DOJ + 0905 puts them on the same population.

| Source | What it measures | Lag |
|--------|-----------------|-----|
| USAJobs historical API | Job postings (closed announcements) | ~0 months from posting date |
| OPM EHRI accessions | Actual new hires | 1–3 months from hire date |

Federal hiring timelines typically run **3–6 months** from posting to start date, so you'd expect accessions to lag postings by that amount under normal conditions.

## Running the notebook

```bash
pip install -r requirements.txt
jupyter notebook doj_attorney_hiring.ipynb
```

No API keys required. The EHRI download will take a few minutes (one parquet file per month from HuggingFace).

## Data sources

- **EHRI**: [`impactproject/opm-ehri-data`](https://huggingface.co/datasets/impactproject/opm-ehri-data) on HuggingFace
- **USAJobs**: [`data.usajobs.gov/api/historicjoa`](https://developer.usajobs.gov/API-Reference/GET-api-historicjoa) — no auth required
- **Methodology reference**: [`abigailhaddad/federal-data-guide`](https://github.com/abigailhaddad/federal-data-guide)
