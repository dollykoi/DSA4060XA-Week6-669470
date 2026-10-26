# DSA 4060XA, Week 6 Guided Evaluation Lab

**Evaluating Recommender Systems: Popularity Baseline, Content-Based Filtering, and Item-Based Collaborative Filtering**

## Student Details

- **Name:** Claire Mwarari
- **Student ID:** 669470
- **Course:** DSA 4060XA, Recommender Systems

## Lab Objective

To evaluate three recommender systems, a popularity baseline, a content-based model, and an item-based collaborative-filtering model, against unseen user-movie interactions using metrics that match the actual recommendation objective: rating accuracy (MAE, RMSE), ranking quality (Precision@K, Recall@K, NDCG@K), and beyond-accuracy measures (catalogue coverage, novelty, and diversity).

## Repository Structure

```
DSA4060XA-Week6-StudentID/
├── DSA4060XA_Week6_Guided_Evaluation_Lab.ipynb
├── DSA4060_Week6_Movies.csv
├── DSA4060_Week6_Ratings.csv
└── README.md
```

## Models Compared

1. **Popularity Baseline**, predicts each movie's average training rating (global mean for unseen movies) and ranks by average rating and rating count.
2. **Content-Based Filtering**, builds a user's genre preference profile from training ratings and scores movies by cosine similarity between the profile and binary genre features.
3. **Item-Based Collaborative Filtering**, computes item-item cosine similarity from the user-item rating matrix and predicts similarity-weighted averages of the user's ratings.

## Results

### Rating Prediction

| Model | MAE | RMSE |
|---|---|---|
| Popularity Baseline | 1.086 | 1.201 |
| Content-Based | 1.094 | 1.381 |
| **Item-Based CF** | **0.924** | **1.070** |

**Best model for rating prediction:** Item-Based Collaborative Filtering, with the lowest MAE and RMSE.

### Ranking (K = 3, relevance = rating ≥ 4)

| Model | Precision@3 | Recall@3 | NDCG@3 |
|---|---|---|---|
| Popularity Baseline | 0.067 | 0.200 | 0.100 |
| **Content-Based** | 0.067 | 0.200 | **0.200** |
| Item-Based CF | 0.000 | 0.000 | 0.000 |

**Best model for ranking:** The content-based model attains the strongest NDCG@3 (relevant items placed nearer the top of the list) while matching the baseline on precision and recall. The item-based CF model recovers no relevant test items at K = 3 in this small evaluation.

### Coverage, Novelty, and Diversity Findings

| Model | Coverage | Novelty | Diversity |
|---|---|---|---|
| Popularity Baseline | 0.35 | 1.689 | 0.736 |
| Content-Based | 0.70 | 1.344 | 0.542 |
| Item-Based CF | **0.85** | 1.695 | **0.780** |

- **Coverage:** The popularity baseline exposes only 35% of the catalogue, concentrating on the same few titles. Content-based (70%) and item-based CF (85%) recommend far more broadly.
- **Novelty:** The popularity baseline and item-based CF recommend comparably novel items; the content-based model leans toward somewhat more familiar titles.
- **Diversity:** Item-based CF produces the most within-list genre variety, followed by the baseline; the content-based model is the least diverse.

The K = 1, 3, 5 extension in the notebook shows precision decreasing and recall increasing as K grows, with the content-based model improving most noticeably at K = 5.

## Final Recommended Model

**Content-based filtering** is recommended for the stated business objective, a streaming service that wants users to discover relevant movies without popular titles dominating every list. It matches the baseline on precision and recall, achieves the best NDCG@3, and doubles the baseline's catalogue coverage. The trade-off accepted is a slightly higher MAE/RMSE than the baseline and item-based CF, which is acceptable since the objective is discovery-oriented ranking rather than exact rating prediction.

## Limitations

1. **Small dataset:** only 12 users, 20 movies, and 120 ratings; metric estimates are unstable and the item-based CF ranking metrics in particular are sensitive to this sparsity.
2. **Single held-out interaction per user:** each user contributes only one test item, so Precision@K and Recall@K are coarse and highly sensitive to the choice of K and the relevance threshold (rating ≥ 4).
3. **No cross-validation:** results depend on a single temporal split; repeated or cross-validated splits would give more reliable estimates.
4. **Cold start:** content-based and collaborative models cannot produce meaningful recommendations for users with no training history, and item-based CF cannot score items absent from the training similarity matrix (it then falls back to the global mean).
5. **Binary genre features:** the content-based model ignores richer signals such as actors, directors, descriptions, or rating magnitude within genres, limiting the depth of its personalization.

## Reproducibility

Open the notebook in Google Colab (or Jupyter), upload `DSA4060_Week6_Movies.csv` and `DSA4060_Week6_Ratings.csv` when prompted, and run all cells from top to bottom. All results are deterministic, no random seeds are required.
