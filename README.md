# Diabetes Prediction (Tiểu đường) — Machine Learning (Notebook)

Dự án dự đoán **khả năng mắc bệnh tiểu đường** dựa trên các chỉ số y tế cơ bản trong bộ dữ liệu `tieu_duong.csv`. Project được triển khai dưới dạng **Jupyter Notebook** để dễ theo dõi quy trình EDA → tiền xử lý → huấn luyện mô hình → đánh giá.

## Tổng quan

- **Bài toán**: Phân loại nhị phân (có/không tiểu đường).
- **Dữ liệu**: `tieu_duong.csv` (các đặc trưng sức khỏe + nhãn `Outcome`).
- **Mô hình trong notebook**:
  - `DecisionTreeClassifier`
  - `RandomForestClassifier`
  - `KNeighborsClassifier (KNN)`
- **Đánh giá**:
  - `accuracy_score`, `precision_score`, `recall_score`
  - `classification_report`
  - `confusion_matrix` + heatmap
  - `cross_val_score` (CV=5) để so sánh độ ổn định
- **Tuning/khảo sát tham số**:
  - Có sử dụng `GridSearchCV` / `RandomizedSearchCV` và các thử nghiệm tham số (ví dụ `max_depth`, `n_estimators`…)
  - Có phần minh họa đường cong **Train/Test Accuracy** theo tham số
- **Diễn giải**: có sử dụng `permutation_importance` để xem mức độ đóng góp của đặc trưng.

## Cấu trúc thư mục

```
diabetes-prediction-master/
├─ DuDoanBenh_Tieu_Duong.ipynb   # Notebook chính
├─ tieu_duong.csv               # Dữ liệu đầu vào
└─ README.md                    # Tài liệu dự án
```

## Mô tả dữ liệu

File: `tieu_duong.csv`

Các cột chính (trích từ header):

- **Number of times pregnant**: Số lần mang thai
- **Plasma glucose concentration a 2 hours in an oral glucose tolerance test**: Nồng độ glucose sau 2 giờ (OGTT)
- **Iastolic blood pressure**: Huyết áp tâm trương (lưu ý: trong file đang viết “Iastolic”, có thể là “Diastolic”)
- **Triceps skin fold thickness**: Độ dày nếp gấp da (Triceps)
- **2-Hour serum insulin**: Insulin huyết thanh sau 2 giờ
- **Body mass index**: BMI
- **Diabetes pedigree function**: Chỉ số di truyền tiểu đường
- **Age**: Tuổi
- **Outcome**: Nhãn mục tiêu (0/1)

## Yêu cầu môi trường

- **Python**: 3.9+ (khuyến nghị 3.10/3.11)
- **Thư viện** (theo notebook):
  - `pandas`, `numpy`
  - `matplotlib`, `seaborn`
  - `scikit-learn`
  - `jupyter` hoặc `jupyterlab`

Cài nhanh:

```bash
pip install -U pandas numpy matplotlib seaborn scikit-learn jupyter
```

## Cách chạy (Windows / PowerShell)

### Chạy bằng 1 lệnh

```bash
.\scripts\run.ps1
```

Script sẽ tự tạo môi trường ảo `.venv` (nếu chưa có), cài dependencies từ `requirements.txt`, rồi mở JupyterLab.

1) Mở terminal tại thư mục project:

```bash
cd "C:\Users\PC\Downloads\diabetes-prediction-master"
```

2) (Khuyến nghị) Tạo môi trường ảo:

```bash
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

3) Cài thư viện:

```bash
pip install -U pandas numpy matplotlib seaborn scikit-learn jupyter
```

4) Chạy Jupyter:

```bash
jupyter lab
```

5) Mở notebook `DuDoanBenh_Tieu_Duong.ipynb` và **Run All**.

### Lưu ý quan trọng về đường dẫn dataset

Trong notebook đang có đoạn đọc dữ liệu dạng Google Colab:

- `df = pd.read_csv('/content/tieu_duong.csv')`

Khi chạy local, hãy đổi thành:

- `df = pd.read_csv('tieu_duong.csv')`

## Kết quả & đánh giá

Notebook có phần so sánh mô hình bằng các chỉ số và cross-validation. Ngoài ra có:

- Ma trận nhầm lẫn (confusion matrix)
- Báo cáo phân loại (classification report)
- Khảo sát ảnh hưởng tham số (ví dụ độ sâu cây, số lượng cây…)
- Permutation importance để xem đặc trưng nào tác động nhiều đến dự đoán

> Lưu ý: Kết quả có thể thay đổi theo cách chia train/test, random seed, chuẩn hoá dữ liệu, và cấu hình tham số.

## Screenshot / biểu đồ kết quả

Các ảnh dưới đây được **trích trực tiếp từ output trong notebook** (đã xuất ra thư mục `assets/`):

- Confusion matrix / heatmap:

![](assets/notebook-01.png)

- Ví dụ các biểu đồ/đầu ra khác:

![](assets/notebook-02.png)
![](assets/notebook-03.png)

## Gợi ý cải tiến (tuỳ chọn)

- Tách pipeline `StandardScaler` + model bằng `sklearn.pipeline.Pipeline`
- Dùng `StratifiedKFold` để CV ổn định hơn cho phân loại
- Lưu model tốt nhất (joblib/pickle) và viết script inference (`predict.py`)
- Thêm `requirements.txt` để tái lập môi trường dễ dàng

## Đóng góp

Bạn có thể mở PR/issue để:

- chuẩn hoá tên cột, làm sạch dữ liệu,
- thêm mô hình khác (Logistic Regression, XGBoost/LightGBM…),
- hoặc đóng gói thành API/CLI.

