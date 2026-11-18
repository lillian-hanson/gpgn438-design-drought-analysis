# Geophysics Capstone Design Project (GPGN 438)
### Lillian Hanson, Aucklynn Sacco
### Advisor: Ryan Venturelli

The original proposal was a historical evaluation of Colorado groundwater, precipitation and aquifer
volume patterns and their effects on agriculture, including how ENSO cycles affect water supply.
The project now focuses on the San Luis Valley (SLV), Colorado, and one question:

> Can Sentinel-1 InSAR plus well water levels detect seasonal ground motion from groundwater
> pumping in the San Luis Valley?

Status (project handoff, Nov 13, 2026): yes at one site, not at most wells. After removing a plane
from each interferogram date, wells more than 25 km from DWR:31512 have a median annual amplitude
of 1.6 mm and a 95th percentile of 3.8 mm (the detection threshold). DWR:31512 and DWR:31576 show
about 16.7 and 16.6 mm. All amplitudes are line-of-sight millimetres. Whether that footprint is real
ground motion or a long-wavelength atmospheric effect is still open. The report has the authoritative
results.

## Repository layout

| Path | Contents |
|---|---|
| `InSAR_processing/` | The three notebooks of the SLV InSAR pipeline |
| `inital_data_exploration/` | Early statewide exploration: groundwater, streamflow, snowpack, ENSO |
| `data/` | Description of the data files. Large files are not in git (see `data/README.md`) |
| `environment.yml` | Conda environment |
| `references.bib` | Bibliography |

## Setup

```bash
conda env create -f environment.yml
conda activate design
export SLV_DATA_DIR=/path/to/working/folder    # Windows: setx SLV_DATA_DIR C:\insar
jupyter lab
```

If `SLV_DATA_DIR` is not set, the notebooks use `C:/insar`.

Credentials (never commit them): an Earthdata Login bearer token for HyP3 (typed at a hidden
prompt), and `USGS_API_KEY` plus optional `CDSS_API_KEY` in a `.env` file at the repo root.
See `data/README.md`.

## Running the InSAR pipeline

The notebooks do not run in numerical order, because the scene notebook needs the well list
that the well notebook makes. Run them in this order:

1. `slv_step1_scenes_and_hyp3_test.ipynb`, from the top through "Step 1.4: find scenes and pick one track".
2. `slv_step2_wells.ipynb`, all cells. Writes `wells.csv`, `wells_all.csv`, `well_obs.csv`.
3. `slv_step1_scenes_and_hyp3_test.ipynb`, the rest: frame coverage, test pairs, HyP3 submission,
   download, coherence at wells. Writes `wells_scored.csv`.
4. `slv_step3_process.ipynb`: reads phase at the wells and reference pixel, closure check, network
   inversion, plane removal, annual-cycle fit. Writes `well_results_plane.csv`,
   `well_timeseries_plane_mm.csv`, `extracted.npz`.

HyP3 jobs spend credits (10 per product). Nothing is submitted until `SUBMIT = True` or
`FULL_SUBMIT = True` is set in the step 1 notebook.

## Caveats

- The only noise handling is a plane removed on each date. There is no troposphere correction,
  so noise is probably underestimated.
- The 3.8 mm threshold comes from 57 wells, a small and spatially correlated sample.
- The record covers about one seasonal cycle, so a repeating atmospheric signal and a repeating
  pumping signal cannot be separated by timing alone.
- The sign convention (uplift positive) follows the GAMMA phase convention and has not been
  checked against a known uplift or subsidence signal.
- "Deep" and "shallow" are a depth proxy (deeper than about 131 ft / 40 m), not measured confinement.
