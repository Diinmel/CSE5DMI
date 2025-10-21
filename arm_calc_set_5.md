# Association Rule Mining – Calculation Set (5 problems)
English only. Answers are in the final section.

## Problem 1 – Threshold screening with support, confidence, and lift
Dataset size N = 1000. Item support counts (number of transactions) are given below.

| Itemset | Count |
| :-- | --: |
| A | 400 |
| B | 300 |
| C | 250 |
| D | 200 |
| AB | 180 |
| AC | 150 |
| AD | 120 |
| BC | 130 |
| BD | 90 |
| CD | 70 |
| ABC | 80 |
| ABD | 60 |
| ACD | 50 |
| BCD | 40 |
| ABCD | 30 |

Minsup = 0.06. Minconf = 0.40.

Tasks.
1) For each rule below, compute support s(X∪Y), confidence c(X→Y), and lift L(X→Y).
   a) A → B
   b) AB → C
   c) AC → D
   d) B → D
2) Which of the above rules meet both thresholds (support ≥ minsup and confidence ≥ minconf)?

---

## Problem 2 – Number of rules from a frequent itemset and pruning seed
Suppose L = {A, B, C, D} is frequent.
1) How many distinct non-empty proper rules can be generated from L?
2) Using the anti‑monotone behavior of confidence with respect to RHS size (within the same L),
   what is the minimal set of rules you must evaluate first in order to be able to prune all
   larger‑RHS rules that cannot meet minconf? Give the rule forms, not their values.

---

## Problem 3 – Apriori candidate generation and pruning
Assume the set of frequent 2‑itemsets is
F2 = { AB, AC, AD, BC }.
The 2‑itemsets BD and CD are infrequent.

1) List all distinct 3‑item candidates C3 that can be formed by self‑joining F2.
2) Apply the subset pruning step: remove any 3‑item candidate that has at least one 2‑subset not in F2.
3) Which 3‑item candidates survive?

---

## Problem 4 – Comparing confidences for rules from the same itemset
Let the following supports be known (fractions of N):
s(A) = 0.40, s(B) = 0.35, s(C) = 0.30,
s(AB) = 0.22, s(ABC) = 0.14, s(ABCD) = 0.09.

1) Compute c(ABC → D) and c(AB → CD), using s(ABCD) = 0.09.
2) Which confidence is larger, and what property does this illustrate about confidence
   with respect to increasing RHS size (for rules generated from the same frequent itemset)?

---

## Problem 5 – Rule screening with minsup/minconf from a 3‑itemset
Suppose {A, B, C} is frequent with supports (fractions of N):
s(A)=0.52, s(B)=0.48, s(C)=0.30, s(AB)=0.33, s(AC)=0.24, s(BC)=0.22, s(ABC)=0.18.

Thresholds: minsup = 0.12, minconf = 0.60.

1) List all non‑empty proper rules that can be generated from {A, B, C}.
2) For each rule, compute support and confidence.
3) Which rules satisfy both thresholds?

---

# Answers

## Problem 1
N = 1000 so s(X) = count(X)/1000.

a) A → B
- s(AB) = 180/1000 = 0.18
- c = s(AB)/s(A) = 0.18 / 0.40 = 0.45
- L = s(AB) / (s(A)·s(B)) = 0.18 / (0.40·0.30) = 1.5
- Meets minsup (0.18 ≥ 0.06) and minconf (0.45 ≥ 0.40).

b) AB → C
- s(ABC) = 80/1000 = 0.08
- c = s(ABC)/s(AB) = 0.08 / 0.18 ≈ 0.444
- L = s(ABC) / (s(AB)·s(C)) = 0.08 / (0.18·0.25) ≈ 1.778
- Meets minsup and minconf.

c) AC → D
- s(ACD) = 50/1000 = 0.05
- c = s(ACD)/s(AC) = 0.05 / 0.15 ≈ 0.333
- L = s(ACD) / (s(AC)·s(D)) = 0.05 / (0.15·0.20) ≈ 1.667
- Fails minsup (0.05 < 0.06) and also fails minconf (≈0.333 < 0.40).

d) B → D
- s(BD) = 90/1000 = 0.09
- c = s(BD)/s(B) = 0.09 / 0.30 = 0.30
- L = s(BD) / (s(B)·s(D)) = 0.09 / (0.30·0.20) = 1.5
- Meets minsup but fails minconf.

Rules that pass both thresholds: A→B and AB→C.

## Problem 2
1) Total rules from a k‑itemset is 2^k − 2. With k=4, total = 14 rules.
2) Evaluate first all rules with RHS of size 1: A, B, C, D each on the RHS.
   Concretely: ABC→D, ABD→C, ACD→B, BCD→A.
   If any of these fail minconf, then all rules whose RHS contains that same consequent
   (for example, AB→CD extends ABC→D) can be pruned because confidence is anti‑monotone with respect to RHS size.

## Problem 3
1) Self‑join candidates (set‑wise):
   - From AB with AC → ABC
   - From AB with AD → ABD
   - From AC with AD → ACD
   - From AB with BC → ABC
   - From AC with BC → ABC
   Distinct C3 = { ABC, ABD, ACD }.
2) Subset pruning:
   - ABC has subsets AB, AC, BC → all in F2 → keep.
   - ABD has subsets AB, AD, BD → BD is infrequent → prune ABD.
   - ACD has subsets AC, AD, CD → CD is infrequent → prune ACD.
3) Survives: only ABC.

## Problem 4
c(ABC→D) = s(ABCD)/s(ABC) = 0.09 / 0.14 ≈ 0.6429  
c(AB→CD) = s(ABCD)/s(AB) = 0.09 / 0.22 ≈ 0.4091  
Thus c(ABC→D) ≥ c(AB→CD). This illustrates that, within the same generating itemset, confidence is anti‑monotone with respect to the size of the RHS (larger RHS → confidence does not increase).

## Problem 5
All non‑empty proper rules from {A,B,C} (2^3 − 2 = 6 rules):
- A→BC, B→AC, C→AB, AB→C, AC→B, BC→A.

Support s(X∪Y) and confidence c(X→Y):
- A→BC: s(ABC)=0.18, c=0.18/0.52 ≈ 0.346 → fails
- B→AC: s(ABC)=0.18, c=0.18/0.48 = 0.375 → fails
- C→AB: s(ABC)=0.18, c=0.18/0.30 = 0.600 → meets minconf, and s=0.18 ≥ 0.12 → passes
- AB→C: s(ABC)=0.18, c=0.18/0.33 ≈ 0.545 → fails
- AC→B: s(ABC)=0.18, c=0.18/0.24 = 0.750 → passes (and s=0.18 ≥ 0.12)
- BC→A: s(ABC)=0.18, c=0.18/0.22 ≈ 0.818 → passes (and s=0.18 ≥ 0.12)

Rules satisfying both thresholds: C→AB, AC→B, BC→A.
