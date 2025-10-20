
# Clustering Exam Prep (Weeks 9–10) — Summary, Calculations, and Practice

This document compiles key concepts from Week 9 (Partitional and Density‑based Clustering), Week 10 (Hierarchical Clustering), and Week 12 exam notes. Formatting avoids heavy bold; structure is provided by headings and tables.

## I. Summary of Key Concepts and Algorithms

This summary focuses on Cluster Analysis, covering Hierarchical Clustering (Week 10) and Partitioning/Density‑Based Clustering (Week 9).

### A. General Clustering Concepts

| Concept | Definition/Explanation | Key Takeaway / Caveats |
| :--- | :--- | :--- |
| Cluster Analysis | Group objects so items within a group are similar and items across groups are dissimilar. | Goal: maximize inter‑cluster distances and minimize intra‑cluster distances. |
| Proximity Matrix | Matrix of pairwise similarity or distance. Used heavily in hierarchical clustering. | If diagonal is 0 -> distance matrix (smaller = closer). If diagonal is 1 -> similarity matrix (larger = closer). |
| Types of Clustering | Partitional vs Hierarchical. Other variants include fuzzy clustering and partial clustering. | Partitional assigns each point to exactly one cluster; hierarchical gives a tree (dendrogram). |
| Cluster Types | Well‑separated, contiguity‑based (nearest‑neighbor), density‑based. | Contiguity and density definitions help with irregular or intertwined shapes. |

### B. Hierarchical Clustering (HAC)

Definition: Produces nested clusters visualized by a dendrogram.

| Type | Process | Notes |
| :--- | :--- | :--- |
| Agglomerative (bottom‑up) | Start with singletons; repeatedly merge the closest two clusters until one remains. | Requires a precomputed proximity matrix. |
| Divisive (top‑down) | Start with all points; recursively split clusters. | Conceptual opposite of agglomerative. |
| Inter‑cluster proximity (linkage) | How to update distances after a merge. | Merge choice (closest pair) is distinct from update rule (single/complete/average/Ward). |

Inter‑cluster proximity (distance‑matrix view):

| Method | Update Rule | Strengths and limitations |
| :--- | :--- | :--- |
| MIN / Single Link | Distance between two clusters is the minimum pairwise distance across them. | Handles non‑elliptical shapes; sensitive to noise (chaining). |
| MAX / Complete Link | Distance is the maximum pairwise distance. | Tighter, less chaining; biased to compact/globular clusters. |
| Group Average | Average of all pairwise distances across the two clusters. | Compromise between single and complete. |
| Ward’s Method | Merge causing the smallest increase in total squared error (SSE). | Often a strong initializer for K‑means. |

### C. K‑means Clustering

Definition: Partitional clustering with a fixed number of clusters K, each represented by a centroid.

| Concept | Explanation | Notes |
| :--- | :--- | :--- |
| Core loop | Assign points to nearest centroid; recompute centroids; iterate to convergence. | Initialization strongly affects results. |
| Objective | Minimize the sum of squared error (SSE): sum over points of squared distance to its cluster centroid. | Smaller SSE indicates tighter clusters (given K). |
| Initialization issues | Random starts may hit poor local minima. | Use multiple restarts, K‑means++, or hierarchical seeds. |
| Limitations | Struggles with different sizes/densities/non‑globular shapes; sensitive to outliers. | Consider DBSCAN or hierarchical when shapes/densities vary. |

### D. DBSCAN (Density‑Based)

Definition: Clusters are dense regions separated by sparse regions.

| Term | Definition | Notes |
| :--- | :--- | :--- |
| Parameters | eps (ε): neighborhood radius; MinPts: minimum neighbors to be dense. | Strongly determines results. |
| Core point | Has at least MinPts points (including itself) within ε. | Forms cluster interiors. |
| Border point | Not core but within ε of a core. | Assigned to that core’s cluster. |
| Noise point | Neither core nor border. | Naturally discarded; robust to outliers. |
| Strengths/limits | Good for irregular shapes and outlier resistance; struggles with varying densities or very high dimensions. | Scaling and distance choice matter. |

### E. Exam Information (Week 12 Review)

Date and venue: 3 November 2025 at 14:00, Union Hall.

