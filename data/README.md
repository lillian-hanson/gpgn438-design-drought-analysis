# Data

The large InSAR products are not stored in this repository. The notebooks read and write a working
folder set by the environment variable `SLV_DATA_DIR` (default `C:/insar`; keep the path short on Windows).

Shared folder with the files: TODO add link.

| File | What it is |
|---|---|
| `wells.csv`, `wells_all.csv`, `well_obs.csv` | Written by step 2. Wells with data, all wells found, and every water-level observation (columns: source, kind, id, time, depth_ft; depth is below land surface) |
| `test_pairs.csv`, `full_pairs.csv` | HyP3 job tables, written by step 1 |
| `hyp3_test/`, `hyp3_full/` | Downloaded unwrapped phase and coherence GeoTIFFs |
| `wells_scored.csv` | The 91 usable wells with coordinates, swing, tier and depth class (written by step 1) |
| `well_results_plane.csv` | Per-well results after plane removal: annual amplitude, standard error, slope against head, residual |
| `well_timeseries_plane_mm.csv` | Date by well displacement in mm of line-of-sight motion, uplift positive, plane removed, first date set to zero |
| `extracted.npz` | Raw phase, coherence and component arrays at the well pixels, for reruns without the GeoTIFFs |

## Credentials

- NASA Earthdata Login bearer token for HyP3, typed into a hidden prompt in the step 1 notebook.
- `USGS_API_KEY` (required) and `CDSS_API_KEY` (optional) in a `.env` file at the repo root, read by step 2.
