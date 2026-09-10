# Decoding movement priorities during human walking with multiple objectives

Analysis code for a study of how people trade off competing movement objectives while walking.
Twelve participants walked under all 27 combinations of three task prompts, each at three levels:

| Prompt | Levels | Code |
| --- | --- | --- |
| Speed | slow / typical / fast | `s0` `s1` `s2` |
| Accuracy (foot placement) | ignore / near / accurate | `a0` `a1` `a2` |
| Balance (perturbation) | zero / low / high | `b0` `b1` `b2` |

Each trial is labeled by its prompt combination, e.g., `s2a0b1`. The neutral
condition is `s1a0b0`.

The analysis addresses the following questions:

- **1a.** Can a participant's movement in any condition be expressed as a weighted
  combination of three *basis* conditions, each isolating one objective?
- **1b.** How much do those recovered weights agree with what participants say they
  were prioritizing?
- **2.** Do the three prompts act additively on measured gait, or do they interact?

## Notebooks

Run both notebooks from the repository root, in the order below. Everything they
need is in `data/`.

| Notebook | What it does |
| --- | --- |
| `01_estimate_objective_weights.ipynb` | Estimates objective weights per participant and condition, compares feature sets by goodness of fit, and produces the main figures |
| `02_prompt_effects_lmm.ipynb` | Linear mixed models testing additive vs. interacting prompt effects on gait outcomes |

The notebooks are committed without cell outputs, so run them to regenerate the
tables and figures.

`00_reference_extract_movement_features.ipynb` is not part of that sequence. It
documents how the committed feature files, and other dimensionality-reduction
tests, were produced: the per-participant PCA/NMF of joint kinematics, EMG, or
spatiotemporal gait metrics, the number of components, the left/right averaging,
and the excluded conditions. The two notebooks above read its committed output,
so they do not require it.

It runs as committed in its default `MODE = "gait_metrics"`, which regenerates
`data/decomp_gait_metrics_pca_fixed5.csv`. Under `MODE = "kinematics"` or
`MODE = "emg"` it reports which raw files it could not find, and the remaining
cells do nothing, so no committed file is overwritten.

## Data

**Included** — everything the notebooks read lives directly in `data/`.

- `data/data_BMH*.xlsx` — spatiotemporal metrics and energy expenditure per
  participant and condition, one workbook per participant. The notebooks read
  energy expenditure from the mass-normalized `EE Wkg` column.
- `data/Subjective_Responses.xlsx` — per-trial self-reported priorities, also
  used to map prompt combinations to trial IDs.
- `data/decomp_gait_metrics_pca_fixed5.csv`,
  `data/decomp_kinematics_pca_fixed5_kin_synergy.csv`,
  `data/decomp_emg_pca_fixed5_synergy.csv` — the three feature files behind the
  reported results, produced by the reference notebook. Five components per
  participant, fit independently within each.
- `data/weights_gait_metrics.xlsx` — the reported objective weights, exported by
  `01_estimate_objective_weights.ipynb` with `selected_model = "SPT-PCA-5"`.

**Not included**

Stride-level joint kinematics and surface EMG (one parquet file per participant)
are not distributed here: the files are large, and they are individual-level
recordings rather than derived summaries. They are available on request.

The reference notebook looks for them at the path in the `MO_RAW_DATA_DIR`
environment variable, which should contain one folder per participant holding
`<subject>_cleaned_strides.parquet`, `<subject>_cleaned_strides_extra.parquet`
and an `*emg*.parquet` file.

Intermediate products of exploratory runs (different component counts, NMF
variants) are not versioned.

## Requirements

Developed on **Python 3.9.20**. Install with:

```bash
pip install -r requirements.txt
```

## Outputs

`01_estimate_objective_weights.ipynb` shows its figures inline and writes the
weights table to `data/weights_<modality>.xlsx`, overwriting the committed file
when the default `selected_model = "SPT-PCA-5"` is left in place.
`selected_model`, set at the top of the weights section, controls which feature
set carries into the figures and the export.

If run, the reference notebook writes `data/decomp_<mode>_<method>_<config>.csv`
and `data/results_claim_vaf_summary.csv`, the variance-accounted-for evidence
table.

## Contact

Patrick Slade — slade@seas.harvard.edu
