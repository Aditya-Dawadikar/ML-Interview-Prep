# Content-Based Filtering (CBF)

## 1. Overview

Content-Based Filtering recommends items based on **item features** and **user preferences derived from past interactions**.

> “Recommend items similar to what the user already liked”

Unlike Collaborative Filtering:

* No dependency on other users
* Fully **personalized per user**

---

## 2. Core Idea

We represent:

* **Items** → feature vectors
* **User** → preference vector

Then compute similarity:

[
score(u, i) = sim(user_profile, item_features)
]

---

## 3. Representation

### 3.1 Item Representation

Each item is converted into a feature vector.

Examples:

#### Text-based (movies, videos)

* TF-IDF / Bag of Words
* Embeddings (BERT, etc.)

```text id="4t84v8"
Movie: "Sci-fi space war"
→ [0.2, 0.8, 0.1, ...]
```

#### Structured features

* genre
* tags
* price
* duration

---

### 3.2 User Representation

User profile is derived from past interactions:

#### Simple approach (average)

[
user = \frac{1}{N} \sum item_vectors
]

#### Weighted approach

[
user = \frac{1}{N} \sum r_{u,i} \cdot item_i
]

Where:

* higher-rated items influence more

---

## 4. Similarity Function

Most common: **Cosine Similarity**

[
sim(u, i) = \frac{u \cdot i}{|u||i|}
]

Interpretation:

* higher score → more relevant item

---

## 5. Recommendation Pipeline

```text id="zrg4qf"
User History → Build User Profile
              ↓
         Candidate Items
              ↓
     Compute Similarity Scores
              ↓
         Top-K Selection
```

---

## 6. Example

User watched:

* Sci-fi movie
* Space documentary

User vector → high weight on:

* "space"
* "technology"

System recommends:

* Interstellar-like movies
* Space exploration videos

---

## 7. Advantages

### 7.1 Handles Cold Start (Items)

* New items can be recommended immediately if features are known

### 7.2 No Dependency on Other Users

* Works even with a single user

### 7.3 Explainability

* “Recommended because it’s similar to X”

---

## 8. Limitations

### 8.1 Limited Exploration

* Recommends similar items only
* Leads to **filter bubble**

### 8.2 Feature Engineering Heavy

* Quality depends on feature representation

### 8.3 Cold Start (Users)

* New users → no history

### 8.4 Overspecialization

* No diversity in recommendations

---

## 9. Improvements

### 9.1 Better Features

* Use embeddings (BERT, CLIP, etc.)
* Multi-modal features (text + image + audio)

### 9.2 Regularization

* Prevent overfitting to narrow preferences

### 9.3 Hybrid Models

* Combine with collaborative filtering

---

## 10. Variants

### 10.1 Profile-Based

* Explicit user profile

### 10.2 Learning-Based

* Train model to predict user-item relevance

Example:

```text id="b4x0w1"
ML model: f(user_features, item_features) → score
```

---

## 11. When to Use

Use when:

* rich item metadata is available
* user base is small or new
* cold start is important

Avoid when:

* features are weak or noisy
* diversity is critical

---

## 12. Comparison with Collaborative Filtering

```text id="pjcrcl"
CBF → uses item features
CF  → uses user interactions

CBF → no cold start for items
CF  → better at discovering hidden patterns
```

---

## 13. Connection to Modern Systems

Content-based filtering evolves into:

```text id="v17p7l"
Feature-based models → Deep encoders → Two-Tower models
```

* Item tower = advanced content encoder
* User tower = learned preference model

---

## 14. Summary

* CBF recommends based on **similarity of item features**
* Core steps:

  * represent items
  * build user profile
  * compute similarity
* Strong for cold start (items)
* Weak for diversity and exploration
