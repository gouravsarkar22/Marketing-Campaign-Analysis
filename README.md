# Marketing-Campaign-Analysis
A/B Testing &amp; Regression


# Project Overview

This project analyzes a large-scale marketing campaign dataset (200,000+ rows) to uncover insights about campaign effectiveness and conversion drivers.

It applies A/B testing to measure the performance difference between marketing channels (e.g., Email vs Google Ads), and regression modeling to predict key business outcomes like Conversion Rate and ROI.

The goal is to demonstrate practical data-driven decision making and statistical thinking — essential for data analyst and data science roles.


# Business Problem

Marketing teams often spend across multiple channels (Email, Social Media, YouTube, etc.) without clear evidence of which channel yields the best conversion.
This project aims to answer:

1. Are some channels performing significantly better in terms of conversion rate?

2. Which campaign attributes (cost, engagement, duration, etc.) best predict conversion performance?


# Objectives

1. Perform A/B Testing
   1. Compare top-performing channels and test if differences in conversion are statistically significant.
   2.  Use z-test confidence intervals for robust inference.

2. Build Regression Models
   1. Predict Conversion_Rate using campaign metrics and categorical variables.
   2.  Compare Linear Regression, Ridge Regression, and Lasso Regression.
   3.  Interpret results through feature importance analysis and business insights.
  

# Dataset Description

Dataset size: 200,000 rows × 16 columns

Source:  Kaggle

Feature	Description

Campaign_ID -	Unique campaign identifier

Campaign_Type	- Email, Influencer, Display, etc.

Channel_Used	- Marketing channel (Google Ads, Email, YouTube, etc.)

Conversion_Rate -	Campaign conversion fraction

ROI	- Return on Investment

Impressions, Clicks	Marketing engagement metrics

Engagement_Score - Internal engagement rating (1–10)

Acquisition_Cost - Campaign cost

Customer_Segment, Target_Audience	Categorical campaign attributes


# Data Preparation (Python + Pandas)

Cleaned columns and standardized naming conventions.

Converted monetary and duration fields to numeric values.

Derived new metrics:

Estimated Conversions: Conversion_Rate × Impressions

Converted_any: Binary conversion flag.

DurationDays: Extracted numeric campaign duration.

Aggregated KPIs by channel for A/B analysis and regression modeling.

# A/B Testing Analysis

*Goal:* Compare two channels — e.g., Google Ads (Group A) vs Email (Group B)

*Statistical Method:*

Two-proportion z-test for conversion rates

Hypotheses: 

H0: pA = pB

H1: pA ≠ pB

Email: conversions=14796870, impressions=184801107, conv_rate=0.0801

Google Ads: conversions=14804539, impressions=185006879, conv_rate=0.0800

Absolute difference in conversion rates: 0.0000

Pooled conversion rate: 0.0800

Standard Error: 0.000028

Z-test results:

z = 1.687, 

two-sided p-value = 0.0917

P-value (0.0917): This is the probability of observing a test statistic as extreme as, or more extreme than, the one calculated from your sample data, assuming the null hypothesis is true.

Significance Level (often 0.05): This is the threshold set before the test is conducted to decide whether to reject the null hypothesis. It is the maximum probability of making a Type I error (rejecting a true null hypothesis) that you are willing to accept. 

If p-value (p < 0.05) , the result is statistically significant, and you reject the null hypothesis.

If p-value (p >= 0.05) , the result is not statistically significant, and you fail to reject the null hypothesis. 


# Results Interpretation

1. The p-value = 0.0917 > 0.05 → Fail to reject the null hypothesis.

2. The observed difference between Email (8.01%) and Google Ads (8.00%) is not statistically significant at 95% confidence.

Although Email shows a slightly higher conversion rate (by 0.01 pp), the effect size is extremely small relative to the sample variance.

Conclusion: Both channels perform equivalently in conversion efficiency; further testing with a refined target segment or creative variation is recommended.

# Business Insight

