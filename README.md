# Phân loại review quần áo phụ nữ bằng Machine Learning

Đây là repository của bài tập lớn môn Học máy (CO3117), thực hiện tại
Trường Đại học Bách khoa - ĐHQG TP.HCM. Đề tài áp dụng các mô hình
**SVM, Naive Bayes, Logistic Regression và XGBoost** để dự đoán khách hàng
có đề xuất sản phẩm quần áo phụ nữ hay không.

## Bài toán

- **Dữ liệu:** [Women's E-Commerce Clothing Reviews trên Kaggle](https://www.kaggle.com/datasets/nicapotato/womens-ecommerce-clothing-reviews)
- **Số mẫu:** 23.486 review sản phẩm.
- **Nhãn:** `Recommended IND` (`1`: đề xuất, `0`: không đề xuất).
- **Đầu vào chính:** nội dung `Title` kết hợp với `Review Text`.
- **Đặc trưng mở rộng:** `Age`, `Division Name`, `Department Name` và
  `Class Name`.
- Cột `Rating` không được dùng làm đặc trưng để tránh rò rỉ mục tiêu.

Do dữ liệu mất cân bằng giữa hai lớp, kết quả được đánh giá bằng Accuracy,
Precision, Recall, F1-score, Macro-F1, ROC-AUC và confusion matrix thay vì
chỉ dùng Accuracy.

## Nội dung thực nghiệm

1. Khám phá và tiền xử lý dữ liệu.
2. So sánh CountVectorizer và TfidfVectorizer.
3. Lựa chọn đặc trưng bằng Chi-Square và ANOVA F-Score.
4. Huấn luyện các mô hình Logistic Regression, Multinomial Naive Bayes,
   XGBoost và SVM.
5. Tối ưu siêu tham số bằng `RandomizedSearchCV` với Stratified 5-Fold
   Cross-Validation.
6. Thử nghiệm Bagging/AdaBoost cho SVM và kết hợp metadata cho XGBoost.
7. So sánh mô hình, phân tích lỗi và diễn giải các từ ảnh hưởng đến dự đoán.

## Cấu trúc repository

```text
.
├── Content/
│   ├── Giai Quyet Bai Toan/
│   │   ├── ML_BTL_Unified_Womens_Clothing_Reviews.ipynb
│   │   ├── LogReg_NaiveBayes_XGBoost.ipynb
│   │   ├── Dang_WomensECommerceClothingReviews_SVM.ipynb
│   │   └── Womens Clothing E-Commerce Reviews.csv
│   ├── Phan Tich/
│   └── So sanh, Phan Tich/
├── figures/
├── Womens Clothing E-Commerce Reviews.csv
├── main.tex
├── main.pdf
├── ML-huong-dan-BTL.pdf
└── requirements.txt
```

## Cài đặt môi trường

Khuyến nghị sử dụng Python 3.10 trở lên và Jupyter Notebook hoặc JupyterLab.

```bash
python -m venv .venv

# Windows PowerShell
.venv\Scripts\Activate.ps1

# macOS/Linux
source .venv/bin/activate

pip install -r requirements.txt
```

Nếu chỉ cần chạy các notebook, có thể cài các thư viện chính:

```bash
pip install numpy pandas scikit-learn xgboost nltk matplotlib seaborn wordcloud jupyter
```

## Chạy notebook

Từ thư mục gốc của repository:

```bash
jupyter notebook
```

Sau đó mở một trong các notebook trong thư mục
`Content/Giai Quyet Bai Toan/` và chạy các cell theo thứ tự. Notebook
`ML_BTL_Unified_Womens_Clothing_Reviews.ipynb` là phiên bản hợp nhất của
quy trình thực nghiệm.

Các notebook đọc file CSV từ thư mục đang chứa notebook. Nếu chạy notebook
ở thư mục khác, hãy cập nhật biến đường dẫn dữ liệu trong các cell khởi tạo.

## Tài liệu

- [`main.pdf`](main.pdf): báo cáo đã biên dịch.
- [`main.tex`](main.tex): mã nguồn LaTeX của báo cáo.
- [`ML-huong-dan-BTL.pdf`](ML-huong-dan-BTL.pdf): tài liệu hướng dẫn bài tập lớn.
- [`figures/`](figures/): các hình ảnh kết quả thực nghiệm.

## Ghi chú

Đây là repository phục vụ mục đích học tập và nghiên cứu trong phạm vi môn
Học máy. Kết quả có thể thay đổi tùy phiên bản thư viện, random seed và môi
trường chạy.
