# Craniofacial Landmark Morphometrics

Geometric morphometric analysis of 3D craniofacial landmarks in mouse models, comparing mandible and maxilla morphology across genetic backgrounds (WT, Splotch, Tcof1, and Splotch;Tcof1 double mutants).

## Overview

This project performs landmark-based morphometric analysis on 3D coordinate data exported from ORS (Object Research Systems) sessions of micro-CT scans. The pipeline aligns specimens using Generalized Procrustes Analysis (GPA), quantifies shape variation, and tests for statistically significant differences between genotype groups.

## Analysis Pipeline

The analysis is contained in [B-Morphometrics.ipynb](B-Morphometrics.ipynb) and follows these steps:

1. **Data Loading** — Reads 3D landmark coordinate CSV files (X, Y, Z per landmark point) exported from ORS.
2. **Landmark Selection** — Filters to mandible (points 1–16) or maxilla (points 17–26) landmarks.
3. **Procrustes Alignment** — Performs GPA via `morphops` to remove differences in position, scale, and orientation.
4. **Shape Visualization** — Interactive 3D scatter plots of aligned landmarks colored by genotype, with napari-based 3D model viewing.
5. **Variability Analysis** — Computes per-landmark and per-specimen displacement from the WT mean shape, identifying outliers beyond 5 SD.
6. **Dimensionality Reduction** — PCA, UMAP, and t-SNE projections of the aligned shape space, with K-means clustering.
7. **Statistical Testing** — Permutation tests (via `sklearn`) comparing classifier accuracy on real vs. shuffled group labels to assess significance of shape differences.

## Input Data

CSV files with columns: `SERIES`, `COLOR`, `FRAME`, `X`, `Y`, `Z`. Each file represents one specimen, and each row is a landmark point. Files are named with genotype and phenotype information (e.g., `811004-3_Mut_Mild.csv`).

## Output

Results are written to `results/` (mandible) or `maxilla_results/`:

- `R_vs_WT.html` — Bar chart of mean displacement from WT per specimen
- `R_vs_WT_Interesting.html` — Same chart, faceted by outlier threshold
- `Above5SD.csv` / `Below5SD.csv` — Specimens above/below 5 SD from WT mean
- `PCA_Clustered.html` — PCA plot with K-means cluster assignments
- `UMAP_distance.html` — UMAP distance from WT centroid with permutation test p-values

## Dependencies

- numpy
- pandas
- scikit-image
- scikit-learn
- napari
- morphops
- plotly
- umap-learn
- matplotlib
