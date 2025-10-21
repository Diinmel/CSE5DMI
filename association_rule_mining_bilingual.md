# Association Rule Mining – Song Ngữ Study Guide  
## Khai thác Luật Kết Hợp (Association Rule Mining)

This bilingual guide summarizes essential definitions, formulas, examples, and exam-style questions from Week 7–8–12 materials.  
Tài liệu song ngữ này tổng hợp các khái niệm, công thức, ví dụ và câu hỏi ôn tập liên quan đến **khai thác luật kết hợp** trong môn Data Mining.

---

## I. Summary / Tóm tắt

**Definition – Định nghĩa**  
Association Rule Mining aims to discover rules that predict the occurrence of one item based on others in the same transaction.  
Mục tiêu của khai thác luật kết hợp là **tìm các quy tắc dự đoán sự xuất hiện của một mặt hàng dựa trên các mặt hàng khác** trong cùng giao dịch.

**Goal – Mục tiêu:**  
Find all rules that satisfy minimum **support (minsup)** and **confidence (minconf)** thresholds.  
Tìm tất cả quy tắc có **độ hỗ trợ (support)** và **độ tin cậy (confidence)** vượt ngưỡng yêu cầu.

Two subtasks / Hai bước chính:
1. **Frequent Itemset Generation – Sinh tập mục thường xuyên**  
   → Tìm các tập mục có support ≥ minsup.  
2. **Rule Generation – Sinh luật kết hợp**  
   → Tạo các luật có confidence ≥ minconf từ tập mục thường xuyên.

---

## II. Key Concepts / Khái niệm cơ bản

| Concept / Khái niệm | Definition / Định nghĩa | Example / Ví dụ |
| :--- | :--- | :--- |
| **Itemset** | A group of one or more items. / Tập hợp gồm ≥ 1 mục. | {Milk, Bread, Diaper} |
| **Support Count (σ)** | Number of transactions containing an itemset. / Số giao dịch chứa tập mục. | σ({Milk, Bread}) = 2 |
| **Support (s)** | σ(X)/N = Fraction of total transactions. | 2 / 5 = 0.4 |
| **Frequent Itemset** | Itemset with support ≥ minsup. | {Bread, Milk} if minsup = 0.4 |
| **Confidence (c)** | Probability P(Y | X) – how often Y appears when X does. | c({Milk}→{Bread}) = 0.67 |

---

### Formulas / Công thức

- Support of X:  s(X) = σ(X)/N  
- Support of rule X → Y:  s(X ∪ Y)  
- Confidence:  c(X → Y) = s(X ∪ Y)/s(X)

---

## III. Apriori Principle / Nguyên lý Apriori

**Anti-Monotone Property – Tính đơn điệu ngược:**  
If an itemset is frequent, **all its subsets are frequent**;  
if infrequent, **all its supersets are infrequent**.  
→ Khi tập mục hiếm, các siêu tập của nó chắc chắn cũng hiếm.  

Support never increases with size:  
s({Milk, Bread, Eggs}) ≤ s({Milk, Bread}) ≤ s({Milk})

**Apriori Algorithm – Thuật toán Apriori**

1. Find F₁ = frequent 1-itemsets.  
2. Generate (k+1)-itemset candidates from Fₖ.  
3. Prune candidates if any subset is infrequent.  
4. Scan database to count support and keep frequent ones.  

→ Giúp giảm đáng kể số lượng ứng viên cần xét.

---

## IV. Compact Representation / Biểu diễn gọn hơn

| Feature | Maximal Frequent Itemset (Tối đa) | Closed Itemset (Đóng) |
| :--- | :--- | :--- |
| **Definition / Định nghĩa** | Frequent & no frequent superset. | No immediate superset has same support. |
| **Purpose / Mục đích** | Nén tập luật, bỏ bớt trùng lặp. | Giữ nguyên thông tin support. |
| **Relationship / Quan hệ** | All maximal → closed. | Not all closed → maximal. |

---

## V. Confidence & Rule Pruning / Độ tin cậy và cắt giảm luật

Confidence is *not anti-monotone* in general:  
→ Adding items to LHS changes denominator unpredictably.  
Tăng số mục ở vế trái làm thay đổi mẫu số nên có thể tăng hoặc giảm độ tin cậy.

But within the same itemset, confidence is anti-monotone with respect to RHS size:  
c(ABC→D) ≥ c(AB→CD) ≥ c(A→BCD)  
→ Trong cùng một tập mục, nhiều mục ở vế phải → confidence giảm.

