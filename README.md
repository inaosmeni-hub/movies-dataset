# MovieLens — Collaborative Filtering

**Notebook:** `collaborative_filtering.ipynb`  
**Dataset:** `movies.csv` · `ratings.csv` · `tags.csv` — 100,836 ratings, 610 users, 9,724 movies

## What the notebook covers

- Data loading and exploratory overview
- User-item matrix construction (610 × 9,724, sparsity 98.3%)
- Train/test split (80/20)
- User-Based Collaborative Filtering using cosine similarity
- Item-Based Collaborative Filtering using cosine similarity
- Top-10 recommendations for a sample user — both methods
- RMSE evaluation and actual vs predicted scatter plot
- User and item similarity heatmaps
- Side-by-side recommendation comparison
- Summary table: User-Based vs Item-Based

## Results

| Method | RMSE |
|---|---|
| User-Based CF | ~1.10 |
| Item-Based CF | ~1.05 |

## Observations

1. The user-item matrix has 98.3% sparsity — most users have rated fewer than 2% of available movies. This is the central challenge of collaborative filtering: the less overlap between users, the less reliable the similarity scores become.

2. User-Based CF finds users with similar rating patterns and recommends what they liked. It works well when users have enough shared ratings to compute meaningful similarity.

3. Item-Based CF recommends movies similar to ones the user already rated highly. It is more stable than User-Based CF because movie similarity changes slowly over time, unlike user preferences.

4. Cosine similarity measures the angle between rating vectors, not their magnitude. Two users who always prefer the same genres will score high similarity even if one gives 5s and the other gives 3s.

5. Both methods fail for new users and new movies with no rating history — the cold start problem. Content-based filtering handles this better by using movie metadata like genres and tags.

## Requirements

```
pandas, numpy, matplotlib, scikit-learn
```
