# Earthquake Prediction using FOP-OPTICS Clustering

An implementation of the improved OPTICS clustering algorithm (FOP-OPTICS) applied to USGS seismic data from Chile, for discovering earthquake clusters with uneven densities.

**Accuracy improvement: 68.5% → 81.9%** over baseline clustering methods.

---

## The Problem

Standard clustering algorithms like DBSCAN use a fixed epsilon (eps) radius to define density, which means they struggle when a dataset has regions of very different densities — some tightly packed, others spread out. Seismic data has exactly this property: earthquake events cluster unevenly across fault lines, depths, and magnitudes.

Traditional methods either miss sparse clusters entirely or lump noise in with real events.

---

## The Approach

This project implements **FOP-OPTICS** (Finding Optimal Peaks in OPTICS), an enhanced version of the OPTICS algorithm described in:

> Tang et al. (2021). *An improved OPTICS clustering algorithm for discovering clusters with uneven densities.* Intelligent Data Analysis, 25(6), 1453–1471. https://doi.org/10.3233/ida-205497

FOP-OPTICS works in four steps:

1. **Generate the Reachability Plot** — OPTICS orders points by reachability distance, producing a 1D plot where valleys = dense clusters and peaks = boundaries or noise.
2. **Find Local Peak Points (LPP)** — Identify points where reachability distance is higher than their neighbours. These are candidate cluster boundaries.
3. **Compute Demarcation Points (DPS)** — Refine LPPs by selecting the highest peaks and ensuring good separation, limiting partitions to a set count (`Cnum`, default: 3).
4. **Extract Clusters** — Points between two demarcation points form a cluster; points outside are noise or separate clusters.

---

## Dataset

- **Source:** USGS Earthquake Hazards Program
- **Region:** Chile (a tectonically active region well-suited for seismic clustering)
- **Features used:** Latitude, Longitude, Depth, Magnitude, Timestamp
- **Preprocessing:** Unwanted features eliminated; data split into monthly subsets by year (2000–2010); each month saved as a separate CSV for temporal analysis

---

## Results

| Year | Clusters Found | Noise (%) |
|------|---------------|-----------|
| 2000 | 4 | 0.00% |
| 2001 | 4 | 0.00% |
| 2002 | 4 | 0.00% |
| 2003 | 4 | 0.00% |
| 2004 | 4 | 0.00% |
| 2005 | 4 | 0.00% |
| 2006 | 4 | 0.00% |
| 2007 | 4 | 0.00% |
| 2008 | 4 | 0.00% |
| 2009 | 4 | 0.00% |
| 2010 | 2 (partial year) | — |

Outputs per year include cluster scatter plots, cluster size distributions, spatial distribution maps, and correlation matrices. High-magnitude events are marked distinctly in visualisations.

**Overall classification accuracy: 81.9%** (vs 68.5% baseline using standard DBSCAN/k-means)

---

## Comparison Datasets

The FOP-OPTICS algorithm was also validated on several benchmark datasets from the UCI Machine Learning Repository:

| Dataset | Type | Size | Clusters |
|---------|------|------|----------|
| Seeds | Real | — | 3 |
| Iris | Real | — | 3 |
| SONAR | Real | — | 2 (Rock/Mine) |
| Wine | Real | — | 3 |
| DS1 | Artificial | 788 | 7 (well-separated) |
| DS2 | Artificial | 10,000 | 9 (arbitrary-shaped, with noise) |
| DS3 | Artificial | 8,000 | 8 (varied density, arbitrary-shaped) |
| DS5 | Artificial | 7,200 | 4 (density ratio 1:5:10:20) |
| Gaussian-500 | Artificial | 3,000 | 5 (density-varied, adjacent) |

---

## Stack

- **Language:** Python
- **Libraries:** Scikit-learn, Pandas, NumPy, Matplotlib
- **Platform:** Kaggle Notebooks

---

## References

1. Tang, C., Wang, H., Wang, Z., Zeng, X., Yan, H., & Xiao, Y. (2021). An improved OPTICS clustering algorithm for discovering clusters with uneven densities. *Intelligent Data Analysis*, 25(6), 1453–1471.
2. Ankerst, M., Breunig, M. M., Kriegel, H.-P., & Sander, J. (1999). OPTICS: Ordering Points to Identify the Clustering Structure. *ACM SIGMOD*, 49–60.
3. Deng, Z. et al. (2015). A scalable and fast OPTICS for clustering trajectory big data. *Cluster Computing*, 18(2), 549–562.
