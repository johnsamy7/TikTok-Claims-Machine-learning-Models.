# TikTok Claims Regression Model.

<h3>Overview</h3>

* Context: Building an ML model to classify videos as claims or opinions to reduce report backlogs.

* Objective: Analyzing how video characteristics relate to an author's verification status at the request of Operations Lead Maika Abadi.

* Methodology: Utilized a baseline Logistic Regression model to predict verification status and discover key feature trends.

<h3>project statues</h3>  

* EDA & Cleaning: Capped engagement outliers and addressed a severe $93.7% class imbalance using a 50/50 upsampling technique.

* Feature Engineering: Created a text_length character counter and dropped video_like_count to avoid severe multicollinearity due to its strong 0.86 correlation with video views. 

* Data Split: Divided data into a 75/25 train/test split and applied One-Hot Encoding to categorical features.



<h3>next steps</h3>

* Pipeline Integration: Port video duration weights into the main claim-vs-opinion model to refine classification.

* Model Upgrades: Test advanced algorithms (e.g., Random Forest, XGBoost) to improve precision beyond the current 61% baseline.

* Queue Automation: Deploy a high-recall 84% threshold to automatically filter trusted accounts from urgent moderation queues.

<h3>key insights</h3> 

* Video Duration Matters: Longer videos increase the log-odds of a user being verified 0.0086 coefficient for video_duration_sec).
  Engagement Disconnect: High video view, share, and comment counts show a negligible independent correlation with an author's verification status, proving that viral engagement metrics alone do not inherently
  dictate whether an account gets verified. 

* Text Length Patterns: Unverified accounts write slightly longer video scripts on average 89.4 characters) than verified accounts 84.6 characters.


# TikTok Claims Classification Models

ISSUE / PROBLEM
TikTok receives a high volume of user reports flagging videos as potentially containing claims
Manual review of every report creates a backlog and slows response time
No automated way to separate genuine claims from opinions before a human ever looks at them
RESPONSE
Engineered features from the raw data: text_length from the video transcription, n-gram text features (CountVectorizer, bigrams/trigrams), engagement metrics (views, likes, shares, comments, downloads), video duration, verification status, and author ban status
Split data 60/20/20 (train/validation/test) and used 5-fold cross-validated grid search, optimizing for recall — since missing a real claim is costlier than over-flagging an opinion
Trained and compared two models: Random Forest and XGBoost
Evaluated both on a held-out validation set before selecting a champion model, then confirmed performance on the untouched test set
KEY INSIGHTS
Classes were well balanced (50.3% claim / 49.6% opinion) — no imbalance correction needed
Both models performed exceptionally: Random Forest hit ~99.5% recall / ~99.9% precision in cross-validation; XGBoost was close behind at ~99% recall / ~99.9% precision. Random Forest was selected as champion.
On the test set, Random Forest achieved ~100% precision and recall on both classes
Video engagement metrics were the most predictive features — more than the text content itself — suggesting claims and opinions differ systematically in how they perform, not just in what they say
Claims averaged longer transcriptions than opinions (~95 vs ~83 characters), a smaller but consistent signal

(Worth double-checking the exact feature importance ranking against your bar chart from cell 108 before finalizing this — I can see the chart was generated but can't read the bar values from the file.)

IMPACT
A model at ~99%+ recall and precision can catch nearly all genuine claims automatically
Moderators can skip manual triage on clear-cut cases and focus only on ambiguous ones
Directly reduces the report backlog and speeds up response time — the exact business goal stated at the outset



