# DIMS Dashboard — reference study

The dashboard everything else is measured against: one recorded session with
its signals, its annotations, and every analysis DIMS ships. If you want to see
what a DIMS dashboard is before building one, start here.

## Run it

```sh
python serve.py            # then open http://localhost:8000
python serve.py 8080       # if that port is taken
```

Nothing to install, and nothing is fetched from the internet. The dashboard is
plain files, `serve.py` uses only the Python standard library, and every
library it draws with is in `vendor/lib/` rather than on a CDN — so it opens on
a conference network, or on none at all. Everything it needs is in this
repository, video included.

## What you get

One session, `3120`, with four signals: `bodysync` and three `neuralsync`
channels. Pick a point on the timeline and every tab narrows to a window around
it — the window is `defaultWindowSize` seconds, 5 here.

| tab | what it draws |
|---|---|
| **Time series** | all four signals on a shared time axis, beside the video |
| **RQA** | a recurrence plot per signal — when the signal returns to a state it held before — with recurrence rate, determinism and laminarity tracked over time |
| **Cross-RQA** | the same between `bodysync` and `neuralsync`. Structure *off* the main diagonal is a lagged relationship, which is the point of the analysis |
| **Cross-Wavelet** | coherence between the two by time and timescale, with a 95% chance level from a Monte Carlo null. Coherence has no meaningful zero, so the tab reports the share of cells that beat chance rather than the raw value alone |
| **ELAN** | annotation tiers aligned to the video |

## The data

| file | what it is |
|---|---|
| `assets/videos/3120.mp4` | the recording |
| `assets/timeseries/3120_{signal}.csv` | one signal per file, columns `Time` and a value |
| `assets/transcripts/3120_transcript.json` | the diarised transcript |
| `assets/elan/3120.eaf` | the annotations |
| `assets/{rqa,crqa,crosswavelet}/` | analysis output — regenerable, see below |

The time series are the study's raw inputs: they came from the original DIMS
work cited below and are not derived from anything else in this repository.
The analysis directories are, and can be rebuilt from the CSVs at any time.

Each analysis writes two files: a rounded, reduced `.json` for the browser to
draw, and a full-resolution `.npz` beside it. **Analyse from the `.npz`** — the
JSON is a picture.

## Rebuild the analyses

The committed assets were produced by DIMS core **1.0.0**, the version named in
`dims-case.json`, and this dashboard reads that payload format. Rebuilding with
a different core rewrites them in whatever format *it* produces, which is how a
working dashboard turns into empty panels — so pin the same version:

```sh
git clone --depth 1 --branch v1.0.0 https://github.com/dims-network/dims /tmp/dims
pip install -e /tmp/dims/packages/dims-analysis
python build_assets.py --check   # what would run
python build_assets.py           # run it
```

Or run the analyses directly: `dims-analysis run --config config.json`.

## Layout

```
config.json          which signals exist and which analyses run
index.html           the page; loads the vendored core
serve.py             local server (stdlib only)
build_assets.py      rebuild the analyses
vendor/dims-core/    pinned copy of the shared core — never edit
vendor/dims-tabs/    one file per tab, pinned with it
vendor/lib/          third-party libraries, so nothing loads from a CDN
assets/              data in, analysis out
```

`vendor/` is a pinned copy of [dims](https://github.com/dims-network/dims), taken
from the release named in `dims-case.json`. A fix belongs upstream and arrives
here as a version bump (`dims-case sync`), never as an edit here.

## Citation

Miao, G. Q., Trujillo, J., Bulls, L. S., Thornton, M. A., Dale, R., & Pouw, W.
(2025). *DIMS Dashboard for Exploring Dynamic Interactions and Multimodal
Signals.* CogSci 2025.
