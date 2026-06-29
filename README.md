# DOJ Attorney Hiring: Postings, Hires, and the Shrinking Workforce

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/abigailhaddad/doj-lawyer-hiring/blob/main/doj_attorney_hiring.ipynb)

Tracks DOJ series 0905 (General Attorney) attorneys using two public data sources: total headcount, actual hiring, departures, and job postings — before and after January 20, 2025.

## What the data shows

**DOJ-wide:**
- **−2,871 attorneys** (~22%) since peak headcount in early 2024; still declining as of April 2026
- Early-2025 hiring freeze was severe (avg ~27 new hires/month Feb–Jun 2025 vs ~75 same period 2024)
- By 2026, both postings and accessions have recovered — but can't keep up with ongoing attrition
- October 2025 saw a large single-month USAO headcount drop (~340), consistent with a RIF event

**By division (inferred — see caveat below):**
- **Civil Rights**: down 66% from peak; postings essentially stopped after inauguration (~10 in all of 2026 YTD vs ~29 in all of 2024)
- **USAO**: actively rebuilding — posting rate is now 3× the pre-inauguration baseline; accessions recovering
- **Civil Division**: posting surge in 2026 driven by immigration litigation (Office of Immigration Litigation)
- **Antitrust**: postings cratered post-inauguration, near zero through mid-2026
- **Criminal**: modest decline, now recovering; absorbed Tax Division attorneys in Dec 2025
- **Tax Division**: dissolved December 2025; attorneys reassigned to Criminal

## How the data sources connect

All use OPM's four-digit occupational series codes. Series **0905** (General Attorney) is the primary attorney classification at DOJ.

| Source | What it measures | Query method |
|--------|-----------------|--------------|
| OPM EHRI | Monthly headcount snapshots, new hires (accessions), and separations | DuckDB `hf://` — no download |
| USAJobs R2 mirror | Historical job postings | DuckDB HTTP — no download |

## Running the notebook

```bash
pip install -r requirements.txt
jupyter notebook doj_attorney_hiring.ipynb
```

No API keys required. All data is queried server-side — nothing is downloaded locally.

## Division-level analysis (Parts 6–9)

The notebook breaks out headcount, accessions, separations, and job postings by Main Justice division, filtering to DOJ's "Offices, Boards and Divisions" subelement (DJ01).

> [!CAUTION]
> **Division assignments are estimates — not confirmed by OPM or DOJ.**
>
> EHRI does not label employees by division name. It uses a **Personnel Office Identifier (POI)**, a four-digit OPM code assigned to each operating office. To map POIs to divisions, we use **duty-station fingerprinting**: we look at the cities where that POI's attorneys are concentrated and match them to known division field offices.
>
> For example, POI 1036 has a notable cluster of attorneys in Denver, consistent with the Environment & Natural Resources Division's Rocky Mountain field office. POI 1817 has a Dallas office consistent with the Tax Division before its December 2025 dissolution. POI 1818 is overwhelmingly concentrated in Washington DC, matching the Civil Rights Division's profile.
>
> **This is inference, not ground truth.** OPM does not publish a POI-to-division crosswalk, and we have not independently verified these assignments with DOJ. Treat division-level charts as directionally informative, not authoritative.

| POI  | Inferred Division | Key location signal |
|------|-------------------|---------------------|
| 1845 | Civil Division | Largest; scattered field offices |
| 1818 | Civil Rights Division | Nearly all DC |
| 1034 | Criminal Division | Miami / LA / Houston / Chicago |
| 1036 | Env & Natural Resources (ENRD) | Denver field office |
| 1830 | Antitrust Division | SF / NY / Chicago |
| 1831 | Justice Management Division | Suburban DC (Merrifield VA) |
| 1817 | Tax Division (dissolved Dec 2025) | Dallas field office |

USAJobs postings are classified to divisions using the `hiringSubelementName` field (e.g. "Civil Rights Division", "Criminal Division"). Approximately 20–25% of DOJ 0905 postings lack a subelement name; most of those are USAO positions identifiable by job title ("Assistant United States Attorney") or announcement number prefix.

## USAJobs data methodology note

The R2 mirror combines two parquet families: `historical_jobs_*.parquet` (from the USAJobs historical API) and `current_jobs_*.parquet` (from the current search API). Both APIs return only the department-level `hiringAgencyName` ("Department of Justice") when the posting agency never populated the sub-agency field — this is a source limitation, not a pipeline gap. The notebook deduplicates by `usajobsControlNumber`, preferring historical records where bureau-level names are more often populated.

See [`abigailhaddad/usajobs_historical`](https://github.com/abigailhaddad/usajobs_historical) README for full details on the combining methodology.

## Data sources

- **EHRI**: [`impactproject/opm-ehri-data`](https://huggingface.co/datasets/impactproject/opm-ehri-data) on HuggingFace
- **USAJobs**: R2 parquet mirror at `pub-317c58882ec04f329b63842c1eb65b0c.r2.dev`
- **Methodology reference**: [`abigailhaddad/federal-data-guide`](https://github.com/abigailhaddad/federal-data-guide)
