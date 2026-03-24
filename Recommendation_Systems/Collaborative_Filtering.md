# Collaborative Filtering (CF)

## 1. Overview

Collaborative Filtering is a recommendation technique based on the assumption:

> Users with similar past behavior will have similar future preferences.

Instead of relying on item features, CF uses **user–item interaction patterns** (ratings, clicks, views).

---

## 2. Core Idea

We model the system as a matrix:

```text
            Items →
        i1   i2   i3   i4
u1      5    ?    3    ?
u2      4    2    ?    1
u3      ?    5    4    ?
↓ Users
```

Goal:

> Predict missing values (e.g., how much u1 will like i2)

---

## 3. Types of Collaborative Filtering

### 3.1 User–User Collaborative Filtering

* Find users similar to a target user
* Recommend items liked by similar users

#### Steps:

1. Represent each user as a vector of item ratings
2. Compute similarity between users
3. Select top-K similar users
4. Predict rating via weighted average

---

### 3.2 Item–Item Collaborative Filtering

* Find items similar to items the user liked
* Recommend similar items

#### Steps:

1. Represent each item as a vector of user ratings
2. Compute similarity between items
3. Recommend items similar to previously liked items

👉 More scalable and stable than user-user

---

## 4. Similarity Metrics

Most common: **Cosine Similarity**

[
\cos(\theta) = \frac{u \cdot v}{|u||v|}
]

Interpretation:

* 1 → identical preferences
* 0 → unrelated
* -1 → opposite (rare in sparse data)

Other metrics:

* Pearson correlation (handles rating bias)
* Jaccard similarity (binary interactions)

---

## 5. Prediction Function

For user-user CF:

[
\hat{r}*{u,i} = \frac{\sum*{v \in N(u)} sim(u,v) \cdot r_{v,i}}{\sum_{v \in N(u)} |sim(u,v)|}
]

Where:

* (N(u)) = similar users
* (sim(u,v)) = similarity
* (r_{v,i}) = rating

---

## 6. Matrix Factorization (Modern CF)

Instead of sparse matrix:

[
R \approx U \cdot V^T
]

Where:

* (U) = user embeddings
* (V) = item embeddings

Prediction:

[
\hat{r}_{u,i} = u_u^T v_i
]

👉 Learns latent features like:

* genre preference
* style affinity

---

## 7. Training Objective

Minimize reconstruction error:

[
\sum_{(u,i) \in observed} (r_{u,i} - u_u^T v_i)^2 + \lambda(||u_u||^2 + ||v_i||^2)
]

* Regularization prevents overfitting

---

## 8. Advantages

* No need for item metadata
* Captures hidden patterns
* Works well with dense interaction data

---

## 9. Limitations

### 9.1 Sparsity

* Most users interact with few items
* Similarity becomes unreliable

### 9.2 Cold Start

* New users → no history
* New items → no interactions

### 9.3 Scalability

* User-user similarity = O(n²)

### 9.4 Popularity Bias

* Popular items dominate recommendations

### 9.5 Grey Sheep Problem

* Users belonging to multiple clusters

### 9.6 Black Sheep Problem

* Users not similar to any group

---

## 10. Practical Improvements

* Normalize ratings (remove user bias)
* Use item-item CF instead of user-user
* Apply dimensionality reduction (SVD)
* Use implicit feedback (clicks, watch time)

---

## 11. When to Use CF

Use when:

* Rich interaction data exists
* Item features are limited or unavailable

Avoid when:

* Cold start is dominant
* Data is extremely sparse

---

## 12. Connection to Modern Systems

Collaborative Filtering evolves into:

```text
Matrix Factorization → Embeddings → Two-Tower Models
```

* CF = shallow embedding learning
* Two-tower = deep, feature-aware extension

---

## 13. Summary

* CF learns from **user behavior patterns**
* Core operations:

  * similarity computation
  * latent embedding learning
* Works well in data-rich environments
* Struggles with cold start and sparsity
