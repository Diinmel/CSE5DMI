# Week 07 — Overfitting, Pruning, and Model Evaluation

## 1. Overfitting and Underfitting

Underfitting occurs when a model is too simple to capture the underlying data patterns. Both the training and test errors are large because the model has high bias and low variance.

Overfitting happens when the model is excessively complex. It learns both signal and noise, resulting in a low training error but a high test error—a symptom of low bias and high variance.

Typical causes of overfitting:
- Too few training examples
- Too many parameters or tree branches
- No regularization or pruning applied

Remedies:
- Collect more training data
- Simplify the model or use pruning
- Apply cross-validation to estimate generalization error

Mathematically, if E_train = error on training set and E_test = error on unseen test set, then generalization failure occurs when E_test ≫ E_train.

## 2. Tree Pruning Strategies

### Pre-Pruning (Early Stopping)
Stop before full depth when:
- Node sample size below threshold
- Class distribution shows little impurity reduction
- Information Gain or Gini improvement is insignificant

Risk: stopping too early may underfit the data.

### Post-Pruning (Reduced-Error or Cost-Complexity Pruning)
1. Grow full decision tree.
2. Replace subtrees with single leaves bottom-up.
3. Compare generalization error before and after pruning.

A subtree is pruned if expected error decreases after replacement.

## 3. Optimistic and Pessimistic Error Estimates

### Optimistic (Resubstitution) Estimate
Assumes test performance equals training performance:
E_gen = E_training = (# misclassified training instances) / (total training instances)

### Pessimistic (Cost-Complexity) Estimate
Adds penalty for model complexity:
E_gen(T) = err(T) + Ω × (k / N_train)
where err(T) = training error rate, k = number of leaf nodes, N_train = total training samples, Ω = regularization constant.

Example:
Node A has 30 samples (20 Yes, 10 No), Ω = 0.5.

| Situation | Training Error err(T) | Leaf Count k | Pessimistic E_gen(T) | Decision |
|------------|----------------------|---------------|----------------------|-----------|
| Prune (A → leaf) | 10/30 | 1 | 0.333 + 0.5×(1/30)=0.350 | — |
| Keep split (4 leaves) | 9/30 | 4 | 0.300 + 0.5×(4/30)=0.367 | Prune |

## 4. Impurity Measures

- Gini = 1 − Σ(p_i²)
- Entropy = −Σ(p_i log₂ p_i)
- Information Gain = Entropy(parent) − Σ(weighted Entropy(children))

## 5. Confusion Matrix and Error Types

| | Predicted Positive (P) | Predicted Negative (N) |
| :--- | :--- | :--- |
| Actual Positive (P) | True Positive (TP) | False Negative (FN) |
| Actual Negative (N) | False Positive (FP) | True Negative (TN) |

Type I Error: FP — predicting positive when actually negative.
Type II Error: FN — predicting negative when actually positive.

Derived metrics:
- Accuracy = (TP + TN) / (TP + FP + FN + TN)
- Precision = TP / (TP + FP)
- Recall = TP / (TP + FN)
- F1 = 2 × (Precision × Recall) / (Precision + Recall)

## 6. Evaluation Enhancements

- Cost Matrix assigns penalty weights to FP and FN.
- Validation Set tunes parameters like Ω before final testing.
- Cross-Validation estimates robustness.
- Stratified CV preserves class proportions for imbalanced datasets.

## 7. Calculation Practice Questions

Q1. Node: 28 Yes, 12 No. Compute Gini Index.
→ p_yes=0.7, p_no=0.3 → Gini=1−(0.7²+0.3²)=0.42.

Q2. TP=45, FP=5, FN=15, TN=35. Compute Accuracy, Precision, Recall, and F1.

Q3. Leaf node: 50 samples, 38 correct. Compute optimistic & pessimistic errors if Ω=0.5, k=2, N_train=200.

Q4. Parent entropy=0.94; child nodes 30% & 70% with entropies 0.8 & 0.5. Compute Information Gain.

Q5. 5-fold F1 scores=[0.81, 0.79, 0.83, 0.77, 0.80]. Compute mean ± std.

## 8. Multi-Select Theory Questions

T1. Overfitting can be reduced by
a) Cross-validation
b) Increasing tree depth
c) Adding random noise
d) Pruning

T2. Pre-pruning stops when
a) Little impurity improvement
b) Training accuracy = 100%
c) Small node size
d) Fixed tree height regardless of data

T3. Accuracy is misleading when
a) Data balanced
b) Classes imbalanced
c) Misclassification costs differ
d) Precision = Recall

T4. False Negatives correspond to
a) Predict “No” when actual “Yes”
b) Predict “Yes” when actual “No”
c) Type I error
d) Type II error

T5. Stratified CV ensures
a) Same class proportion across folds
b) Random splits ignoring class
c) More data in training folds
d) Equal sampling across classes
