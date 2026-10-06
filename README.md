# k12-breach-check

A public, HaveIBeenPwned-style lookup for K-12 school cybersecurity incidents.
Visitors search for their school or district and see every publicly reported
breach incident tied to it: dates, records affected, data types exposed, and
source links.

Live site: https://nyu-mlab.github.io/k12-breach-check/

## What this is not

We track incidents **per school, not per person**. This repo holds no email
addresses, student lists, or individual records from any breach, and the site
cannot tell a visitor whether their personal information was exposed.

## Files

- `index.html` — the single-page site (search, filters, results, vendor section).
- `index.json` — the slim public index. The only data file in this repo.
  Generated from the canonical breach database; see below.

## Refreshing the index

The index is a build artifact. When the canonical breach database
(`data/breach/breach_canonical_final.json` in
[nyu-mlab/k12-cybersecurity-research](https://github.com/nyu-mlab/k12-cybersecurity-research))
is updated, refresh it in one step:

```bash
cd k12-cybersecurity-research
python3 scripts/build_public_breach_index.py
cp analysis_output/public_breach_index.json /path/to/k12-breach-check/index.json
cd /path/to/k12-breach-check
# commit and push index.json; GitHub Pages redeploys automatically
```

The generator (`scripts/build_public_breach_index.py`) is deterministic and
re-runnable. It includes only US K-12 district/school headline incidents plus
vendor-level incidents, and only structured public-safe fields: district name,
state, NCES district ID, and per incident the ID, date, type, records affected,
confidence level, keyword-derived data-type tags, and up to two public source
URLs. Free-text descriptions, analyst notes, and internal provenance fields are
never published.

## Hosting

GitHub Pages serves this repo's `main` branch (root). Pushing to `main`
redeploys the site automatically.

## Data

Breach data comes from the NYU MLab K-12 cybersecurity research project,
compiled from public records: state attorney general filings, news reporting,
vendor disclosures, and research datasets. Coverage reflects what has been
publicly reported; an empty search result means no publicly reported incidents
were found, not that nothing ever happened.
