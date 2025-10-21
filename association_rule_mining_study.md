# Association Rule Mining Study Notes

This document summarizes the key study materials related to **Association Rule Mining**, including definitions, examples, and detailed exam-style practice questions. It is formatted for GitHub readability and aligned with La Trobe Data Mining exam expectations.

---

## I. Association Rule Mining Summary

### Define Association Rule Mining Tasks

Association rule mining aims to find rules that predict the occurrence of an item based on the presence of other items in a transaction.

Goal: Given a set of transactions (T), the objective is to find all rules that satisfy a minimum support (minsup) threshold and a minimum confidence (minconf) threshold.

Two main subtasks:
1. Frequent Itemset Generation: Find all itemsets whose support ≥ minsup.
2. Rule Generation: From each frequent itemset, generate high-confidence rules (X → Y).

Key takeaway: Association rule mining analyzes market-basket transactions to learn about customer purchasing behavior.

### Basic Concepts

| Concept | Definition | Example |
| :--- | :--- | :--- |
| Itemset | A collection of one or more items. | {Milk, Bread, Diaper} |
| Support Count (σ) | Number of transactions that contain an itemset. | σ({Milk, Bread, Diaper}) = 2 |
| Support (s) | Fraction of transactions containing an itemset. | s({Milk, Bread, Diaper}) = 2/5 |
| Frequent Itemset | Itemset with support ≥ minsup. | {Bread, Milk} if minsup = 0.5 |
| Confidence (c) | Likelihood that Y occurs given X. | c({Milk, Diaper} → {Beer}) = 0.67 |

**Formulas:**

- Support of X:  s(X) = (Support Count of X) / (Total Transactions)
- Support of rule X → Y: s(X ∪ Y)
- Confidence:  c(X → Y) = s(X ∪ Y) / s(X)

### Apriori Principle (Anti-Monotone Property)

If an itemset is frequent, all its subsets must also be frequent. Conversely, if an itemset is infrequent, all its supersets are infrequent.  
Support never increases as the itemset grows: s({Milk, Bread, Eggs}) ≤ s({Milk, Bread}) ≤ s({Milk}).

This enables pruning of large search spaces during candidate generation.

### Apriori Algorithm Summary

1. Generate F1 = frequent 1-itemsets.
2. Iteratively:
   - Generate candidate (k+1)-itemsets from Fk.
   - Prune candidates containing infrequent subsets.
   - Count supports in the database.
   - Eliminate candidates below minsup.

**Rule Generation:** Confidence pruning is applied similarly within rules generated from the same frequent itemset.

### Maximal vs Closed Itemsets

| Feature | Maximal Frequent Itemset | Closed Itemset |
| :--- | :--- | :--- |
| Definition | Frequent and has no frequent supersets. | No immediate superset has the same support. |
| Purpose | Compact representation of frequent itemsets. | Compact without losing support info. |
| Relationship | All maximal itemsets are closed. | Not all closed itemsets are maximal. |

### Interestingness Evaluation

Support and confidence can be misleading:
- Low support → rules occur by chance.
- High confidence → may still be misleading if P(Y|X) ≤ P(Y).

**Independence:** P(X, Y) = P(X) × P(Y)

**Interest (Lift):**
Interest = P(X, Y) / [P(X) × P(Y)]

Interpretation:
- Lift > 1 → positive correlation.
- Lift < 1 → negative correlation.

---

## II. Practice Questions

### A. Calculation Questions

Given the contingency table for 1000 people (T: Tea, C: Coffee):

| | Coffee (C) | Not Coffee (¬C) | Total |
| :--- | :--- | :--- | :--- |
| Tea (T) | 150 | 50 | 200 |
| Not Tea (¬T) | 650 | 150 | 800 |
| Total | 800 | 200 | 1000 |

**Q1. Support({Tea, Coffee}) = 150 / 1000 = 0.15**

**Q2. Confidence(¬Tea → Coffee) = 150 / 200 = 0.75**

**Q3. Interest(Tea → Coffee) = 0.15 / (0.2 × 0.8) = 0.9375 → negatively correlated.**

### B. Multiple Choice Questions

1. Which concept justifies pruning of supersets when an itemset is infrequent?  
   **Answer:** Apriori Principle (Anti-monotone Property of Support)

2. Why confidence is not anti-monotone?  
   **Answer:** Adding items to LHS changes denominator unpredictably.

