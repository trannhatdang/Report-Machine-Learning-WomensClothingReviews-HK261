# Phân loại đánh giá quần áo nữ bằng học máy

Đồ án môn **Học máy (CO3117)** của Trường Đại học Bách Khoa - ĐHQG
TP. Hồ Chí Minh.

Đề tài xây dựng mô hình phân loại nhị phân để dự đoán khách hàng có đề xuất
sản phẩm hay không (`Recommended IND`) từ nội dung đánh giá quần áo nữ.

## Thành viên

| Thành viên | MSSV |
|---|---:|
| Nguyễn Quang Bảo | 2410276 |
| Cao Quang Đôn | 2310752 |
| Trần Nhật Đăng | 2310737 |

Giảng viên hướng dẫn: **ThS. Võ Thanh Hùng**

## Mục tiêu và dữ liệu

- **Dataset:** Women's E-Commerce Clothing Reviews.
- **Số mẫu:** 23.486 đánh giá.
- **Đầu vào chính:** ghép `Title` và `Review Text`.
- **Nhãn:** `Recommended IND` (0: không đề xuất, 1: đề xuất).
- **Đặc trưng mở rộng:** `Age`, `Division Name`, `Department Name`,
  `Class Name`.
- **Tiền xử lý:** xử lý giá trị thiếu, biểu diễn văn bản bằng TF-IDF và
  chia dữ liệu train/test theo tỷ lệ 80:20 với `random_state=42`.

Tập dữ liệu mất cân bằng (xấp xỉ 82,24% lớp 1 và 17,76% lớp 0), vì vậy
`Macro-F1`, `F1-score` từng lớp và `Recall` của lớp 0 được ưu tiên hơn
`Accuracy`. Cột `Rating` được loại bỏ khỏi đặc trưng để tránh rò rỉ mục tiêu.

## Các mô hình

Thực nghiệm chính so sánh:

- Linear SVM (`LinearSVC`);
- Logistic Regression;
- Multinomial Naive Bayes;
- XGBoost.

Ngoài ra, notebook hợp nhất có khảo sát TF-IDF, chọn đặc trưng, tối ưu
siêu tham số bằng `RandomizedSearchCV`, ensemble và một baseline deep
learning tùy chọn.

## Cấu trúc thư mục

```text
.
├── Content/
│   ├── Phan Tich/                       # Phân tích dữ liệu và EDA
│   ├── Giai Quyet Bai Toan/              # Các notebook thực nghiệm
│   └── So sanh, Phan Tich/               # So sánh, thảo luận, kết luận
├── figures/                              # Hình dùng trong báo cáo
├── ref/                                  # Tài liệu tham khảo
├── Womens Clothing E-Commerce Reviews.csv
├── main.tex                              # Mã nguồn báo cáo LaTeX
├── main.pdf                              # Báo cáo đã biên dịch
└── requirements.txt                      # Các gói Python
```

## Môi trường

- Python **3.13.x** (phiên bản đã dùng khi tạo các notebook).
- Jupyter Notebook hoặc JupyterLab.
- Có thể dùng GPU CUDA cho XGBoost nếu máy đã cài driver và bản XGBoost
  tương thích; nếu không, notebook sẽ chạy trên CPU.
- Các thư viện được ghim phiên bản trong
  [`requirements.txt`](requirements.txt).

## Cài đặt

Từ thư mục gốc của dự án, chạy trên PowerShell:

```powershell
py -3.13 -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
python -m pip install notebook
```

Nếu PowerShell chặn kích hoạt môi trường ảo, có thể chạy lệnh cài đặt trực
tiếp bằng:

```powershell
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
```

### Linux/macOS

Từ thư mục gốc của dự án, chạy:

```bash
python3.13 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
python -m pip install notebook
```

Nếu hệ thống không có lệnh `python3.13`, hãy dùng Python 3.13 đã cài đặt
trên máy để tạo môi trường ảo:

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
```

## Chạy thực nghiệm

Khởi động Jupyter từ thư mục gốc để các đường dẫn tương đối tới file CSV
được giữ nguyên:

```powershell
.\.venv\Scripts\python.exe -m jupyter notebook
```

Trên Linux/macOS:

```bash
source .venv/bin/activate
python -m jupyter notebook
```

Mở và chạy các notebook sau:

1. [`ML_BTL_Unified_Womens_Clothing_Reviews.ipynb`](Content/Giai%20Quyet%20Bai%20Toan/ML_BTL_Unified_Womens_Clothing_Reviews.ipynb):
   quy trình thực nghiệm đầy đủ và kết quả tổng hợp.
2. [`Dang_WomensECommerceClothingReviews_SVM.ipynb`](Content/Giai%20Quyet%20Bai%20Toan/Dang_WomensECommerceClothingReviews_SVM.ipynb):
   khảo sát SVM, kernel và các biến thể liên quan.
3. [`LogReg_NaiveBayes_XGBoost.ipynb`](Content/Giai%20Quyet%20Bai%20Toan/LogReg_NaiveBayes_XGBoost.ipynb):
   Logistic Regression, Naive Bayes và XGBoost.

Notebook hợp nhất là nguồn chạy chính. Khi chạy lại, nên **Restart Kernel
and Run All** để bảo đảm các biến và kết quả được tạo theo đúng thứ tự.
Notebook `LogReg_NaiveBayes_XGBoost.ipynb` có thể ghi các kết quả trung gian
vào thư mục `demo/`.

## Biên dịch báo cáo

Để biên dịch lại báo cáo, cần cài XeLaTeX và chạy từ thư mục gốc:

```powershell
xelatex -interaction=nonstopmode main.tex
xelatex -interaction=nonstopmode main.tex
```


## Tái hiện kết quả

- Giữ nguyên file CSV ở thư mục gốc và không đổi tên file.
- Chạy notebook với đúng thư mục làm việc là thư mục gốc của repository.
- Các phép chia dữ liệu và thực nghiệm chính dùng `random_state=42`.
- Kết quả có thể thay đổi nhẹ theo phiên bản thư viện, số luồng CPU và việc
  sử dụng GPU.
- XGBoost sẽ chậm hơn khi chạy CPU. Có thể chạy các mô hình tuyến tính và
  Naive Bayes trên CPU để kiểm tra pipeline nếu không có GPU.

## Tệp lớn và nguồn dữ liệu

File dữ liệu [`Womens Clothing E-Commerce Reviews.csv`](Womens%20Clothing%20E-Commerce%20Reviews.csv)
được cung cấp trong thư mục dự án.

Nguồn tham khảo dataset: [Kaggle - Women's E-Commerce Clothing Reviews](https://www.kaggle.com/datasets/nicapotato/womens-ecommerce-clothing-reviews).

## Báo cáo và quy định học thuật

Báo cáo đầy đủ nằm trong [`main.pdf`](main.pdf), mã nguồn báo cáo nằm trong
[`main.tex`](main.tex).