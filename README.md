# 📱 TikTok Content Moderation Analytics
**Automating Video Classification to Streamline Human Review**

## 🎯 Executive Summary
TikTok users generate millions of videos daily, creating a massive moderation backlog. Data indicates that users who violate the platform's terms of service are significantly more likely to be presenting a "claim" rather than an "opinion"[cite: 2]. 

This project leverages machine learning to automatically classify videos as claims or opinions[cite: 2]. By accurately identifying claims, we can filter out opinions and efficiently prioritize high-risk videos for human moderators[cite: 2]. 

**Bottom-Line Result:** By upgrading from a baseline Logistic Regression model to an advanced Random Forest Classification Tree, the model's ability to accurately catch claim videos improved to nearly **100%**, providing a highly reliable automated sorting mechanism[cite: 2].

---

## 🗂️ Project Workflow

This analysis was structured using the **PACE** (Plan, Analyze, Construct, Execute) framework[cite: 1, 2].

| Stage | Key Actions |
| :--- | :--- |
| **Plan** | Defined the business goal: build a classification model to mitigate misinformation[cite: 2]. Selected **Recall** as the primary metric to minimize false negatives (missing actual claims)[cite: 2]. |
| **Analyze** | Conducted Exploratory Data Analysis (EDA). Identified a strong correlation (0.86) between video views and likes, indicating potential multicollinearity[cite: 1]. Discovered that claim videos have longer transcriptions on average (95 characters) compared to opinions (83 characters)[cite: 2]. |
| **Construct** | Balanced the target variable classes using upsampling and used `CountVectorizer` to extract numerical features (n-grams) from video text[cite: 2]. Built a baseline Logistic Regression model[cite: 1], followed by Random Forest and XGBoost classifiers[cite: 2]. |
| **Execute** | Evaluated models on a validation set and holdout test set[cite: 1, 2]. Extracted feature importances to determine the key drivers of the model's decisions[cite: 2]. |

---

## 📈 Model Comparison: Why Classification Trees Won

We trained two primary types of models across the project notebooks. Here is how the advanced Classification Tree models drastically improved upon the baseline Logistic Regression.

### 1. The Baseline: Logistic Regression
*   **Goal:** Predict user `verified_status` to understand underlying video characteristics[cite: 1].
*   **Challenge:** Logistic regression assumes no severe multicollinearity[cite: 1]. To meet this assumption, `video_like_count` had to be excluded because it was too highly correlated with view counts[cite: 1], resulting in lost data.
*   **Result:** Moderate predictive power (65% Accuracy, 84% Recall)[cite: 1].

### 2. The Champion: Random Forest Classification Tree
*   **Goal:** Direct prediction of `claim_status` (Claim vs. Opinion)[cite: 2].
*   **The Improvement:** Tree-based models are naturally robust to outliers and multicollinearity[cite: 2]. This allowed us to feed *all* engagement metrics (including likes) into the model without penalty. 
*   **Result:** Exceptional performance with **~99.5% Recall** and nearly **100% Accuracy**[cite: 2].

| Model Type | Accuracy | Recall | Key Advantage / Disadvantage |
| :--- | :--- | :--- | :--- |
| **Logistic Regression** | 65.0% | 84.0% | Required dropping highly correlated features[cite: 1]. |
| **XGBoost** | ~99.0% | ~99.0% | Highly accurate, but errors leaned slightly toward false negatives[cite: 2]. |
| **Random Forest** | **~100%** | **~99.5%** | **Champion Model.** Handled all features; caught almost all claims[cite: 2]. |

---

## 💡 Key Business Insights

1.  **Engagement Drives Classification:** The most predictive features in determining if a video is a claim were user engagement levels[cite: 2]. The AI relies heavily on how many views, likes, shares, and downloads a video receives to make its prediction[cite: 2].
2.  **Transcription Length Matters:** While not the top predictor, textual analysis confirmed that videos presenting claims tend to have longer transcriptions (about 13 more characters on average) than opinion videos[cite: 2].
3.  **Ready for Production:** The Random Forest model is highly reliable. It confidently captures almost all claims, ensuring that potentially harmful content is consistently routed to human moderation teams without flooding them with benign opinions[cite: 2].
