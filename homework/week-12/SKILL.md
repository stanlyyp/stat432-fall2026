---
name: compare-clustering
description: Compare multiple unsupervised clustering algorithms and produce a concise, one-page report of the selected clustering. Use only when explicitly invoked by name.
---

# Clustering Analysis

Use only when the user says "invoke compare-clustering" or provides this file and asks you to follow it. Keep decisions evidence-based and the report concise.

## 1. Check the data
- Identify observations, features, identifiers, and known labels. Use any roles given at invocation first; otherwise infer them and state the assumption. Ask only if roles still cannot be determined.
- Report dimensions, variable types, missingness, duplicate or constant features, and possible outliers.
- Never use known labels to fit, tune, evaluate, or select; reserve them for final interpretation.

## 2. Prepare features
- Encode usable categoricals and handle missing values; explain exclusions and transformations.
- Standardize features for distance-based methods; consider skewness and outliers.
- For high-dimensional data, use PCA fitted on features only (PCA is not a clustering algorithm). Compare 5, 10, and 20 PCs (capped at n-1); report variance explained and whether the selected clustering changes (ARI between representations). Use the most stable representation and state the sensitivity.
- Use the same feature representation for all algorithms in the main comparison.

## 3. Try at least three algorithms
- K-means, hierarchical, and spectral clustering when feasible; K = 2 to min(8, n-1).
- K-means: n_init >= 50, seed = 42.
- Hierarchical: compare ward, complete, and average linkage.
- Spectral: nearest-neighbor affinity with n_neighbors = 5 and 10; adjust to sample size (keep n_neighbors below about n/3, and add 15 when n > 100).
- Save reproducible code.

## 4. Compare and select
- For each candidate, record silhouette (on the clustering feature space), cluster sizes, and stability.
- Stability: refit on 30 random 90% subsamples (seed = 42); compute ARI between the subsample labels and the full-data labels on the kept points; report mean and SD. Do not use seed reruns alone (deterministic methods always score 1.0).
- Reject degenerate solutions: singleton or tiny clusters (< 5% of n or < 3 points) unless clearly justified.
- Tie-breaking: (1) highest silhouette; (2) if within 0.02, prefer higher mean stability ARI; (3) then more balanced cluster sizes; (4) then the simpler method and smaller K. Note agreement between algorithms (ARI) as supporting evidence.
- Save the full candidate table (method, settings, K, silhouette, sizes, stability) as CSV.
- Only after selection, optionally compare clusters with known labels to interpret, not revise, the result.

## 5. Visualize the selected result
- One clear figure for the selected method only: a PCA scatterplot colored by cluster (sizes in the legend) or a readable dendrogram.
- Label axes with variance explained and settings. Note that 2D distances may not reflect full-dimensional distances.
- Add a compact size/profile table only if it adds essential information.

## 6. Report and verify
- Report the selected algorithm, K, PC choice and sensitivity, tuning rationale, key evidence (silhouette, stability), group characteristics, and limitations in plain English.
- Briefly name other algorithms considered and why the selected one was preferred, without displaying their results.
- **STRICTLY ONE PRINTED PAGE**, including all text, figures, captions, and tables. Present results for only the selected method.
- Put detailed comparisons and code outside the report.
- Verify labels were not used in selection, numbers match the plots, and the exported PDF is exactly one page (count pages; shrink layout and rebuild until it is).
