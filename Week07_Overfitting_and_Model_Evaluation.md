# Week 04 — Overfitting, Pruning, and Model Evaluation

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
| Prune (A → leaf) | 10/30 | 1 | 0.333 + 0.5×(1/30)=0.350 | Prune |
| Keep split (4 leaves) | 9/30 | 4 | 0.300 + 0.5×(4/30)=0.367 | - |

## 4. Confusion Matrix and Error Types

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

## 5. Evaluation Enhancements

- Cost Matrix assigns penalty weights to FP and FN.
- Validation Set tunes parameters like Ω before final testing.
- Cross-Validation estimates robustness.
- Stratified CV preserves class proportions for imbalanced datasets.

## 6. Calculation Practice Questions

Q1. TP=45, FP=5, FN=15, TN=35. Compute Accuracy, Precision, Recall, and F1.

Q2. Leaf node: 50 samples, 38 correct. Compute optimistic & pessimistic errors if Ω=0.5, k=2, N_train=200.

Q3. Parent entropy=0.94; child nodes 30% & 70% with entropies 0.8 & 0.5. Compute Information Gain.

Q4. 5-fold F1 scores=[0.81, 0.79, 0.83, 0.77, 0.80]. Compute mean ± std.

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

---

## 9. Answers and Explanations

### Q1. Gini Index Calculation

Given: 28 “Yes”, 12 “No”  
p(Yes) = 28/40 = 0.7  
p(No) = 12/40 = 0.3  

**Gini = 1 − (p(Yes)² + p(No)²)**  
= 1 − (0.7² + 0.3²) = **0.42**

→ Moderate impurity; node is not perfectly pure.

---

### Q2. Confusion Matrix Metrics

TP = 45, FP = 5, FN = 15, TN = 35  

**Accuracy = (TP + TN) / (TP + FP + FN + TN)**  
= (45 + 35) / 100 = **0.80**

**Precision = TP / (TP + FP)**  
= 45 / (45 + 5) = **0.90**

**Recall = TP / (TP + FN)**  
= 45 / (45 + 15) = **0.75**

**F1 = 2 × (Precision × Recall) / (Precision + Recall)**  
= 2 × (0.9 × 0.75) / (0.9 + 0.75) = **0.818**

→ The model achieves 80% accuracy, but recall is lower than precision, meaning it misses some positives.

---

### Q3. Optimistic vs Pessimistic Error

Leaf node: 50 samples, 38 correct → 12 incorrect.  
Ω = 0.5, k = 2, N_train = 200.

**Optimistic error:**  
err(T) = 12 / 50 = 0.24

**Pessimistic error:**  
E_gen(T) = err(T) + Ω × (k / N_train)  
= 0.24 + 0.5 × (2 / 200)  
= 0.24 + 0.005 = **0.245**

→ Slightly higher due to complexity penalty.

---

### Q4. Information Gain

Entropy(parent) = 0.94  
Children:  
- Node A (30%) → Entropy = 0.8  
- Node B (70%) → Entropy = 0.5  

**Weighted entropy = 0.3×0.8 + 0.7×0.5 = 0.24 + 0.35 = 0.59**

**Information Gain = 0.94 − 0.59 = 0.35**

→ Splitting reduces uncertainty by 0.35 bits.

---

### Q5. Cross-Validation Mean ± Std

F1 scores: [0.81, 0.79, 0.83, 0.77, 0.80]

**Mean = (Σ scores) / n = 0.80**  
**Standard deviation = √(Σ(x−mean)² / (n−1)) = 0.022**

→ **F1 = 0.80 ± 0.02**

→ Consistent model performance across folds.

---

---

## 9. Answers and Explanations

### Q1. Gini Index Calculation

Given: 28 “Yes”, 12 “No”  
p(Yes) = 28/40 = 0.7  
p(No) = 12/40 = 0.3  

**Gini = 1 − (p(Yes)² + p(No)²)**  
= 1 − (0.7² + 0.3²) = **0.42**

→ Moderate impurity; node is not perfectly pure.

---

### Q2. Confusion Matrix Metrics

TP = 45, FP = 5, FN = 15, TN = 35  

**Accuracy = (TP + TN) / (TP + FP + FN + TN)**  
= (45 + 35) / 100 = **0.80**

**Precision = TP / (TP + FP)**  
= 45 / (45 + 5) = **0.90**

**Recall = TP / (TP + FN)**  
= 45 / (45 + 15) = **0.75**

**F1 = 2 × (Precision × Recall) / (Precision + Recall)**  
= 2 × (0.9 × 0.75) / (0.9 + 0.75) = **0.818**

→ The model achieves 80% accuracy, but recall is lower than precision, meaning it misses some positives.

---

### Q3. Optimistic vs Pessimistic Error

Leaf node: 50 samples, 38 correct → 12 incorrect.  
Ω = 0.5, k = 2, N_train = 200.

**Optimistic error:**  
err(T) = 12 / 50 = 0.24

**Pessimistic error:**  
E_gen(T) = err(T) + Ω × (k / N_train)  
= 0.24 + 0.5 × (2 / 200)  
= 0.24 + 0.005 = **0.245**

→ Slightly higher due to complexity penalty.

---

### Q4. Information Gain

Entropy(parent) = 0.94  
Children:  
- Node A (30%) → Entropy = 0.8  
- Node B (70%) → Entropy = 0.5  

**Weighted entropy = 0.3×0.8 + 0.7×0.5 = 0.24 + 0.35 = 0.59**

**Information Gain = 0.94 − 0.59 = 0.35**

→ Splitting reduces uncertainty by 0.35 bits.

---

### Q5. Cross-Validation Mean ± Std

F1 scores: [0.81, 0.79, 0.83, 0.77, 0.80]

**Mean = (Σ scores) / n = 0.80**  
**Standard deviation = √(Σ(x−mean)² / (n−1)) = 0.022**

→ **F1 = 0.80 ± 0.02**

→ Consistent model performance across folds.

---

### T1. Overfitting reduction

✅ a) Using cross-validation  
✅ d) Pruning the model  
❌ b), c)

---

### T2. Pre-pruning stops when

✅ a) Class impurity improvement is negligible  
✅ c) Node sample size is below threshold  
❌ b), d)

---

### T3. Accuracy is misleading when

✅ b) Classes are highly imbalanced  
✅ c) Misclassification costs differ  
❌ a), d)

---

### T4. False Negatives correspond to

✅ a) Predict “No” when actual “Yes”  
✅ d) Type II error  
❌ b), c)

---

### T5. Stratified Cross-Validation ensures

✅ a) Same class proportion across folds  
✅ d) Equal sampling across classes  
❌ b), c)