| Item | Details |
| :--- | :--- |
| Format | 6 questions total: 6 MCQs (Q1: 40 marks) and 5 calculation questions (Q2–Q6: total 80 marks). Focus is MCQ and calculations; memorizing long definitions is not required. |
| Duration | 2 hours plus 15 minutes reading time. |
| Allowed materials | Non‑programmable calculator; one A4 page of handwritten notes (two‑sided). Write down key equations (e.g., probability, SSE, silhouette). |

## II. Calculation Section: Reference Procedures

### 1. K‑means SSE
SSE = \(\sum_{i=1}^{K} \sum_{x \in C_i} \text{dist}(x, m_i)^2\).

### 2. HAC Linkage Updates (distance matrices)
Single link: \(D(C_i, C_j) = \min\{ \text{dist}(p_a, p_b) : p_a \in C_i, p_b \in C_j \}\).  
Complete link: \(D(C_i, C_j) = \max\{ \cdot \}\).  
Group average: mean of all pairwise distances across clusters.  
Ward: choose merge with smallest increase in SSE.

### 3. Similarity matrices
When using similarity rather than distance, the merge rule flips: choose the largest similarity at each step; update rules use max/min accordingly.

## III. Subjective Questions (Model Answers)

1) K‑means limitations and how DBSCAN/hierarchical address them: initialization/local minima, non‑globular shapes, outliers, and needing K; DBSCAN handles shapes/outliers; hierarchical lets you choose K post‑hoc via cutting the dendrogram.

2) Agglomerative vs divisive: bottom‑up merging vs top‑down splitting. Two agglomerative decisions at each step: which pair to merge (closest by current proximity) and how to update proximities (linkage rule).

3) DBSCAN point types (core, border, noise) and robustness to noise: focus on dense neighborhoods; points outside any dense region are ignored.

## IV. Multiple Choice Questions (Answers at right)

1. Which linkage handles non‑elliptical shapes but is noise‑sensitive?  Answer: C (Single link).  
2. K‑means minimizes which objective?  Answer: B (SSE).  
3. With a similarity matrix, which value indicates the closest pair to merge?  Answer: D (largest similarity).  
4. In DBSCAN, a non‑core point within ε of a core is called what?  Answer: D (border point).  
5. What choice most strongly influences K‑means results?  Answer: C (initial centroids).  
6. Strength of hierarchical vs K‑means?  Answer: D (don’t need K in advance).

## V. Worked HAC Examples From Lecture Matrix

Distance matrix (P1–P6) as provided:

|   | P1 | P2 | P3 | P4 | P5 | P6 |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| P1 | 0 | 0.24 | 0.22 | 0.37 | 0.34 | 0.23 |
| P2 | 0.24 | 0 | 0.15 | 0.20 | 0.14 | 0.25 |
| P3 | 0.22 | 0.15 | 0 | 0.15 | 0.28 | 0.11 |
| P4 | 0.37 | 0.20 | 0.15 | 0 | 0.29 | 0.22 |
| P5 | 0.34 | 0.14 | 0.28 | 0.29 | 0 | 0.39 |
| P6 | 0.23 | 0.25 | 0.11 | 0.22 | 0.39 | 0 |

1) First merge: P3 with P6 at distance 0.11 (distance matrix -> smallest non‑zero).  
2) Single‑link update for C1={P3,P6} vs P1: min(0.22, 0.23) = 0.22.  
3) Complete‑link update for C1 vs P5: max(0.28, 0.39) = 0.39.  
4) Group‑average update for CA={P3,P6,P4} vs P1: (0.22+0.23+0.37)/3 ≈ 0.273.  
5) Group‑average update for CB={P2,P5} vs P1: (0.24+0.34)/2 = 0.29.

## VI. Five New Calculation Practice Questions (with answers and explanations)

### Q1. HAC merge sequence under different linkages

Distance matrix for A, B, C, D:

|   | A | B | C | D |
| :--- | :--- | :--- | :--- | :--- |
| A | 0 | 2 | 4 | 5 |
| B | 2 | 0 | 3 | 6 |
| C | 4 | 3 | 0 | 1 |
| D | 5 | 6 | 1 | 0 |

