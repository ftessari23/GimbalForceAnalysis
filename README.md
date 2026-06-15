# Force Analysis – Gimbal Experiment

**Author:** Federico Tessari, PhD  
**Affiliation:** Mechanical Engineering Department, MIT  
**Contact:** ftessari@mit.edu  
**Last Update:** May 7, 2026

---

## Overview

This repository contains the full MATLAB analysis pipeline for the **Gimbal Experiment**, designed to study **maximum voluntary force (MVF)** and **force coordination** under different arm configurations and task conditions.  
The script reproduces all figures and statistics reported in the related manuscript (**Figs. 2–5** and **Supplementary Figs. 4-13**).

The analysis is fully automated: simply run `main.m` to import, process, and visualize data for all subjects.  
All required data are included in this repository.

---

## What the Script Does

The script executes the following stages:

1. **Setup and Formatting**  
   Sets MATLAB figure, font, and interpreter defaults for publication-quality plots.

2. **Data Loading**  
   - Imports per-subject MVF data (`S#_MVF_i.txt`, `S#_MVF_f.txt`)  
   - Imports all trial data for both conditions (`CN`, `UCN`) and all tasks (`FLAT`, `LCK`, `FEX`, `FEXURD`, `ULCK`)  
   - Automatically trims the last 3 s of each trial for steady-state analysis  

3. **Computation of Force Metrics**  
   - Calculates instantaneous and median total force magnitude ‖F‖  
   - Computes per-trial, per-subject, and pooled values across repetitions  

4. **Statistical Analysis**  
   - Performs 2-way ANOVA (Condition × Task) per subject with Bonferroni-corrected post-hoc tests  
   - Computes Cohen’s *d* effect sizes for relevant comparisons  
   - Conducts paired *t*-tests between Pre- and Post-MVF conditions  

5. **Visualization**  
   - Single-Subject Analysis (Figs. 2, Supplementary Figs. 4–12): time-series plots for total and component forces (Fx, Fy, Fz)  
   - Per-Subject Boxplots (Fig. 3): distributions across tasks and conditions  
   - Histogram Summaries (Fig. 4): median differences between configurations  
   - Pre vs Post Comparisons (Fig. 5): boxplots aligned by subject with significance reporting  

---

## Folder Structure

```text
.
├── Data/                      # Folder containing all raw text files
│   ├── S1_MVF_i.txt
│   ├── S1_MVF_f.txt
│   ├── S1_CN_FLAT_T1.txt
│   └── ...
├── Source_Data.xlsx           # Processed data used for figure/source verification
├── main.m                     # Main analysis script
└── README.md                  # This file
```

---

## Input File Naming Convention

| Type | Example | Description |
|------|---------|-------------|
| MVF (Initial) | `S1_MVF_i.txt` | Pre-experiment maximum voluntary force |
| MVF (Final) | `S1_MVF_f.txt` | Post-experiment maximum voluntary force |
| Trial | `S1_CN_LCK_T3.txt` | Subject 1, Condition CN (Undesired), Task LCK, Trial 3 |

Each file must contain **time-series data** with at least the first three columns being **Fx, Fy, Fz [N]**.

---

## Key Variables

| Variable | Meaning |
|----------|---------|
| `condition` | `{'CN','UCN'}` — Undesired / Preferred arm configurations |
| `task` | `{'FLAT','LCK','FEX','FEXURD','ULCK'}` |
| `fs` | Sampling frequency (default 50 Hz) |
| `idx_min`, `idx_max` | Index range used for 3 s steady-state window |
| `MVF_tot_i`, `MVF_tot_f` | Subject-level MVF stats (mean / std of ‖F‖) |
| `f_tot_med` | Median force magnitude per trial |
| `Tmed` | CN − UCN difference in median force |
| `PostHoc_PrePost` | Table summarizing Pre / Post MVF results |

---

## Output and Figures

When run, the script automatically generates:

- Per-subject figures — time-series plots (Undesired vs Preferred)  
- Boxplots — task-wise distributions across conditions  
- Histograms — median differences in absolute / percentage terms  
- Summary table — Pre vs Post MVF differences with paired *t*-test p-values  
- ANOVA post-hoc tables — stored as variables (`resultsTable{subj}`) with effect sizes  

---

## Dependencies

- MATLAB R2022a or newer  
- Statistics and Machine Learning Toolbox (for `anovan`, `multcompare`, `ttest`)  
- No particular hardware required  
- Tested configuration: Windows 11, Intel i7 11th Gen CPU, 16 GB RAM  

---

## Performance and Setup

| Parameter | Detail |
|-----------|--------|
| Typical running time | < 300 seconds (< 5 minutes) |
| Installation time | None (after MATLAB is running) |
| Hardware requirements | None beyond a standard laptop/desktop |
| Operating system tested | Windows 11 |
| Raw data availability | All data files included in `Data/` |
| Processed data availability | All processed data used in figures are also provided in `Source_Data.xlsx` |

---

## How to Run

1. Ensure MATLAB is installed and running.  
2. Place all data files inside a folder named `Data` in the same directory as `main.m`.  
3. Open `main.m` in MATLAB.  
4. Press **Run**, or execute:

```matlab
main
```
