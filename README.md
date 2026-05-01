# Datathon 2026 — The Gridbreaker (Vòng 1)

Repository chứa source code và bài nộp của đội **2Boys2Biz** cho cuộc thi
**VinDatathon 2026 — Vòng Sơ loại** do VinTelligence (VinUniversity Data Science & AI Club) tổ chức.

**Kaggle competition:** https://www.kaggle.com/competitions/datathon-2026-round-1

## Cấu trúc thư mục

```
.
├── notebooks/
│   ├── 01_data_profiling.ipynb       # Validation FK/PK, business constraints
│   ├── 02_mcq_part1.ipynb            # Phần 1: 10 câu MCQ với code tính
│   ├── 03_eda_part2.ipynb            # Phần 2: 7 sections EDA (4 cấp Descriptive→Prescriptive)
│   └── 04_forecasting_part3.ipynb    # Phần 3: pipeline LightGBM + Prophet + SHAP
├── report/
│   ├── main.tex                      # Báo cáo NeurIPS LaTeX
│   └── main.pdf                      # PDF compiled
├── submissions/
│   └── submission.csv                # File nộp Kaggle (548 rows)
├── data/
│   └── raw/                          # 14 CSV gốc (KHÔNG commit theo competition rules)
├── requirements.txt
└── README.md
```

## Setup môi trường

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
python -m ipykernel install --user --name datathon --display-name "Python (datathon)"
```

## Tải data

Vào https://www.kaggle.com/competitions/datathon-2026-round-1/data → Download All →
giải nén vào `data/raw/`. Sau khi xong, `ls data/raw/` phải thấy **14 file CSV**.

## Reproduce kết quả

Notebook chạy tuần tự, mỗi notebook độc lập từ `data/raw/`:

```bash
jupyter lab
# Mở từng notebook và Run All:
# notebooks/01_data_profiling.ipynb       (kiểm tra data quality)
# notebooks/02_mcq_part1.ipynb            (10 đáp án MCQ Phần 1)
# notebooks/03_eda_part2.ipynb            (insights Phần 2)
# notebooks/04_forecasting_part3.ipynb    (sinh submissions/submission.csv cho Phần 3)
```

Hoặc chạy notebook 04 từ command line để xuất submission:

```bash
jupyter nbconvert --to notebook --execute notebooks/04_forecasting_part3.ipynb --inplace
# → submissions/submission.csv
```

## Compile báo cáo

```bash
cd report/
latexmk -xelatex main.tex     # → main.pdf (4 trang, NeurIPS format)
```

## Reproducibility

- Tất cả `random_seed = 42` (numpy, sklearn, lightgbm, xgboost).
- Time-series cross-validation dùng `TimeSeriesSplit` (không shuffle, giữ thứ tự thời gian).
- Notebook 04 deterministic — chạy lại từ đầu cho cùng kết quả submission.

## Khai báo tuân thủ Phần 3

(i) Không sử dụng `Revenue`/`COGS` từ tập test làm đặc trưng.
(ii) Không dùng dữ liệu ngoài bộ 14 file CSV được cung cấp.
(iii) Random seed = 42 cho mọi thư viện.
(iv) Toàn bộ pipeline có thể tái lập từ source code trong repo này.
