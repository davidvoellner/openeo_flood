# Flood Monitor & Water Depth Pipeline

One pipeline from raw inputs to water-depth maps: **FABDEM DEM** (Google Earth
Engine) + **GFM flood extent** (EODC STAC) + **FLEXTH water depth/level**
([FLEXTH fork](https://github.com/kwundram2602/FLEXTH), based on
[hyunholee26/FLEXTH](https://github.com/hyunholee26/FLEXTH)).
The app is driven by a
single `config.yaml` and a Streamlit dashboard.

## Quick start

```bash
uv sync

uv run flood-pipeline dashboard
```

## Steps

The runner executes six steps in order, each configurable and skippable:

- **dem** – downloads the FABDEM terrain model for the AOI from Google Earth Engine.
- **gfm** – loads GFM flood extents from the EODC STAC catalog and aggregates them over the chosen time range.
- **osm** – fetches roads and railways via OSMnx and intersects them with the flood extent to get flooded kilometres.
- **flexth** – resamples DEM and flood mask onto a common grid and runs FLEXTH to estimate water level and depth.
- **population** – combines WorldPop with the depth raster to count affected people.
- **ghsl** – combines GHSL built-up data with the depth raster to estimate building damage.

Each project lives in `projects/<name>/` with its own `config.yaml` and AOI, and
all results are written to that project's output folder.

## Data flow

Where each step gets its data from and what it writes to `outputs/`:

![Flood pipeline data flow](assets/flood-pipeline-data-flow.png)

An explorable version with search, relationship tracing and guided views lives in
[`assets/pipeline_dataflow.html`](assets/pipeline_dataflow.html) — clone and open it in a
browser, GitHub cannot render it inline.
