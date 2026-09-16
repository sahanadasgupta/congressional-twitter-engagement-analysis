# congressional-twitter-engagement-analysis
# Congressional Twitter Engagement Analysis

Statistical modeling and text analysis of congressional Twitter activity, built to answer a practical question for a political advocacy use case: **what actually drives retweets and likes on political accounts, and how should a team schedule and target its posts?**

## Overview

This project analyzes a merged dataset of **548 congressional Twitter profiles** and **880,000+ tweets** to test three hypotheses about what drives engagement, then translates the results into concrete posting-strategy recommendations for a simulated client (Lobbyists4America).

## Data & Cleaning

- Merged `users_df` (548 profiles) and `tweets_df` (880,057 tweets) via a one-to-many relational join on user ID, using SQL for filtering, joins, and row selection.
- Standardized a `created_at` column containing a mix of Unix epoch timestamps and string-formatted dates using a two-step parsing function.
- Handled geolocation fields with 98%+ missing values, and used non-parametric statistics (medians, IQR, Spearman's rho) to manage heavily skewed distributions elsewhere in the data.

## Hypotheses & Methods

| # | Hypothesis | Method | Result |
|---|---|---|---|
| H1 | Tweets with media earn significantly higher retweets/favorites | Welch's t-test | **Not significant** (t = 0.2596, p = 0.7952) — media tweets had a higher sample mean (85.10 vs. 59.21 retweets), but high variance meant the difference wasn't statistically reliable |
| H2 | Tweeting during peak platform volume yields higher per-tweet engagement | One-way ANOVA | **Significant, but disproved the hypothesis** (F = 15.33, p = 9.67×10⁻⁶¹) — peak posting hours (14:00–20:00 UTC) actually showed *diluted* engagement due to feed saturation; off-peak hours performed better |
| H3 | Very high follower counts drive disproportionately high engagement | Spearman correlation | **Significant** (ρ = 0.2958, p < 0.0001) — engagement scales non-linearly, with a tipping point where only the top 25% ("Very High" tier) of accounts earn exponentially higher likes (173.33 avg) |

Also tested: verified vs. unverified accounts, which showed a substantial engagement gap (105.73 mean retweets for verified vs. 67.22 for unverified).

## Text Analysis

Applied **TF-IDF vectorization** across the tweet corpus to surface dominant themes. Top terms clustered into two categories:
- **Policy & economy** language (house, act, jobs, work)
- **Constituent engagement** language (today, thanks, support)

## Custom Metrics

Built two engagement metrics not present in the raw data, to normalize for account size:
- **Engagement Rate** = (retweets + favorites) / (followers + 1) — surfaces efficient "micro-influencer" accounts regardless of raw follower count (dataset average: 0.37%)
- **Amplification Ratio** = retweets / (favorites + 1) — measures how far a tweet travels outside an account's existing audience (dataset median: 1.0)

## Recommendations

Translating the statistical findings into strategy:
1. **Shift high-priority posts to low-volume windows** (02:00–07:00 UTC) to avoid feed competition during peak hours.
2. **Prioritize verified, "Very High" follower-tier accounts** for major campaigns, since engagement scales non-linearly at the top of the distribution.
3. **Track engagement-per-follower to identify micro-influencer accounts** for partnership — these accounts outperform their raw follower count would suggest.

## Tools

Python (pandas, numpy, matplotlib, scipy), SQL, TF-IDF vectorization, Welch's t-test, one-way ANOVA, Spearman correlation.

## Files

- `[notebook filename].ipynb` — full analysis notebook
- `poster.pdf` — research poster summarizing methodology and findings