Practical Impact: A 0.01 percentage point lift corresponds to roughly 100 additional conversions per million impressions — statistically negligible at current spend levels.

Recommendation: Keep both channels active but shift focus to optimizing cost per acquisition (CPA) and ROI rather than raw conversion rate.

Next Step: Run a segmented A/B test (e.g., by age group, region, or audience type) to detect micro-level differences that aggregate data may hide.

<img width="889" height="390" alt="image" src="https://github.com/user-attachments/assets/967aedb6-2c2f-4210-9876-5d6986f69fa6" />



# Regression Analysis

To understand the underlying drivers of conversion, two models were built:

Model	Target: Linear Regression	

Conversion Rate (continuous)	

Metric: RMSE = 0.0016, R² = -0.0002	

Performance: Low explanatory power — conversions likely driven by non-linear or unobserved factors

Baseline (mean) RMSE: 0.001660

Linear model vs baseline: essentially identical RMSE; model does not improve materially beyond the mean predictor.

Additional regressions: Ridge & Lasso tested — R² near 0 and negative (no uplift)

<img width="854" height="547" alt="image" src="https://github.com/user-attachments/assets/23a60b3d-ec24-400c-9914-19537fe85ce7" />



*Interpretation:* RMSE is very small (because conversion rates are small and data is large), but a negative or near-zero R² indicates the model explains effectively no additional variance over the mean. This suggests either (a) features used are weak predictors for conversion rate at campaign granularity, or (b) the signal is overwhelmed by noise/aggregation.

# Model diagnostics

Before reset: X_test.shape = (40020, 8) y_test.shape = (40020,)

After reset: X_test.shape = (40020, 8) y_test.shape = (40020,)

Linear model: RMSE ≈ 0.001660, R² ≈ -0.000142, MAE ≈ 0.035148

Ridge / Lasso: R² values near zero/negative (no improvement)

Baseline mean prediction performs on par with linear models.

<img width="560" height="435" alt="image" src="https://github.com/user-attachments/assets/be834bdd-d1ab-4a81-9c12-d50237ad1d4b" />



# Key takeaways 

A/B result (primary business conclusion): No statistically significant difference in conversion rates between Email and Google Ads at the 95% confidence level (p = 0.0917). At scale, the channels are functionally equivalent on conversion rate.

Model conclusion: Predictive models trained on campaign-level features did not substantially outperform simple baselines for conversion rate — indicating that campaign-level aggregation may hide user-level signals required for strong prediction.


<img width="700" height="547" alt="image" src="https://github.com/user-attachments/assets/f111bed9-1087-4e22-8fcf-d3c1ce081d25" />



# Dashboards

<img width="853" height="476" alt="Screenshot 2025-11-10 234507" src="https://github.com/user-attachments/assets/5a0c6341-7221-4da0-83a4-599308a3f5aa" />











<img width="852" height="482" alt="Screenshot 2025-11-10 235258" src="https://github.com/user-attachments/assets/a5690a57-e762-423a-af8b-e28e19f8433a" />






# Business recommendations

1. Do not reallocate spend solely on these conversion rates. The observed difference is tiny and not statistically significant.

2. Focus on ROI/CPA rather than raw conversion rate — if Google Ads or Email have different acquisition costs, the preferred channel may change after cost adjustment.

3. Run targeted segmented tests (by audience segment, device, creative) to detect heterogenous effects that aggregate averages mask.

4. Collect event-level data (per-user conversions) and use randomized assignment in production for rigorous causal inference.

5. Improve modeling features: include user-level features, recency/frequency, campaign creative metadata, or time-to-convert to increase predictive power.


# Tech Stack

Language: Python (v3.10+)

Libraries: pandas, numpy, scipy, scikit-learn, matplotlib, seaborn

Modeling: Linear, Ridge Lasso Regression, Logistic Regression

Statistical Tests: z-test

Visualization: Seaborn & Matplotlib

Dashboard: Power BI 

# Author

 Gourav Sarkar
 
 Email: gouravsarkar222000@gmail.com
