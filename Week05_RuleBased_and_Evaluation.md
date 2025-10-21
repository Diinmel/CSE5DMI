
# Rule-based Classification and Model Evaluation — Formulas with Intuition

This note distills the core ideas behind rule-based classifiers, shows how to calculate the key metrics, and gives practice questions that match La Trobe Master-level exams. Formulas are paired with short “why it makes sense” explanations so you remember the logic, not just the algebra.

## 1. Rule-based classifiers in one page

A rule has the form: if [conditions on features] then predict class c.

Typical design choices
- Ordering. Use an ordered rule list (decision list). Evaluate rules from top to bottom; first matching rule fires. Needed to resolve conflicts.
- Default class. Used when no rule fires. Usually the majority class on the training set not covered by any rule.
- Exclusive vs exhaustive. A decision list is exclusive by construction. Add a default class to make it exhaustive.
- Learning strategies.
  - Indirect: train a decision tree, then convert each root→leaf path to a rule. Optionally simplify each rule.
  - Separate-and-conquer (covering): grow one high-precision rule at a time, remove covered examples, repeat. FOIL and RIPPER follow this template.
- Rule quality. Prefer rules with high precision and useful coverage, penalizing complexity.

## 2. Metrics for individual rules

Let LHS be the rule condition and RHS be the predicted class.

- Coverage = records satisfying LHS / all records
  - Intuition: how often the rule is applicable.
- Rule accuracy = records satisfying LHS and RHS / records satisfying LHS
  - Intuition: trustworthiness when the rule fires.
- Support of the rule = records satisfying LHS and RHS / all records
  - Intuition: how many correct cases the rule contributes overall.
- Confidence = same as rule accuracy for a class rule.

## 3. Confusion-matrix metrics for a classifier

Let TP, FP, FN, TN come from a chosen decision threshold.

- Precision = TP / (TP + FP)
  - Think: of all predicted positives, what fraction were right.
- Recall (TPR, sensitivity) = TP / (TP + FN)
  - Think: of all actual positives, what fraction we found.
- Specificity (TNR) = TN / (TN + FP)
- FPR = FP / (FP + TN) = 1 - Specificity
- Accuracy = (TP + TN) / N
  - Warning: misleading when classes are imbalanced.
- F1 = 2 × (Precision × Recall) / (Precision + Recall)
  - Intuition: harmonic mean rewards balance; one low value drags F1 down.

## 4. ROC and AUC in two steps

- ROC plots TPR on the y-axis against FPR on the x-axis while sweeping the decision threshold from strict to lenient.
- AUC is the probability that a randomly chosen positive is ranked above a randomly chosen negative. AUC 0.5 means random guessing. AUC 1.0 is perfect ranking.

## 5. FOIL information gain for rule growth

When adding a new condition to a candidate rule R0 to obtain R1, FOILGain measures purity improvement on positives that R1 still covers:

FOILGain(R0 → R1) = p1 × [ log2( p1 / (p1 + n1) ) − log2( p0 / (p0 + n0) ) ]

where p0, n0 are counts of positive and negative examples covered by R0, and p1, n1 by R1. The multiplier p1 rewards rules that improve precision without shrinking to near-zero coverage.

## 6. Worked calculation examples

Example 1. Coverage and rule accuracy
- Data size 300. Rule L: (Age < 25 and App = Mobile) → Churn = Yes.
- 90 records satisfy the LHS. Among them 63 are actually Yes.
- Coverage = 90 / 300 = 0.30
- Rule accuracy = 63 / 90 = 0.70
- Support of rule = 63 / 300 = 0.21
Key idea: Coverage asks “how often can I even use this rule,” accuracy asks “when I use it, how often am I right.”

Example 2. Confusion-matrix metrics
- TP = 48, FP = 12, FN = 22, TN = 118. N = 200.
- Precision = 48 / (48 + 12) = 0.80
- Recall = 48 / (48 + 22) = 0.686
- F1 = 2 × (0.80 × 0.686) / (0.80 + 0.686) = 0.738
- Accuracy = (48 + 118) / 200 = 0.83
Key idea: High accuracy hides the fact that recall is under 0.70; look beyond accuracy when classes are skewed.

