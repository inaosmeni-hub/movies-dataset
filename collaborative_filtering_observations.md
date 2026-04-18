# MovieLens — Collaborative Filtering

## Abstract

This notebook implements user-based and item-based collaborative filtering on the MovieLens dataset — 100,836 ratings from 610 users across 9,724 movies. A user-item matrix is constructed and cosine similarity is computed between users and between items separately. Both approaches generate top-10 movie recommendations for individual users and are evaluated with RMSE on a held-out 20% test set. Results are compared through RMSE scores, actual vs predicted scatter plots, similarity heatmaps, and a side-by-side recommendation comparison.

---

## Written Observations

**1. Matrix Sparsity is the Fundamental Challenge of Collaborative Filtering**
The user-item matrix contains 610 × 9,724 = 5,931,640 possible entries, of which only 100,836 are filled — a sparsity of 98.3%. This means the average user has rated fewer than 2% of all available movies. Cosine similarity between two users is only meaningful when they have rated many of the same movies, so with high sparsity most user pairs have very little overlap and similarity scores become unreliable. This is why RMSE remains around 1.10 even with a reasonable neighbourhood size — the sparse overlap limits how precisely ratings can be predicted.

**2. User-Based CF Finds People Who Think Like You**
User-Based Collaborative Filtering computes cosine similarity between every pair of users based on their rating vectors. To generate recommendations for a target user, it finds the most similar users, collects the movies those neighbours have rated that the target user has not seen, and predicts ratings as a weighted average of neighbour ratings weighted by their similarity score. This approach is intuitive and works well when users have enough shared ratings, but it requires recomputing similarities whenever new ratings arrive, which is expensive at scale.

**3. Item-Based CF is More Stable for Large Catalogues**
Item-Based Collaborative Filtering computes cosine similarity between every pair of movies based on how users rated them. To recommend movies for a user, it finds movies similar to ones the user already rated highly and ranks candidates by weighted similarity. The key advantage over User-Based CF is stability — movie similarity changes very slowly because movies do not acquire new characteristics over time, whereas user preferences can shift. The item similarity matrix can be precomputed offline and updated periodically, making Item-Based CF more scalable and predictable in production.

**4. Cosine Similarity Captures Taste Direction But Not Rating Scale**
Cosine similarity measures the angle between two rating vectors in high-dimensional space, ignoring their magnitudes. Two users who consistently prefer the same genres will have high cosine similarity even if one habitually gives 5-star ratings and the other gives 3-star ratings. In practice this means the model may identify the right movies to recommend but predict their ratings at the wrong scale. Mean-centring the ratings before computing similarity — subtracting each user's average rating from their ratings — is a common improvement that addresses this scale mismatch.

**5. Both Methods Fail for New Users and New Movies**
Collaborative filtering relies entirely on historical rating data. A new user who has not yet rated any movies cannot be matched to similar users in User-Based CF, and their item preferences cannot be inferred in Item-Based CF. Similarly, a movie that has received no ratings has no similarity to any other item. This cold start problem is a structural limitation of pure collaborative filtering. Content-based filtering, which uses movie metadata such as genres, cast, and tags, does not suffer from this limitation because recommendations can be generated from item features alone without any rating history.

---

*Keywords: collaborative filtering, user-based, item-based, cosine similarity, user-item matrix, sparsity, cold start, RMSE, MovieLens, recommendation system*
