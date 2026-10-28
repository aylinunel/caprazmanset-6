# Çapraz Manşet 6.0

Static export of Çapraz Manşet 6.0: the built site, its data and the pipeline that produces them.
Built from [`aylinunel/caprazmanset-oct26`](https://github.com/aylinunel/caprazmanset-oct26) branch
`claude/capraz-5-pipeline` at `b484582`; that repository remains the place where the code is developed.

| | |
|---|---|
| Window | 1 September – 27 October 2026 (Türkiye time); 28 October was a partial day and is left out |
| Articles | 194,383 (163,313 with a lede), 33 outlets, no sampling |
| Events | 4,362 (4,222 automatic, 140 hand-checked and unchanged) |
| Active outlets | 16 (4 left, 2 middle, 10 right by the supplied placements) |
| Source | `backfill-data` branch of `aylinunel/turkish-news-scraper`, runs up to 40610337809 |

## Open the site

`public/` is the whole site: `index.html` plus `public/data/*.bin` (gzip JSON packs: `core`, one
headline pack and one lede pack per month). Serve the folder with any static server, for example

```bash
cd public && python3 -m http.server 8000   # then open http://localhost:8000
```

Opening `index.html` straight from disk does not work: the page fetches its data files.

## Rebuild or extend

```bash
python -m pip install -r pipeline/requirements.txt     # Python 3.11+, Node 22
cd pipeline
python -m capraz_pipeline run --from <collector export> --since 2026-09-01 --site ../public
```

Each article is stored once, under the day of its first collected copy; a later copy can only fill a
missing lede. Older dates or a scheduled live run use the same command with a different
`--from`/`--since`; see [pipeline/README.md](pipeline/README.md) and
[ADR-0002](docs/adr/0002-analysis-pipeline.md). `docs/build-report.json` holds this build's
validation figures (classifier cross-validation, clustering agreement with the reference events,
topic coherence, sizes, run time).

## Layout

```
public/            built site (publish this)
pipeline/          analysis pipeline + offline tests
shared/analysis/   the headline scorer shared by the site and the pipeline
site/              site sources (renderer, views, data loader)
data/reference/    reference labels, hand-checked events and editorial layer the algorithms extend
docs/              ADR-0002, the build report, and biases-and-detection.md (bias concepts and detection methods)
```

## Boundaries

Machine results on this site (scores, events, blind spots, speed, headline–lede gap, similarity map,
topic model) are labelled "otomatik" and documented on the Yöntem page. Outlet placements, ownership
notes, narrative coding and hand-checked events are supplied material carried unchanged, not
independently verified. Nothing here is published to the editorial app; imports there stay drafts.