Example 3. FOIL gain step
- Before adding a condition: p0 = 20, n0 = 10.
- After adding the condition: p1 = 16, n1 = 4.
- Precision0 = 20 / 30 = 0.667; Precision1 = 16 / 20 = 0.80.
- FOILGain = 16 × [ log2(0.80) − log2(0.667) ]
  = 16 × [ (−0.322) − (−0.585) ] ≈ 16 × 0.263 ≈ 4.21
Decision: positive gain, so keep the new condition.
Key idea: Improve precision and retain many positives (p1 large) to get a strong gain.

Example 4. Rule list with default class
- Ordered rules:
  1) if Income > 120k and City = CBD then class = Elite
  2) if Tenure < 3 and App = Mobile then class = Risky
  default class = Stable
- A record matches rule 2 and also matches a later rule 3 (if it existed). The decision list uses the first match only.
Key idea: ordering resolves conflicts. Add a default to guarantee exhaustiveness.

Example 5. From tree path to rule
- Path: root Age < 30 → Device = iOS → Leaf class = Buyer.
- Rule: if Age < 30 and Device = iOS then Buyer.
- Simplify: drop any condition that does not change validation accuracy for that rule.
Key idea: Rule simplification is local pruning; it keeps rules readable without hurting performance.

Example 6. Building an ROC point
- Scores for five cases with true labels [1,1,0,0,1] and scores [0.95, 0.70, 0.65, 0.40, 0.20].
- Choose threshold 0.60. Predict positive if score ≥ 0.60.
- Predictions: [1,1,1,0,0] vs truth [1,1,0,0,1] → TP=2, FP=1, FN=1, TN=1.
- TPR = 2 / (2 + 1) = 0.667; FPR = 1 / (1 + 1) = 0.5.
Key idea: Lower the threshold → more TP but also more FP, so the ROC point moves up and right.

## 7. Ten multi-select MCQs (choose all that apply)

Q1. When classes are imbalanced, which metrics are usually preferred over accuracy?
A) Recall  B) Precision  C) F1  D) Coverage

Q2. A well-constructed decision list needs
A) an explicit order of rules  B) mutually exclusive rules by position
C) a default class  D) rules that all fire simultaneously

Q3. Converting a full decision tree to rules yields, before simplification,
A) mutual exclusivity  B) exhaustiveness  C) the need for a default class  D) unordered conflicts

Q4. A rule with high coverage but low accuracy most likely means
A) it applies often but is unreliable when it fires
B) it applies rarely but is very precise
C) it should be moved to the bottom of an ordered list or specialized further
D) it is ideal for the default class

Q5. For a fixed classifier, increasing the decision threshold typically
A) increases precision  B) increases recall  C) decreases FPR  D) increases TPR

Q6. FOIL gain will be high when
A) precision improves a lot and p1 is still large
B) precision improves but p1 is tiny
C) precision does not change but coverage grows
D) precision drops while coverage increases

Q7. Which statements about ROC/AUC are correct?
A) ROC uses TPR vs FPR
B) AUC equals the probability a random positive is ranked above a random negative
C) AUC 0.5 implies random ranking
D) AUC is identical to accuracy

Q8. In an ordered rule list, conflicts between rules are resolved by
A) voting across all matching rules
B) taking the first rule that matches
C) weighting rules by support
D) using the rule with the highest confidence regardless of position

Q9. Rule accuracy equals
A) TP / (TP + FP) on the subset covered by the rule
B) records satisfying LHS and RHS divided by records satisfying LHS
C) support divided by coverage
D) 1 − FDR on the subset covered by the rule

Q10. A default class is primarily used to
A) resolve ties between multiple matching rules
B) provide a label when no rule fires
C) guarantee the classifier is exhaustive
D) maximize precision at the cost of recall

## 8. Answer key for MCQs

Q1: A,B,C
Q2: A,B,C
Q3: A,B
Q4: A,C
Q5: A,C
Q6: A
Q7: A,B,C
Q8: B
Q9: B,D
Q10: B,C
