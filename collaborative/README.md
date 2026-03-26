## Product Recommendation System using Collaborative Filtering (ALS + Surprise)
### A. Problem Statement
* E-commerce platforms often face the challenge of recommending the right products to users from a vast catalog. Traditional search and filtering methods cannot capture personal user preferences effectively.
* Collaborative Filtering (CF) leverages historical user-item interactions (ratings, reviews, clicks) to model user preferences and make personalized recommendations. However, challenges include:
** Sparsity of user-product ratings (many products unrated by each user)
** Cold-start users or items with few ratings
** Scalability to large datasets with thousands of users and products
** Balancing recommendation accuracy with query speed
** This project addresses these challenges by implementing two CF approaches:
** ALS (Alternating Least Squares) – matrix factorization method optimized for implicit feedback
** Surprise SVDpp – explicit rating prediction model
** The goal is to compare both methods in terms of accuracy (RMSE), execution time, and recommendation quality, and explore a hybrid recommendation framework that combines the strengths of both.
### B. Dataset Overview
* Source: E-commerce skincare product dataset from Hasaki
* Total users: ~1,500
* Total products: ~1,200
* Main attributes used:
** ma_khach_hang (user ID)
** ma_san_pham (product ID)
** so_sao (rating, 1–5 stars)
** ten_san_pham (product name)
** gia_ban, gia_goc (price info)
* User ratings are the core feature used to model preferences.
### C. Data Preprocessing
* Steps performed:
** Checked and removed duplicate user-product ratings
** Filtered ratings to focus on meaningful interactions (e.g., so_sao >= 3 for positive preferences)
** Converted user and product IDs to numeric indices for ALS
** Split data into training and testing sets for evaluation
** These steps ensure high-quality input for collaborative filtering models and reduce bias in RMSE evaluation.
### D. Exploratory Data Analysis (EDA)
* Key observations:
* Users typically rate between 5–30 products, showing sparse interaction
* Most products have 10–50 ratings, sufficient for collaborative learning
* Ratings are skewed toward positive (3–5 stars), which reflects user satisfaction trends
* Users show distinct preferences for product types and brands
* These insights confirm that CF methods are suitable and can benefit from personalized user patterns.
### E. Model 1: ALS (Spark)
* Pipeline
** Train ALS model on user-product rating matrix
** Optimize latent factors to minimize reconstruction error on known ratings
** Generate top-N recommendations per user using recommendForUserSubset
* Evaluation:
** RMSE = 0.66 → demonstrates strong predictive performance
** Predicted ratings for top recommendations: 6.19–6.66 (scaled implicit feedback)
** Execution time per user: ~2.73 seconds
** Pros: Personalized, captures user taste well, handles large datasets efficiently
** Cons: Slightly slower for real-time querying, may require tuning for cold-start users
### F. Model 2: Surprise
* Pipeline

** Model Selection and Training:

*** Algorithms tested

| Algorithm       | RMSE (Mean) | Elapsed Time (s) |
|-----------------|------------|----------------|
| KNNBaseline     | 0.720      | 7.88           |
| SVDpp           | 0.729      | 6.90           |
| KNNBasic        | 0.753      | 5.75           |
| SVD             | 0.777      | 4.91           |
| KNNWithZScore   | 0.794      | 8.33           |
| KNNWithMeans    | 0.798      | 6.08           |
| BaselineOnly    | 0.810      | 0.58           |
| CoClustering    | 0.819      | 4.14           |
| SlopeOne        | 0.826      | 0.90           |
| NMF             | 0.869      | 7.88           |

*** Observation:

**** Lower RMSE indicates higher predictive accuracy.
**** SVDpp achieves low RMSE (0.729) while maintaining reasonable runtime (~6.9s).
<img width="1396" height="520" alt="image" src="https://github.com/user-attachments/assets/48585272-3f4f-444f-a3c2-457759f10592" />
****  -> Choose SVDpp

** Train SVDpp on explicit ratings
** Predict ratings for all user-product pairs
** Select top-N products based on estimated scores
* Evaluation:
** RMSE ~ 0.72 (slightly higher than ALS)
** Predicted scores mostly uniform (5.0) for top recommendations
** Execution time per user: ~0.03 seconds (very fast)
** Pros: Extremely fast for generating recommendations
** Cons: Lacks strong personalization in some cases; top predictions may have similar scores
### G. Hybrid Recommendation Function
* Approach: Combine ALS + Surprise predictions:
** Generate top-N recommendations from ALS (personalized, higher accuracy)
** Generate top-N recommendations from Surprise (fast scoring)
**Combine results into a single DataFrame for comparison
** Include predicted rating and time taken for transparency
* Benefits:
Captures ALS personalization while leveraging fast Surprise scoring
** Enables trade-off between accuracy and response speed
** Supports comparative evaluation per user
### H. Recommendation Examples
* User ID = 23

** ALS Recommendations:

| Product | Predicted Rating | Notes |
|---------|----------------|-------|
| Sữa Rửa Mặt Naris Acmedica | 6.66 | High personalization, matches user history |
| Nước Dưỡng Da Chinoshio | 6.41 | Matches brand preference |
| Serum B.O.M 8 Loại Trà | 6.19 | Related to previously rated products |

* Surprise Recommendations:

| Product | Predicted Rating | Notes |
|---------|----------------|-------|
| Nước Dưỡng Da Chinoshio | 5.0 |	Fast scoring, limited personalization |
| Dầu Tẩy Trang Kosé Softymo |	5.0 |	Matches general user preference |
* Observations:
** ALS gives a more diverse set of high-quality personalized products
** Surprise gives uniformly high scores and is extremely fast
### I. Performance Comparison
* Model	RMSE	Time per User	Notes
* ALS	0.66	2.73 s	Highly personalized, slightly slower
* Surprise	0.72	0.03 s	Fast, less personalized, scores less diverse
* ALS is preferred for user experience where personalization matters
* Surprise is suitable for high-throughput systems prioritizing speed
### J. Strengths and Limitations

* Strengths:

** Collaborative filtering captures actual user preferences
** ALS is robust to sparse datasets
** Surprise enables fast rating predictions

* Limitations:

** Cold-start problem for new users or items
** ALS slower in real-time queries
** Surprise lacks strong differentiation between top products

* Future Work:

** Hybridize ALS + Surprise to optimize speed and personalization
** Integrate content-based features (product descriptions) for cold-start scenarios
** Explore transformer embeddings (BERT/XLM-R) for semantic understanding
** Implement real-time incremental updates to ALS model
### K. Conclusion
* This project demonstrates a collaborative filtering product recommendation system using ALS and Surprise.
* ALS produces highly personalized recommendations with low RMSE, but takes longer to compute.
* Surprise provides fast recommendations but lacks score diversity.
* A hybrid approach combining both methods can balance accuracy and efficiency.
The results confirm that CF is highly effective in delivering relevant product suggestions in e-commerce platforms, particularly when combined with performance-aware strategies.
