# Ghi chú bảo vệ báo cáo (Report Notes)

## 1. Trọng số Meta-Learner trong Stacking Ensemble (Mục 3.6.1.3)

Trong `StackingClassifier`, mỗi bài đánh giá được chuyển thành 4 xác suất dự đoán ngoài mẫu (*out-of-fold*, 5-Fold Stratified CV) từ 4 mô hình cơ sở, sau đó đưa vào bộ học tầng hai (`LogisticRegression(class_weight='balanced')`):

$$\text{logit}\big(P(y=1)\big) = b + 4.0069\,p_{\text{logreg}} + 2.6973\,p_{\text{xgb}} + 2.1078\,p_{\text{nb}} - 1.1931\,p_{\text{svm}}$$

### Ý nghĩa từng trọng số (`final_estimator_.coef_`)
- **`logreg` (`+4.0069`):** Trọng số lớn nhất vì Logistic Regression trực tiếp tối ưu log-loss và xuất xác suất hậu nghiệm được hiệu chỉnh tốt trên không gian TF-IDF.
- **`xgb` (`+2.6973`) và `nb` (`+2.1078`):** Đều nhận trọng số dương đáng kể vì thuộc hai họ mô hình khác biệt (cây tăng cường và xác suất sinh), có sai số ít trùng lặp với nhóm tuyến tính → bổ sung **tính đa dạng** giúp Stacking đạt ROC-AUC cao nhất toàn thực nghiệm (`0.9526`).
- **`svm` (`-1.1931`) — Tại sao âm dù SVM là mô hình đứng đầu?**
  - **Không phải vì SVM kém hay dự đoán ngược.**
  - Nguyên nhân là hiện tượng **đa cộng tuyến (*multicollinearity*)**: cả `LinearSVC` (đã hiệu chỉnh qua `CalibratedClassifierCV`) và `LogisticRegression` đều là mô hình tuyến tính có điều chuẩn $L_2$ trên TF-IDF, nên xác suất đầu ra $p_{\text{svm}}$ và $p_{\text{logreg}}$ tương quan tuyến tính rất mạnh (gần như mang cùng một tín hiệu).
  - Khi hai biến đầu vào của Logistic Regression bị đa cộng tuyến mạnh, bộ tối ưu thường đẩy trọng số của một biến lên cao (`+4.0069`) và gán trọng số âm nhỏ cho biến còn lại (`-1.1931`) để hiệu chỉnh chênh lệch nhỏ giữa hai đường xác suất. Tổng đóng góp của nhóm tuyến tính vẫn dương mạnh ($+4.0069 - 1.1931 = +2.8138$).
