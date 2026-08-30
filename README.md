1. Title + Abstract (and Introduction)

Prioritizing SEO Content Refreshes: A Predictive Ranking Model for Action Queues
Abstract
Content teams often struggle to prioritize which aging articles to refresh under strict review budgets. This project develops a machine learning ranking model to identify high-potential declining content. Using a robust client-grouped validation split to prevent data leakage, a Random Forest Regressor was trained on historical SEO metadata (e.g., impressions, average position). The model successfully outperformed a naive baseline, effectively ranking pages that require immediate attention. This playbook serves as a directional, decision-support tool rather than an automated publishing system.

Introduction / Problem Statement
Organic search traffic decays over time. When a team can only manually review a limited number of pages (e.g., 50 pages a month), the order of the review queue is critical. A model that nobody acts on is just a science project; therefore, the goal of this research is to build a ranked action queue that maximizes the impact of human editorial time.

2. Data

Data
The model was built using an anonymized subset of real production search data.

Included Features: Safe, historical metrics such as search_volume, competition, avg_position, and content_age_days.

Exclusions for Public Safety & Leakage: Client names, URLs, and private queries were strictly excluded. Furthermore, downstream metrics like sessions_90d and pageviews_90d were dropped from the feature set to prevent target leakage, as they occur alongside or after the target variable (clicks_90d).

3. Methodology

Methodology

Algorithm: Random Forest Regressor was chosen for its robustness against non-linear relationships.

Validation Design: A naive random split (e.g., train_test_split) yielded artificially optimistic results due to client-data memorization. To ensure an honest evaluation, a GroupShuffleSplit on client_id was strictly enforced. This guarantees the model is evaluated on its ability to generalize to unseen clients.

Baseline: The model was evaluated against a simple Mean Predictor (Dummy Regressor) on the exact same grouped split.

4. Results (vs baseline)

Results
Under the strict client-grouped split, the Random Forest model achieved a lower Mean Absolute Error (MAE) compared to the naive mean baseline. While a random split previously showed a near-perfect (but misleading) score, the grouped split provided a realistic, measured expectation of performance on new data.
(Note: You can insert your generated matplotlib charts or tables here if you want to display the visual difference).

5. Limitations & Honest Framing

Limitations

Predictive, Not Causal: This model observes correlations between metrics (like position and clicks) but does not prove that updating a specific feature will cause a traffic recovery.

Time-Window Limits: The evaluation was conducted on a single time-window snapshot. Continuous monitoring and retraining are required to account for major Google Algorithm updates.

No Content Context: The model reads metadata, not the actual written text. It cannot judge brand safety or tone.

6. Ranked Recommendations (Action Playbook)

Ranked Recommendations
The model's output is transformed into a human-in-the-loop playbook with the following ranked reason codes:

REFRESH_HIGH_POTENTIAL: High search volume, poor position, content older than 365 days. Action: Send to editor for rewrite.

MONITOR_DECAY: Content aging past 180 days with slight dips. Action: Hold and observe next month's queue.

Guardrails: Automated publishing is strictly prohibited. URLs cannot be redirected without human SEO expert approval.

7. Reproducibility & Acknowledgments

Reproducibility
All code, methodology, and exported figures are available in the repository. The environment, random seeds (random_state=42), and validation splits are fully documented to allow exact replication of the metrics.

Repository Link: https://github.com/mohamed0449/flyrank-ml-internship

Acknowledgments & Data Credit
This research and model were built on the FlyRank ML Internship dataset. Special thanks to the FlyRank team for the mentorship.
Dataset and program details: https://flyrank.ai
