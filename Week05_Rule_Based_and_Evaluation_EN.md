# Rule-Based Classification and Model Evaluation — Formulas with Intuition

This note summarizes rule-based classifiers and the core evaluation logic. Every formula is paired with a short intuition so you can recall the why, not just the math. The end includes 10 multi-select MCQs and 6 worked-style calculation prompts.

## I. What a Rule-Based Classifier is and how rules are derived

### A. Core concept and properties

A classifier is a set of if–then rules. Each rule has
- Left-hand side (LHS, antecedent): a conjunction of conditions on features. All conditions must hold.
- Right-hand side (RHS, consequent): the predicted label y.

Example: (BloodTemp = Warm) and (LaysEggs = Yes) -> Birds

Desired qualities
- Mutually exclusive: at prediction time, at most one rule should fire for a case.
- Exhaustive: every case should be covered by at least one rule, so a prediction is always returned.

Design resolutions
- Conflicts, when multiple rules fire with different labels: use an ordered rule list (decision list). Evaluate rules top to bottom; the first matching rule determines the label.
- Uncovered cases: use a default class applied when no rule fires.

### B. Deriving rules from decision trees (indirect method)

Each root-to-leaf path of a decision tree corresponds to one rule. The set of rules extracted from a full tree is mutually exclusive and exhaustive by construction. After extraction, simplify rules by removing conditions that do not change validation performance for that rule. This is local pruning that improves readability and generalization.

## II. Model and rule evaluation measures

Class imbalance makes naive accuracy misleading. These measures focus on what the classifier does on positives and negatives separately, and what each rule contributes.

### A. Precision, recall, accuracy

Let TP, FP, FN, TN be counts on a confusion matrix.

- Recall, also Sensitivity or True Positive Rate (TPR)
  TPR = TP / (TP + FN)
  Intuition: out of all actual positives, how many we recovered.

- Precision, also Positive Predicted Value
  Precision = TP / (TP + FP)
  Intuition: out of all predicted positives, how many were correct.

- Accuracy
  Accuracy = (TP + TN) / N
  Warning: with imbalance, a trivial classifier can get high accuracy by always predicting the majority class.

- False Discovery Rate
  FDR = 1 − Precision = FP / (TP + FP)
  Intuition: fraction of predicted positives that were actually false alarms.

### B. Rule-level metrics

Let LHS be the rule condition and RHS the predicted class.

- Coverage
  Coverage = records satisfying LHS / all records
  Intuition: how often the rule applies.

- Rule accuracy
  Rule accuracy = records satisfying LHS and RHS / records satisfying LHS
  Intuition: trustworthiness when the rule fires.

- Support of the rule
  Support = records satisfying LHS and RHS / all records
  Intuition: how many correct cases this rule contributes overall.

### C. ROC curve and AUC

- Axes
  y-axis is TPR. x-axis is FPR, where FPR = FP / (FP + TN) = 1 − TNR.

- Construction logic
  1) Sort examples by a continuous score such as a probability. 2) Sweep a threshold over unique scores. 3) At each threshold, predict positive if score ≥ threshold. 4) Recompute TP, FP, FN, TN. 5) Plot the pair (FPR, TPR).

- Key points
  (0, 0) predicts everything negative. (1, 1) predicts everything positive. The diagonal is random guessing. Ideal behavior approaches the top-left corner.

- AUC
  Area under the ROC curve. AUC = 1 is perfect ranking. AUC = 0.5 is random ranking. Higher is better.

### D. Naive Bayes
Not covered by the provided sources, so omitted here.

## III. Practice section

### A. Ten multi-select MCQs
Choose all options that are correct.

1) When classes are imbalanced, which metrics are typically preferred to judge performance?
A) Recall   B) Precision   C) F1   D) Coverage

2) A well-designed decision list requires
A) an explicit rule order
B) rules that are mutually exclusive by position
C) a default class
D) all rules firing simultaneously

3) Converting a full decision tree to rules yields, before simplification,
A) mutual exclusivity   B) exhaustiveness   C) the need for a default class   D) unordered conflicts

4) A rule has high coverage but low accuracy. What follows?
A) it applies often but is unreliable when it fires
B) it applies rarely but is very precise
C) it should be specialized further or moved lower in the order
D) it is ideal to use as the default class

5) For a fixed scoring model, increasing the decision threshold generally
A) increases precision   B) increases recall   C) decreases FPR   D) increases TPR

6) FOIL-style separate-and-conquer strategies prefer to add a condition when
A) precision improves substantially and many positives remain covered
B) precision improves but almost no positives remain covered
C) precision is unchanged while coverage increases
D) precision drops while coverage increases

7) Which statements about ROC and AUC are correct?
A) ROC uses TPR versus FPR
B) AUC equals the probability a random positive is ranked above a random negative
C) AUC of 0.5 implies random ranking
D) AUC equals accuracy

8) In an ordered rule list, conflicts between rules are resolved by
A) voting across all matching rules
B) taking the first rule that matches
C) weighting by support
D) choosing the rule with the highest confidence irrespective of order

9) Rule accuracy equals
A) TP / (TP + FP) on the subset covered by the rule
B) records satisfying LHS and RHS divided by records satisfying LHS
C) support divided by coverage
D) 1 − FDR on the subset covered by the rule

10) A default class is used to
A) resolve ties when multiple rules fire
B) label cases when no rule fires
C) guarantee that the classifier is exhaustive
D) maximize precision at the cost of recall

### B. Six calculation-style prompts

Assume in Prompts 1 to 3 we have a test set of 100 examples with the confusion matrix:

            Predicted Yes   Predicted No   Total
Actual Yes       TP = 40         FN = 10     50
Actual No        FP = 10         TN = 40     50
Totals               50              50     100

Prompt 1. Recall
Compute Recall = TPR = TP / (TP + FN). Show the numeric value and a one-line interpretation.

Prompt 2. Precision
Compute Precision = TP / (TP + FP). Show the numeric value and a one-line interpretation.

Prompt 3. False positive rate
Compute FPR = FP / (FP + TN). Show the numeric value and a one-line interpretation.

Prompt 4. Coverage of a rule
Dataset size is 100. Rule R: (Status = Single) -> No. Forty records satisfy the LHS condition.
Compute Coverage and interpret in one sentence.

Prompt 5. Rule accuracy
Continuing Prompt 4, of the 40 cases satisfying LHS, 25 have actual label No. Compute rule accuracy and give a short interpretation.

Prompt 6. FOIL information gain
We consider adding one conjunct to rule R0 to create R1.
R0 covers p0 = 10 positives and n0 = 5 negatives.
R1 covers p1 = 8 positives and n1 = 2 negatives.
Use FOILGain(R0 → R1) = p1 × [ log2(p1/(p1+n1)) − log2(p0/(p0+n0)) ].
Set up the calculation explicitly and simplify to a single numerical value. Conclude whether the added condition is worthwhile.

