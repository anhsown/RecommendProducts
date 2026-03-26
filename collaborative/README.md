## Product Recommendation System using Content-Based Filtering
### A. Problem Statement

In modern e-commerce platforms, users are exposed to thousands of products, making it difficult to discover items that match their preferences. Traditional search mechanisms rely heavily on keyword matching, which often fails to capture the semantic similarity between products. As a result, users may miss relevant items that are described using different wording but serve similar purposes.

From a Natural Language Processing (NLP) perspective, product descriptions contain rich semantic information that can be leveraged to build intelligent recommendation systems. However, challenges arise due to:

Noisy product descriptions with mixed formats and special characters
High dimensionality of textual data
The need to measure semantic similarity efficiently across thousands of products
Ensuring recommendation quality while maintaining fast response time

This project addresses these challenges by implementing two content-based recommendation approaches:

TF-IDF + Cosine Similarity
Gensim (BoW + TF-IDF + Similarity Index)

The goal is to compare both methods in terms of recommendation quality and execution efficiency, and combine them into a hybrid evaluation framework.

### B. Dataset Overview
Source: E-commerce skincare product dataset
Total products: ~1,200 items
Main attributes used:
ma_san_pham (product id)
ten_san_pham (product name)
mo_ta (product description)
gia_ban, gia_goc
diem_trung_binh (average rating)

The product description (mo_ta) is the core textual feature used to compute similarity between products.

### C. Data Preprocessing

* To ensure high-quality textual features, multiple preprocessing steps were applied.

* Steps performed:
* Text normalization
* Remove extra whitespace
* Standardize text format
* Vietnamese word tokenization
* Applied word_tokenize to preserve compound words such as:
** da_dầu, chống_nắng, nhạy_cảm
* Lowercasing and special character removal
* Stop-word removal
* Final processed text columns
* content_gem_re → used for Gensim model
* processed_content → used for TF-IDF cosine model

These steps significantly reduce noise and improve semantic representation.

### D. Exploratory Data Analysis (EDA)

Key observations from product descriptions:

Most products contain repetitive domain terms such as:
dưỡng ẩm, chống nắng, da dầu, nhạy cảm, làm sạch
Descriptions are typically under 300 tokens
Products of the same brand/category share many semantic patterns

This confirms that content-based filtering is suitable for this dataset.

### E. Model 1: Gensim Content-Based Recommendation
Pipeline:
Create Dictionary from tokenized text
Convert documents to Bag of Words (BoW)
Transform BoW to TF-IDF vectors
Build Similarity Index
Compute similarity between input product and all products
Recommendation function:
Input: product_id or keyword
Output: Top 5 similar products with diem_trung_binh >= 3

This method works directly on sparse BoW representations and is memory-efficient.

### F. Model 2: TF-IDF + Cosine Similarity
Pipeline:
Use TfidfVectorizer on cleaned text
Create full TF-IDF matrix
Compute cosine similarity matrix (NxN)
Retrieve most similar items based on similarity score

This approach is straightforward and fast during querying due to precomputed similarity matrix.

### G. Recommendation by Product ID (Example)
Input product:
ma_san_pham: 422207117
Lotion Curél Dưỡng Ẩm Chuyên Sâu Cho Da Lão Hóa
Gensim Recommendations:
Product	Similarity Insight
Gel Dưỡng Da Curél Cho Da Dầu	Same brand, similar moisturizing function
Lotion Naris Dưỡng Ẩm	Similar skincare purpose
Gel Tẩy Trang Curél	Same brand care line
Sữa Dưỡng Da Curél	Same hydration category
Lotion Meishoku	Similar skin treatment usage
Cosine Similarity Recommendations:
Product	Similarity Insight
Lotion Naris Dưỡng Ẩm	High text similarity
Gel Dưỡng Da Curél	Brand & function similarity
Kem Dưỡng Hada Labo	Same moisturizing purpose
Gel Tẩy Trang Curél	Similar context words
Nước Dưỡng Naris	Similar skincare description

Both methods return highly relevant skincare hydration products.

### H. Recommendation by User Keyword (Example)
User input:
Keyword: "da dầu"
Gensim Results:

Mostly returns La Roche-Posay products targeting oily skin with cleansing features.

Cosine Results:

Also returns La Roche-Posay product line with similar descriptions.

This proves that both methods understand semantic meaning of keywords, not just literal matching.

### I. Performance Comparison
Method	Execution Time	Memory Usage	Quality
Gensim	~0.008s	Low	Very good
Cosine	~0.003s	Higher (NxN matrix)	Excellent
Cosine is faster at query time
Gensim is more scalable for larger datasets
### J. Hybrid Evaluation Function

A combined function was built to:

Run both methods
Measure execution time
Display results separately for comparison

This allows objective evaluation between approaches.

### K. Conclusion

In this project, we developed a content-based product recommendation system using two NLP approaches: Gensim TF-IDF similarity and TF-IDF cosine similarity.

The system successfully:

Extracted semantic meaning from Vietnamese product descriptions
Provided highly relevant recommendations by product ID and keyword
Demonstrated fast execution suitable for real-time applications
Showed that both approaches produce consistent and high-quality results
Strengths:
No need for user behavior data
Works purely on product descriptions
Scalable and efficient
Easy to deploy in e-commerce systems
Limitations:
Does not consider user preferences or purchase history
Sensitive to text quality
Cosine method consumes more memory for large datasets
Future Work:
Combine with collaborative filtering
Use transformer embeddings (BERT/XLM-R) for deeper semantic similarity
Add category/brand weighting

Overall, the results confirm that NLP-based content similarity is highly effective for building practical product recommendation systems in real-world e-commerce environments.