a) Using single link, list merges and merge distances until one cluster remains.  
b) Using complete link, list merges and merge distances.

Answer.

a) Single link:  
• Closest pair: C–D at 1 -> merge {CD}.  
• Distances from {CD} to A and B use min rule: D({CD},A)=min(4,5)=4; D({CD},B)=min(3,6)=3; D(A,B)=2.  
• Next smallest is A–B at 2 -> merge {AB}.  
• Final merge between {AB} and {CD} at min(3,4)=3.  
Sequence: C–D (1), A–B (2), {AB}–{CD} (3).

b) Complete link:  
• Closest pair: C–D at 1 -> merge {CD}.  
• Distances with complete link: D({CD},A)=max(4,5)=5; D({CD},B)=max(3,6)=6; D(A,B)=2.  
• Next smallest is A–B at 2 -> merge {AB}.  
• Final merge between {AB} and {CD} at max(5,6)=6.  
Sequence: C–D (1), A–B (2), {AB}–{CD} (6).

Explanation: Single link chains via minimum edges; complete link waits until the worst‑case pair is close, hence the higher final merge distance.

### Q2. One iteration of K‑means and SSE

1D points: 1, 2, 3, 10, 11, 12. K=2. Initial centroids: m1=2 and m2=11.  
a) Assign each point to its nearest centroid.  
b) Recompute centroids.  
c) Compute SSE for the resulting clusters.

Answer.

a) Assignments: {1,2,3} to m1=2; {10,11,12} to m2=11.  
b) New centroids: mean({1,2,3})=2; mean({10,11,12})=11. (Already stable.)  
c) SSE: For {1,2,3} around 2: (1−2)^2 + (2−2)^2 + (3−2)^2 = 1 + 0 + 1 = 2.  
For {10,11,12} around 11: 1 + 0 + 1 = 2. Total SSE = 4.

### Q3. Silhouette coefficient for a single point

A point i lies in cluster A. Its average distance to all other points in A is a_i = 1.5. The smallest average distance from i to another cluster is b_i = 2.1.  
Compute its silhouette s(i) = (b_i − a_i) / max(a_i, b_i).

Answer.

s(i) = (2.1 − 1.5) / max(1.5, 2.1) = 0.6 / 2.1 ≈ 0.286.  
Interpretation: modestly positive cohesion/separation.

### Q4. DBSCAN core/border/noise classification (1D)

Points: 0.0, 0.2, 0.4, 5.0, 5.1. Parameters: eps = 0.3, MinPts = 3.  
Classify each point as core, border, or noise (use closed ball within eps and count the point itself).

Answer.

Neighborhoods (within 0.3):  
• 0.0 has {0.0, 0.2} → count 2 → not core.  
• 0.2 has {0.0, 0.2, 0.4} → count 3 → core.  
• 0.4 has {0.2, 0.4} → count 2 → not core.  
• 5.0 has {5.0, 5.1} → count 2 → not core.  
• 5.1 has {5.0, 5.1} → count 2 → not core.

Labels: 0.2 is core; 0.0 and 0.4 are within eps of a core, so border; 5.0 and 5.1 are neither core nor within eps of a core, so noise.

### Q5. Ward’s method: increase in SSE from a merge

In 1D, cluster C1 has n1=3 points with mean m1=2; cluster C2 has n2=2 points with mean m2=5. For Ward linkage, the increase in total SSE when merging is

ΔSSE = (n1 * n2) / (n1 + n2) * (m1 − m2)^2.

Compute ΔSSE and interpret.

Answer.

ΔSSE = (3*2)/(3+2) * (2−5)^2 = 6/5 * 9 = 10.8.  
Interpretation: among candidate merges, Ward chooses the pair with the smallest ΔSSE; here a Δ of 10.8 would be compared to alternatives to decide whether to merge these two clusters.

## VII. Quick Study Checklist

• Understand how linkage rules update proximity after each merge and how that shapes dendrograms.  
• Be able to compute one K‑means iteration and SSE quickly.  
• Remember silhouette definition and quick computation.  
• For DBSCAN, practice counting neighbors under eps and MinPts, including the point itself.  
• For Ward linkage, memorize ΔSSE formula and apply with small n and means.

