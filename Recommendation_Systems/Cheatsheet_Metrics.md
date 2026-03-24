# 📊 Recommendation System Metrics Cheat Sheet

# 0. Mental Model (don’t skip)

```text
Offline → “Is my model good?”
Online  → “Are users happier?”
System  → “Is my system working?”
```

---

# 1. OFFLINE METRICS (Model Evaluation)

## 1.1 Precision@K

### What

Fraction of recommended items that are relevant.

### Formula

[
Precision@K = \frac{\text{# relevant items in top K}}{K}
]

### Used For

* ranking quality (top-K purity)
* debugging noisy recommendations

### Notes

* ignores missing relevant items
* biased if K is small

---

## 1.2 Recall@K

### What

Fraction of relevant items that were retrieved.

### Formula

[
Recall@K = \frac{\text{# relevant items in top K}}{\text{total relevant items}}
]

### Used For

* retrieval stage evaluation
* two-tower / ANN quality

### Notes

* important for **candidate generation**

---

## 1.3 NDCG@K (Most Important Offline Metric)

### What

Measures ranking quality with position importance.

### Formula

[
DCG = \sum_{i=1}^{K} \frac{rel_i}{\log_2(i+1)}
]
[
NDCG = \frac{DCG}{IDCG}
]

### Used For

* ranking model evaluation
* listwise training

### Notes

* top positions matter more
* standard for ranking problems

---

## 1.4 MAP (Mean Average Precision)

### What

Average precision across relevant items.

### Used For

* information retrieval style evaluation

### Notes

* less common than NDCG in modern recsys

---

## 1.5 AUC (Area Under Curve)

### What

Probability that a positive item is ranked above a negative one.

### Used For

* pairwise ranking evaluation

### Notes

* ignores position importance

---

## 1.6 Coverage

### What

Fraction of items recommended across users.

### Formula

[
Coverage = \frac{\text{unique items recommended}}{\text{total items}}
]

### Used For

* diversity at system level

---

## 1.7 Popularity Bias

### What

How skewed recommendations are toward popular items.

### Used For

* detecting “everything is trending content”

---

# 2. ONLINE METRICS (User Behavior)

## 2.1 CTR (Click Through Rate)

### What

Probability user clicks recommended item.

### Formula

[
CTR = \frac{\text{clicks}}{\text{impressions}}
]

### Used For

* ranking model evaluation
* A/B testing

### Notes

⚠️ Easy to game (clickbait problem)

---

## 2.2 Watch Time / Dwell Time

### What

Time user spends consuming content.

### Used For

* deeper engagement
* video/music systems

### Notes

Better than CTR

---

## 2.3 Completion Rate

### What

Fraction of content fully consumed.

### Formula

[
Completion = \frac{\text{completed views}}{\text{total views}}
]

### Used For

* content quality + satisfaction

---

## 2.4 Retention (D1, D7, etc.)

### What

Users returning after 1 day, 7 days, etc.

### Used For

* long-term success metric

### Notes

🚨 Most important business metric

---

## 2.5 Session Length

### What

Time spent per session.

### Used For

* engagement tracking

---

## 2.6 Skip Rate (Critical)

### What

Fraction of items quickly skipped.

### Formula

[
Skip = \frac{\text{skipped items}}{\text{shown items}}
]

### Used For

* negative feedback
* ranking tuning

---

## 2.7 Negative Feedback Rate

### What

* dislikes
* hides
* reports

### Used For

* safety + quality

---

## 2.8 Diversity

### What

Variety of recommendations.

### Metrics

* category entropy
* unique creators
* intra-list similarity

---

## 2.9 Novelty

### What

How new/unseen items are.

### Used For

* exploration quality

---

## 2.10 Freshness

### What

Recency of recommended content.

### Used For

* content platforms (news, reels)

---

# 3. SYSTEM METRICS (Infra Health)

## 3.1 Latency

### What

Time to serve recommendation.

### Metrics

* P50 / P95 / P99

### Used For

* SLA guarantees

---

## 3.2 Throughput

### What

Requests handled per second (QPS)

---

## 3.3 Error Rate

### What

Failed recommendation requests

---

## 3.4 Feature Freshness

### What

Lag between event and feature update

### Used For

* real-time correctness

---

## 3.5 Fallback Rate (Important)

### What

% of requests using fallback:

* cached results
* offline-only results

### Used For

* detecting silent failures

---

## 3.6 ANN Recall Proxy

### What

How good approximate retrieval is vs exact

---

# 4. HYBRID SYSTEM METRICS

## 4.1 Source Contribution

### What

% of recommendations from:

* offline
* ANN
* trending
* session-based

---

## 4.2 Source Performance

### What

CTR / engagement per source

---

## 4.3 Online vs Offline Uplift

### What

Improvement due to online system

---

## 4.4 Exploration Effectiveness

### What

Performance of exploration items

---

# 5. BUSINESS METRICS (Often Forgotten)

## 5.1 Revenue / Ads CTR

## 5.2 Creator Exposure / Fairness

## 5.3 Content Distribution Balance

---

# 6. WHICH METRIC WHERE? (Quick Map)

```text
Retrieval Stage:
    Recall@K

Ranking Stage:
    NDCG, CTR

System Health:
    Latency, Fallback Rate

User Satisfaction:
    Watch Time, Retention

Exploration:
    Novelty, Coverage

Hybrid Debugging:
    Source Contribution
```

---

# 7. Common Pitfalls (Important)

### ❌ Optimizing only CTR

→ leads to clickbait

### ❌ Ignoring skip rate

→ poor UX

### ❌ Ignoring diversity

→ repetitive feed

### ❌ Ignoring retention

→ short-term gains, long-term loss

---

# 8. Final Mental Model

```text
Offline metrics → filter bad models
Online metrics  → validate real impact
System metrics  → ensure reliability
```

---

# 9. If you remember only 6 metrics

* Recall@K
* NDCG@K
* CTR
* Watch Time
* Retention
* P99 Latency
