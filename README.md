# Product Recommendation System  
## Vietnamese Skincare Dataset

---

## Overview

This project develops a Content-Based Recommendation System for skincare products using Vietnamese NLP techniques.

The system suggests similar products based on textual descriptions, helping users discover relevant items efficiently.

### Key Features

- Recommend products by Product ID  
- Recommend products by user keywords  
- Compare two approaches:
  - Gensim (TF-IDF + Similarity Index)  
  - Cosine Similarity (Scikit-learn)  

---

## System Architecture

Raw Data (CSV)
      ↓
Text Preprocessing
      ↓
Vectorization (TF-IDF)
      ↓
Similarity Computation
      ↓
Recommendation Engine
---

## Dataset

- File: `San_pham.csv`  
- Size: ~1200 products  

### Features

- ma_san_pham: Product ID  
- ten_san_pham: Product Name  
- gia_ban: Selling Price  
- gia_goc: Original Price  
- mo_ta: Product Description  
- diem_trung_binh: Average Rating  

---

## Tech Stack

- Python  
- Gensim  
- Scikit-learn  
- Underthesea (Vietnamese NLP)  
- Pandas, NumPy  

---

## Pipeline

### Text Preprocessing

- Normalize text  
- Tokenize using underthesea  
- Remove:
  - Stopwords  
  - Numbers  
  - Special characters  
- Convert to lowercase  

---

### Vectorization

#### Gensim

- Dictionary  
- Bag of Words  
- TF-IDF Model  

#### Scikit-learn

- TfidfVectorizer  
- TF-IDF Matrix  

---

### Similarity Computation

- Gensim: SparseMatrixSimilarity  
- Sklearn: cosine_similarity  

---

## Recommendation Functions

### By Product ID

get_recommendations_gen(product_id)  
get_recommendations_cos(product_id)  

---

### By Keyword

get_recommendations_by_keyword_gen(keyword)  
get_recommendations_by_keyword_cos(keyword)  

---

### Compare Models

combined_recommendations(product_id)  

---

## Results

### Quality Comparison

- Gensim:
  - High accuracy  
  - Very high relevance  
  - Low diversity  

- Cosine:
  - Medium accuracy  
  - Noisy results  
  - Higher diversity  

---

### Performance

- Gensim: ~0.008s  
- Cosine: ~0.003s  

Cosine is faster, but Gensim provides better recommendations.

---

## Example

Input:

Curél Moisturizing Lotion  

Output:

- Curél Moisturizing Gel  
- Hydrating Lotion  
- Moisturizing Milk  
- Similar products (same brand and function)  

---

## Business Impact

### For Users

- Faster product discovery  
- Better recommendations  
- Improved shopping experience  

---

### For Businesses

- Increase conversion rate  
- Boost cross-sell / up-sell  
- Extract insights from product content  

---

## Technical Highlights

- Applied Vietnamese NLP pipeline  
- Built end-to-end recommendation system  
- Compared two industry-standard approaches  
- Designed scalable pipeline  

---

## Conclusion

- Gensim:
  - Better accuracy and relevance  

- Cosine:
  - Faster and scalable  

Final choice: Gensim  

---

## Future Improvements

- Apply BERT / Sentence Transformers  
- Build hybrid recommendation system  
- Deploy API (FastAPI / Flask)  
- Build web app (Streamlit / React)  
- Improve diversity and reduce bias  

---

## Installation

pip install gensim underthesea scikit-learn pandas numpy  

---

## Usage

get_recommendations_gen(123)  
get_recommendations_by_keyword_gen("dưỡng ẩm")  

---

## Author

Sparkle Sown  

- Applied AI / Data Science  
- Focus: NLP, Recommendation Systems 
