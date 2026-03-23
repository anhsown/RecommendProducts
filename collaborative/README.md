# Product Recommendation System (Collaborative Filtering)

## Overview

This project builds a recommendation system using Collaborative Filtering techniques on a Vietnamese skincare dataset.

Two main approaches are implemented:

- ALS (Alternating Least Squares) using PySpark
- Surprise Library (SVD, SVDpp, KNN, etc.)

The goal is to recommend products based on user behavior (ratings).

---

## Dataset

Files used:

- Danh_gia.csv (ratings)
- San_pham.csv (products)
- Khach_hang.csv (customers)

### Key Features

- ma_khach_hang: Customer ID
- ma_san_pham: Product ID
- so_sao: Rating (1-5)

### Statistics

- 21,575 ratings
- 4,485 users
- 686 products

---

## Tech Stack

- Python
- PySpark (ALS)
- Surprise Library
- Pandas, NumPy

---

## Pipeline

### 1. Data Processing

- Load data using Pandas (to avoid Spark parsing issues)
- Convert to PySpark DataFrame
- Select relevant columns:
  - user (ma_khach_hang)
  - item (ma_san_pham)
  - rating (so_sao)

---

### 2. ALS Model (PySpark)

#### Training

- Split data: 80% train / 20% test
- Parameters:
  - maxIter = 10
  - regParam = 0.09
  - nonnegative = True

#### Evaluation

- RMSE = 0.67
- R2 = 0.39
- MSE = 0.45

#### Insights

- Good prediction accuracy (RMSE < 1)
- Personalized recommendations
- Slower inference time (~2.7s)

---

### 3. Recommendation (ALS)

- Recommend top N products for a user
- Filter predicted rating > 3
- Join with product & customer info

---

### 4. Surprise Models

Algorithms tested:

- SVD
- SVDpp
- NMF
- KNN (multiple variants)
- CoClustering
- BaselineOnly

#### Best Model

- KNNBaseline: RMSE ≈ 0.72
- SVDpp: RMSE ≈ 0.73

Selected model: SVDpp

---

### 5. Surprise Recommendation

- Train on full dataset
- Predict rating for all items
- Sort and select Top-N

---

## Results Comparison

### ALS

- RMSE: ~0.66
- Highly personalized
- Diverse predictions
- Slower (~2.7s)

### Surprise (SVDpp)

- RMSE: ~0.73
- Faster (~0.03s)
- Less personalized (scores often similar)

---

## Conclusion

- ALS:
  - Better accuracy and personalization
  - Slower performance

- Surprise:
  - Faster and scalable
  - Lower personalization quality

Final Choice: Surprise (for better performance and deployment)

---

## Future Work

- Hybrid model (ALS + Content-based)
- Deploy API (FastAPI / Flask)
- Optimize Spark performance
- Add evaluation metrics (Precision@K, Recall@K)

---

## Author

Sparkle Sown

- Applied AI / Data Science
- Focus: Recommendation Systems
