# Datathon 2026 — The Gridbreakers

## Cấu trúc thư mục

```
project/
├── data/
│   └── raw_data/
│       ├── analytical/     sales.csv, sample_submission.csv
│       ├── master/         customers, geography, products, promotions
│       ├── operational/    inventory, web_traffic
│       └── transaction/    orders, payments, returns, reviews, shipments
│
├── part2/                  EDA & Report
├── part3/
│   └── datathon_optimized.ipynb   ← file chạy chính
├── requirements.txt
└── README.md
```

---

## Pipeline (Part 3)

```
Raw Data ──► Feature Engineering
              │  Calendar · Tết · Sale Seasons · Peak Proximity
              │  Promotions · Web Traffic · Inventory
              │  Lag / Rolling / EWM features
              ▼
    ┌──────────────────────────────────────┐
    │   Prophet Trend Extraction (per fold) │──► global + seasonal trend
    └──────────────────────────────────────┘
              │  residual = log(Revenue) − prophet_trend
              ▼
    ┌──────────────────────────────────────┐
    │   Layer 1 — Base Models (Huber Loss)  │
    │   XGBoost   reg:pseudohubererror      │
    │   LightGBM  huber              OOF ──►│──► stacked OOF matrix
    │   CatBoost  Huber:delta=1.5           │
    └──────────────────────────────────────┘
              ▼
    ┌──────────────────────────────────────┐
    │   Layer 2 — HuberRegressor Meta-Model │──► final log_Revenue
    └──────────────────────────────────────┘
              ▼
    COGS = Revenue × COGS_Ratio   (separate parallel stacking)
```

---

## Cài đặt

```bash
pip install -r requirements.txt
```

---

## Chạy Part 3

1. Mở `Part3/Part3.ipynb`
2. Kiểm tra `DATA_DIR` ở cell 2 trỏ đúng vào `data/raw_data`
3. **Run All** — chạy tuần tự từ trên xuống


Output: `submission.csv` trong thư mục `part3/`

---

## Lưu ý

- Chạy **đúng thứ tự** các cell, không bỏ qua
