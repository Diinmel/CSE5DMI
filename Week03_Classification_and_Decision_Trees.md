# Week 03 – Classification Task and Decision Trees

This document summarises the Week 03 lecture content, highlights key examinable concepts, and provides a focused mock test designed to strengthen your understanding of Decision Tree algorithms, impurity measures, and split evaluation—core topics reinforced again in Week 12.

---

## 1. Lecture Summary

### Classification Task

- **Definition:** Classification is the process of learning a model to predict a *categorical* target variable (class label **y**) from a set of predictor attributes **X**.  
- **Goal:** Build a model that accurately assigns class labels to unseen records and assess performance on a test set.  
- **Example:** Spam vs non-spam emails, benign vs malignant tumors.

### Decision Trees

A Decision Tree represents a series of tests leading to class predictions.

- **Structure:** Root node, internal nodes (attribute tests), branches (test outcomes), and leaf nodes (class labels).
- **Induction Process:** Learn the tree from data by recursively partitioning records.

#### Hunt’s Algorithm
If all records at node *t* belong to the same class → make it a **leaf** labeled with that class.  
If not → choose an attribute test to split data and apply recursively to child nodes.

### Design Issues

1. **Splitting Criteria**
   - How to choose the attribute and test condition at each node.
   - Based on the attribute type (binary, nominal, ordinal, continuous).
   - Ordinal attributes must *preserve order* when grouped.

2. **Stopping Criteria**
   - When to stop splitting (e.g., all records pure, minimum samples reached, or no further gain).

### Impurity and Split Evaluation

Decision Trees prefer splits that produce **purer** child nodes.

| Measure | Formula | Notes |
| :-- | :-- | :-- |
| **Gini Index** | `Gini(t) = 1 - Σ p_i(t)²` | 0 = pure; higher = more impure |
| **Entropy** | `Entropy(t) = - Σ p_i(t) log₂ p_i(t)` | 0 = pure; 1 = max impurity (for 2-class) |
| **Weighted Gini** | `Σ (n_i / n) * Gini(i)` | Used after a split |
| **Information Gain** | `Gain = Entropy(parent) - WeightedEntropy(children)` | Higher = better split |

**Goal:**  
- *Maximise* Information Gain  
- *Minimise* Weighted Gini (or Weighted Entropy M)

---

## 2. Key Concepts Likely to Appear in the Exam

| Concept | Definition / Importance |
| :-- | :-- |
| **Classification Model (f)** | Predicts categorical label y from X. |
| **Homogeneity (Purity)** | Nodes containing mostly one class are “pure.” |
| **Gini Index** | Measures impurity; minimum = 0. |
| **Entropy** | Measures impurity using information theory. |
| **Information Gain** | Reduction in entropy after a split. |
| **Attribute Test Condition (Ordinal)** | Must preserve the order of attribute values. |

---

## 3. Mock Test for Final Exam Practice

### Section A – Conceptual Questions

#### Question A1 – Decision Tree Design (10 marks)

1. Describe the two main categories of design issues in Decision Tree induction.  
2. Explain the **Attribute Test Condition** differences between *Nominal* and *Ordinal* attributes.  
   - What restriction applies uniquely to Ordinal attributes?

#### Question A2 – Impurity Measures and Best Split (10 marks)

1. Define **Node Impurity** and explain why we use Gini Index or Entropy.  
2. Describe the **greedy procedure** used to select the best split.  
   - Define **Gain** and clarify whether we maximise or minimise it.

---

### Section B – Calculation Practice

**Given:**  
Parent node P → 10 records C0, 10 records C1.  
`P_C0 = 0.5`, `P_C1 = 0.5`.

#### Split A – Binary Split

| Child Node | C0 Count | C1 Count | Total |
| :-- | --: | --: | --: |
| N1 (A = Yes) | 9 | 1 | 10 |
| N2 (A = No) | 1 | 9 | 10 |

#### Split B – Multi-way Split

| Child Node | C0 Count | C1 Count | Total |
| :-- | --: | --: | --: |
| N3 (B = X) | 4 | 1 | 5 |
| N4 (B = Y) | 4 | 4 | 8 |
| N5 (B = Z) | 2 | 5 | 7 |

---

### Question B1 – Gini Index Calculation (15 marks)

1. Compute `Gini(N1)` and `Gini(N2)`.  
2. Compute **Weighted Gini** for Split A:  
   `GINI_splitA = (n1/n)*Gini(N1) + (n2/n)*Gini(N2)`.

### Question B2 – Gini Index Comparison (15 marks)

Given:  
`Gini(N3)=0.32`, `Gini(N4)=0.50`, `Gini(N5)=0.49`

1. Compute `GINI_splitB = Σ (n_i/n)*Gini(i)`.  
2. Identify which attribute (A or B) yields the **best split**, using the rule that the best split minimises Weighted Gini.

---

### Question B3 – Entropy and Information Gain (15 marks)

Given:  
`Entropy(P)=1.0`  
`Entropy(N1)=0.469`, `Entropy(N2)=0.469`

1. Compute Weighted Entropy for Split A:  
   `M_entropyA = (n1/n)*Entropy(N1) + (n2/n)*Entropy(N2)`.  
2. Compute `Gain_splitA = Entropy(P) - M_entropyA`.

---

### Question B4 – Continuous Attribute Strategy (10 marks)

List the three computational steps to find the best binary split for a continuous attribute:

1. **Sort** all training records by the attribute’s numeric value.  
2. **Scan linearly** to evaluate impurity (Gini or Entropy) at every candidate boundary between distinct class labels.  
3. **Select** the split point giving the minimum weighted impurity (or maximum information gain).

---

## 4. Study Tip

When revising, practice by hand-calculating Gini Index and Entropy for small 2-class datasets. It reinforces intuition about “purity.” In exams, remember: *Decision Trees are greedy optimisers of purity, not global optimisers of truth.*

---

*Prepared for BUS5PR1 – Data Mining, La Trobe University (Master of Business Analytics).*