3. Useful rule criterion: Confidence(X → Y) > Support(Y)

4. Closed itemset definition: No immediate superset has the same support.

5. Interest = 0.5 → negatively correlated.

6. Symmetric measures: M(A, B) = M(B, A)

---

## III. Advanced Calculation Exercises

### Q1. Conditional Probability and Lift Comparison
Dataset: 100 transactions.
- 30 contain {Milk}
- 40 contain {Bread}
- 20 contain {Milk, Bread}

**(a)** Compute Support({Milk, Bread}), Confidence(Milk → Bread), and Lift(Milk → Bread).

**Solution:**
Support = 20/100 = 0.2  
Confidence = 0.2 / 0.3 = 0.667  
Lift = 0.2 / (0.3 × 0.4) = 1.667 → positive correlation.

### Q2. Effect of Changing Minsup
If minsup = 0.25 and the support of {A,B} = 0.22, is {A,B} frequent? What happens to {A,B,C}?

**Solution:** {A,B} is infrequent → all supersets ({A,B,C}, {A,B,D}) are automatically pruned (Apriori principle).

### Q3. Support and Confidence with Conditional Items
In 600 transactions:  
- {Laptop} = 120  
- {Mouse} = 300  
- {Laptop, Mouse} = 90  
Compute confidence and lift for Laptop → Mouse.

**Solution:**
Support = 90/600 = 0.15  
Confidence = 0.15 / 0.2 = 0.75  
Lift = 0.15 / (0.2 × 0.5) = 1.5 (positive correlation)

### Q4. Combined Rule Evaluation
For a supermarket:
- P(Bread) = 0.4, P(Butter) = 0.3, P(Bread, Butter) = 0.25.

Find: Confidence(Butter → Bread), Lift, and whether the rule is interesting.

**Solution:**
Confidence = 0.25 / 0.3 = 0.8333  
Lift = 0.25 / (0.4 × 0.3) = 2.083 → strong positive relation.

### Q5. Multiple Rule Comparison
Given three rules:
- R1: {A} → {B}, support = 0.3, confidence = 0.8
- R2: {B} → {A}, support = 0.3, confidence = 0.4
- R3: {A, B} → {C}, support = 0.2, confidence = 0.67

Which has the highest lift if P(A)=0.5, P(B)=0.6, P(C)=0.3?

**Solution:**
Lift R1 = 0.3 / (0.5×0.6)=1.0  
Lift R2 = 0.3 / (0.6×0.5)=1.0  
Lift R3 = 0.2 / (0.3×0.3)=2.22 → R3 strongest.

---

## IV. Conceptual True/False & Multi-Correct Questions

Each question may have more than one correct answer.

**1. Which statements about Support are true?**  
A. Support is symmetric (s(A,B) = s(B,A)).  
B. Support can increase as the itemset grows.  
C. Support is anti-monotone.  
D. Support represents conditional probability P(Y|X).

**Answer:** A, C.

**2. Which statements about Confidence are true?**  
A. Confidence is asymmetric.  
B. Confidence(X→Y) = Support(X∪Y)/Support(X).  
C. Confidence can be greater than 1.  
D. Confidence is independent of Support.

**Answer:** A, B.

**3. Identify correct implications of the Apriori principle.**  
A. If {A,B} is infrequent, {A,B,C} can still be frequent.  
B. Frequent subsets guarantee that their supersets are frequent.  
C. If {A,B,C} is frequent, all subsets of it must be frequent.  
D. It enables pruning during frequent itemset generation.

**Answer:** C, D.

**4. Which of the following measures are symmetric?**  
A. Support  
B. Confidence  
C. Lift  
D. Interest

**Answer:** A, C, D.

**5. Which situations may lead to misleading rules?**  
A. High confidence but low lift (<1).  
B. High support and confidence.  
C. Confidence(X→Y) ≤ Support(Y).  
D. P(X,Y) = P(X)×P(Y).

**Answer:** A, C, D.

**6. Which statements about Closed and Maximal itemsets are true?**  
A. All closed itemsets are maximal.  
B. All maximal itemsets are closed.  
C. Closed itemsets retain exact support info.  
D. Maximal itemsets provide minimal redundancy but lose support detail.

**Answer:** B, C, D.

---

*End of Association Rule Mining Study File*

