# Rideshare Demand, Experimentation & Rider Retention

## Overview

This project analyzes customer demand and retention for a simulated rideshare company. It combines three related business problems:

1. Identifying recurring login-demand patterns to support marketplace planning.
2. Designing an experiment to evaluate whether reimbursing bridge tolls encourages drivers to serve two neighboring cities.
3. Predicting whether a rider will remain active six months after signup and translating the results into retention recommendations.

The project demonstrates exploratory data analysis, customer segmentation, experiment design, feature engineering, classification modeling, model evaluation, and business communication.

## Business Questions

- When does customer demand peak, and how does it change throughout the week?
- Would reimbursing tolls increase the share of drivers serving both cities?
- What percentage of riders remain active after six months?
- Which early behaviors and customer characteristics are most associated with retention?
- How can the company use these findings to improve driver coverage and rider retention?

## Project at a Glance

| Component | Methods | Business Use |
|---|---|---|
| Demand analysis | 15-minute aggregation, time-series exploration, weekday and daypart segmentation | Anticipate periods of high and low marketplace demand |
| Experiment design | Success-metric definition, treatment/control design, hypothesis testing, guardrails | Evaluate a toll-reimbursement incentive |
| Retention analysis | Data cleaning, segmentation, feature engineering, logistic regression, random forest | Identify riders at risk of becoming inactive |

## Key Findings

### Demand Patterns

- Demand was strongest during late-night hours, particularly between **10:00 p.m. and 11:00 p.m.**, with another peak around lunchtime.
- Demand was weakest during the morning period from approximately **5:30 a.m. to 10:00 a.m.**
- Weekends generated the most demand, led by Saturday and Sunday.
- Overnight demand was concentrated on weekends, while after-work demand increased through the week and peaked on Friday.

These patterns could inform driver-incentive timing, staffing decisions, and marketplace monitoring.

### Rider Retention

- Approximately **37.6% of riders were active** at the end of the observation period; **62.4% were inactive**.
- Trips completed during the first 30 days had the strongest observed relationship with longer-term activity.
- Riders completing more than 10 trips in their first month were substantially more likely to remain active.
- Riders from King's Landing, iPhone users, and Ultimate Black users showed higher observed retention rates.

These results are associations rather than proof of causation, but they identify useful segments and behaviors for further testing.

### Predictive Modeling

Logistic-regression and random-forest classifiers were compared. A class-weighted random forest provided the most useful balance on the original holdout set:

| Metric | Result |
|---|---:|
| Test accuracy | 78.0% |
| Active-rider precision | 70% |
| Active-rider recall | 75% |
| Active-rider F1 score | 72% |

Recall is particularly important in this use case because missing a rider who could have been retained may mean losing an opportunity for proactive engagement.

## Experiment Design: Toll Reimbursement

The primary success metric is the **weekly proportion of eligible drivers who complete trips in both cities**. This directly measures whether the policy changes cross-city driver behavior.

An improved experimental design would:

1. Randomly assign eligible drivers to a toll-reimbursement treatment group or a business-as-usual control group.
2. Stratify assignment by home city and baseline trip volume to improve comparability.
3. Run the experiment across multiple complete weekly cycles, with the duration and sample size determined through a power analysis.
4. Compare the proportion of cross-city drivers using a two-proportion test or chi-square test and report the effect size and confidence interval.
5. Evaluate profitability and operational guardrails before recommending expansion.

Recommended guardrails include:

- Net revenue after reimbursements
- Trips completed per active driver
- Driver earnings
- Rider wait time
- Cancellation rate
- Total reimbursement cost

Potential complications include driver spillover between groups, seasonal demand, special events, construction, and changes in the underlying mix of riders or drivers.

## Business Recommendations

- Target onboarding and lifecycle campaigns toward riders who have not established frequent usage during their first 30 days.
- Test incentives that encourage riders to complete an early series of trips rather than assuming the observed relationship is causal.
- Investigate why retention differs by city and device before targeting those attributes directly.
- Use the retention model as a prioritization tool for outreach, with campaign performance measured through controlled experiments.
- Expand toll reimbursement only if it produces a meaningful increase in cross-city coverage while maintaining positive unit economics.

## Methodology

1. Parsed and validated timestamp and rider-level data.
2. Aggregated login activity into 15-minute intervals.
3. Compared demand by time of day, day of week, and daypart.
4. Defined six-month rider activity from the most recent trip date.
5. Cleaned missing values, duplicates, data types, and extreme observations.
6. Encoded categorical features and used robust scaling for skewed numeric features.
7. Compared logistic-regression and random-forest classifiers.
8. Evaluated accuracy, precision, recall, F1 score, overfitting, and business usability.

## Limitations

- The data is simulated and covers a limited observation period.
- Retention drivers are observational associations and should be validated through experiments.
- A random train/test split does not test how the model would generalize to a future customer cohort.
- The notebook also explores SMOTE, but oversampling was performed before a later train/test split. Those results are excluded from the headline model comparison because synthetic observations could enter the holdout set. In a production workflow, resampling should occur only within the training folds of a pipeline.
- Additional behavioral variables, acquisition channels, incentive history, service quality, and pricing data could improve the retention analysis.

## Repository Contents

| File | Description |
|---|---|
| [`Ulimate_challenge_logins.ipynb`](Ulimate_challenge_logins.ipynb) | Login-demand aggregation and exploratory analysis |
| [`Ultimate Case Study - Part 2 Experiment Design.pdf`](Ultimate%20Case%20Study%20-%20Part%202%20Experiment%20Design.pdf) | Original toll-reimbursement experiment response |
| [`Ultimate_case_predictive_modeling.ipynb`](Ultimate_case_predictive_modeling.ipynb) | Rider-retention analysis and classification modeling |

The source datasets are intentionally excluded. The notebooks retain the analytical outputs but require the original local data to rerun.

## Tools

Python, Pandas, NumPy, Matplotlib, Seaborn, scikit-learn, imbalanced-learn, exploratory data analysis, experiment design, and classification modeling.
