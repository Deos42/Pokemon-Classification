# Classifying Pokémon using DBSCAN From Scratch

## Overview

This project implements the **DBSCAN (Density-Based Spatial Clustering of Applications with Noise)** algorithm from scratch using NumPy.

Unlike centroid-based methods such as K-Means, DBSCAN clusters data based on **density connectivity**, enabling it to:

- Discover arbitrarily shaped clusters
- Automatically detect outliers (noise points)
- Avoid specifying the number of clusters

The model is applied to Pokémon combat statistics using three numerical features:

- Attack
- Defense
- Speed

Clustering is performed in 3D standardized feature space.

---

## Dataset & Feature Engineering

From the Pokémon dataset, the following numerical attributes were selected:

- `Attack`
- `Defense`
- `Speed`

The feature matrix:

X ∈ ℝ^(n_samples × 3)

Each row represents a Pokémon and each column represents a combat statistic.

---

## Preprocessing

Since DBSCAN is distance-based, proper scaling is essential.

The features were standardized using **StandardScaler**:

X_scaled = (X − μ) / σ

Where:

- μ = feature mean
- σ = feature standard deviation

This ensures:

- Mean = 0
- Standard deviation = 1
- Balanced contribution of all features to Euclidean distance

Clustering is performed entirely in this standardized 3D space.

---

## Algorithm Implementation

The DBSCAN algorithm was implemented manually without using `sklearn.cluster.DBSCAN`.

### 1. Region Query

For a given point, all neighbors within radius `eps` are found using Euclidean distance:

distance(x_i, x_j) = ||x_i − x_j||₂

### 2. Core Point Definition

A point is classified as a **core point** if:

number_of_neighbors ≥ minPts

(Neighbors include the point itself.)

### 3. Cluster Expansion

If a point is a core point:

- A new cluster ID is created
- Density-connected neighbors are iteratively added
- Border points are assigned to the cluster
- Unreachable points are labeled as noise

### Label Convention

- 0 → Unvisited
- -1 → Noise
- 1, 2, … → Cluster IDs

---

## Hyperparameters

- eps = 0.4
- minPts = 5

Since data is standardized, `eps` operates in normalized feature space.

Parameter sensitivity:

- Smaller `eps` → More noise
- Larger `eps` → Fewer, larger clusters
- Higher `minPts` → Stricter density threshold

---

## Results

- DBSCAN identified multiple density-based clusters in the 3D scaled feature space.
- Outliers were automatically detected and labeled as noise.
- Clusters were computed using all three features (`Attack`, `Defense`, `Speed`).

For visualization, results were plotted using:

- X-axis → Scaled Attack
- Y-axis → Scaled Defense

Note: Speed influences clustering but is not directly visible in the 2D projection.

---

## Computational Complexity

This implementation uses naive neighbor search:

- Time complexity: O(n²)
- Space complexity: O(n)

Distance calculations are vectorized using NumPy, but no spatial indexing (KD-Tree / Ball-Tree) is used.

---

## Tech Stack

- Python
- NumPy
- Pandas
- Matplotlib
- scikit-learn (StandardScaler only)

---

## Key Takeaways

This project demonstrates:

- Strong understanding of density-based clustering
- Implementation from first principles
- Correct preprocessing for distance-based models
- Handling of core, border, and noise points
- Awareness of computational trade-offs

---

## Future Improvements

- Implement KD-Tree for sub-quadratic neighbor search
- Perform eps tuning using k-distance graph
- Compare performance against sklearn’s DBSCAN
- Extend to higher-dimensional clustering analysis