**Pruning Logic – Cắt giảm:**  
If a rule has low confidence, skip all its extensions (bigger RHS).  
Nếu một luật có độ tin cậy thấp, mọi luật mở rộng của nó đều bị loại.

---

## VI. Interestingness Measures / Độ thú vị và vấn đề của Confidence

Confidence may be misleading if Y is very common.  
→ Độ tin cậy cao không luôn ý nghĩa nếu Y xuất hiện nhiều sẵn.

Example / Ví dụ:  
Tea → Coffee c = 0.75, but P(Coffee)=0.8  
⇒ P(Coffee | Tea) < P(Coffee): luật sai lệch.

**Useful Rule Criterion / Tiêu chuẩn luật hữu ích:**  
c(X→Y) > P(Y)

**Lift (Interest) Formula / Công thức Lift:**  
Lift = P(X,Y) / [P(X)×P(Y)]

| Lift | Interpretation / Diễn giải |
| :--- | :--- |
| > 1 | X & Y positively correlated / Tương quan dương |
| < 1 | X & Y negatively correlated / Tương quan âm |

---

## VII. Properties of Measures / Tính chất của thước đo

| Property | Meaning / Giải thích | Examples / Ví dụ |
| :--- | :--- | :--- |
| **Symmetric** | M(A,B)=M(B,A). Đổi thứ tự không đổi giá trị. | Support, Lift |
| **Asymmetric** | Phụ thuộc vào thứ tự vế trái/phải. | Confidence |
| **Inversion** | Hoán đổi “có” ↔ “không có” (X↔¬X). | φ-coefficient = invariant |
| **Null Addition** | Thêm giao dịch không chứa X hay Y. | Lift bị ảnh hưởng; Confidence không. |

---

## VIII. Practice Calculations / Bài tập tính toán

**Dataset:** N = 1000 (Tea = T, Coffee = C)

| | C | ¬C | Total |
| :--- | :--- | :--- | :--- |
| T | 150 | 50 | 200 |
| ¬T | 650 | 150 | 800 |
| **Total** | 800 | 200 | 1000 |

**1️⃣ Support({T,C})** = 150/1000 = 0.15 (15%)  
→ 15% giao dịch chứa cả Tea và Coffee.

**2️⃣ Confidence(T→C)** = 150/200 = 0.75 (75%)  
→ Nghe có vẻ mạnh nhưng có thể gây hiểu nhầm.

**3️⃣ Lift(T→C)** = 0.15 / (0.2×0.8) = 0.9375 < 1  
→ Tương quan âm – luật không thật sự hữu ích.

---

## IX. Multiple-Choice Review / Ôn tập trắc nghiệm

| # | Question / Câu hỏi | Answer / Đáp án |
| :-- | :-- | :-- |
| 1 | Which concept allows pruning of supersets? / Nguyên lý nào cho phép loại siêu tập ? | **Apriori (anti-monotone)** |
| 2 | Why is confidence not anti-monotone? | **Adding items shrinks denominator.** |
| 3 | Useful rule condition | **c(X→Y) > s(Y)** |
| 4 | Closed itemset definition | **No superset has same support.** |
| 5 | Lift = 0.5 means | **Negative correlation.** |
| 6 | Symmetric measures | **Support, Lift, Interest.** |

---

## X. Advanced Exercises / Bài tập nâng cao

### Example 1 – Milk & Bread
100 transactions → Milk = 30, Bread = 40, Milk∩Bread = 20  
Support = 0.2 Confidence = 0.667 Lift = 1.667 (positive)

### Example 2 – Change of Minsup  
minsup = 0.25, support({A,B}) = 0.22 → infrequent → prune supersets.

### Example 3 – Laptop & Mouse  
600 transactions: Laptop = 120, Mouse = 300, Both = 90  
Support = 0.15 Confidence = 0.75 Lift = 1.5 (positive)

### Example 4 – Bread & Butter  
P(Bread)=0.4, P(Butter)=0.3, P(Both)=0.25  
Confidence = 0.8333 Lift = 2.083 → Strong association.

---

## XI. Key Takeaways / Tổng kết

- **Support** → frequency / mức độ xuất hiện.  
- **Confidence** → reliability / mức độ tin cậy.  
- **Lift** → correlation strength / độ mạnh liên kết.  
- **Apriori principle** → enables efficient pruning.  
- **Closed vs Maximal** → trade-off between compactness and information.  

---

*End of Bilingual Association Rule Mining Guide – Tài liệu ôn tập song ngữ Khai thác Luật Kết Hợp*
