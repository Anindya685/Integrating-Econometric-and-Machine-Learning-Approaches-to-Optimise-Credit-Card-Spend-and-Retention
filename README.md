# Integrating Econometric and Machine-Learning Approaches to Optimise Credit-Card Spend and Retention

## Overview
This project leverages an integrated econometric and machine learning approach to analyze a dataset of 10,127 US credit card accounts. My primary goal was to uncover actionable behavioral patterns that drive portfolio value, focusing on optimizing credit card spend and improving customer retention. Through rigorous statistical modeling and advanced classification techniques, I've identified key predictors for annual spending and churn risk, enabling the development of data-driven strategies for credit limit management, re-engagement initiatives, and targeted retention efforts.

## Why This Project is Relevant
Credit cards are a dual asset for financial institutions, serving as both a profit powerhouse and a rich source of consumer behavioral data. In today's competitive market, intensified by fintech disruptors and closed-loop payment systems, competitive advantage increasingly hinges on the superior utilization of this first-party data. This project directly addresses this need by transforming standard transaction information into customized, actionable insights that can significantly boost revenue margins and reduce preventable customer churn, ultimately benefiting both the financial institution and its clientele.

## Research Objectives
My analysis aimed to achieve the following objectives:

To quantify the independent effects of credit capacity and engagement recency on annual purchase volume.

To examine how credit utilization affects customer attrition differently across various credit limits, accounting for inactivity periods and transaction patterns.

To maximize the early identification of customers likely to churn by leveraging complex nonlinear patterns and multi-factor interactions across behavioral and demographic variables.

## Methodology
My analytical framework progressed through three progressive stages: descriptive profiling, predictive econometrics, and machine learning classification, ensuring a comprehensive understanding of customer behavior.

## Data Sources and Features
The project utilized a comprehensive Credit-Card Portfolio dataset comprising 10,127 pseudonymized customer records with 23 variables. Key variable categories included:

Demographics: gender, customer_age, education_level

Financial Capacity: credit_limit, total_revolving_balance, avg_open_to_buy

Usage Patterns: total_trans_amt, total_trans_ct, months_inactive_12_mon, contacts_count_12_mon, avg_utilization_ratio

Product Attributes: card_category, premium_card

Performance Metrics: attrition_flag (target variable for churn models)

## Data Pre-processing
While the report doesn't detail a separate "data pre-processing" section, it implicitly covers several aspects within its analysis:

Variable Selection: Features were strategically selected based on their predictive power and strategic relevance to customer management.

Transformation/Handling: Annual spend was likely winsorized (as mentioned in OLS justification) to address outliers and ensure a more symmetric distribution suitable for linear regression. Categorical variables were appropriately handled (e.g., attrition_flag converted to a numeric dummy attrition_flag_num).

Multicollinearity Check: Correlation analysis (heatmap) and Variance Inflation Factors (VIFs < 2.1) were used to ensure predictors were statistically independent, allowing for concurrent inclusion of diverse variables without multicollinearity concerns.

## Data Processing/Analysis/Methods Used
I employed an integrated approach combining econometric and machine learning models:

Model A: Linear Spend Analysis (Ordinary Least Squares - OLS)

Research Focus: Measuring the independent effects of credit capacity (credit_limit) and engagement recency (months_inactive_12_mon) on annual purchase volume.

Methodology: OLS regression was used, justified by the winsorized annual spend showing symmetry, homoscedastic residuals (Breusch–Pagan p = 0.23), and low VIFs.

Model B: Credit Line-Usage Interaction and Customer Attrition (Logistic Regression)

Research Focus: Examining whether a customer's credit utilization affects attrition differently across various credit limits, while accounting for inactivity periods and transaction patterns.

Methodology: Logistic regression was employed, specifically including an interaction term between credit_limit and avg_utilization_ratio to capture the "constraint-release" phenomenon. Model performance was evaluated using AUC and balanced accuracy.

Model C: Random Forest for Early Churn Detection (Classification)

Research Goal: Leveraging complex nonlinear patterns and multi-factor interactions to maximize early identification of customers likely to churn.

Methodology: A Random Forest classifier was trained using five-fold cross-validation. Performance was assessed using Out-of-Bag (OOB) AUC, recall, precision, and variable importance rankings.

## Technologies Used
The project primarily utilized the R programming language and its rich ecosystem of statistical and machine learning libraries.

R: Core language for statistical modeling and analysis.

tidyverse: For data manipulation and visualization.

janitor: For data cleaning.

broom: For tidying model outputs.

car: For regression diagnostics (e.g., VIFs, Breusch–Pagan test).

corrplot: For correlation heatmaps.

ggplot2: For creating various plots and visualizations (histograms, boxplots, scatter plots, ROC curves, decision trees).

gridExtra: For arranging multiple plots.

randomForest: For implementing the Random Forest classifier.

caret: For machine learning workflow (e.g., cross-validation, confusion matrix).

pROC: For ROC curve analysis.

rpart and rpart.plot: For decision tree visualization.

scales and RColorBrewer: For plot aesthetics.

## Key Findings
Model A: Linear Spend Analysis
Credit Limit Impact: Each additional $1,000 in credit limit generates approximately $42 in additional annual spend (p < .001).

Inactivity Impact: Each inactive month reduces annual spend by $103 (p = .002).

The model accounts for 67% of spend variance when behavioral variables are included, confirming capacity and engagement as core spending drivers.

Model B: Credit Line-Usage Interaction and Customer Attrition
A significant negative interaction between credit limit and utilization was found (β = –0.00097, p < .001).

"Constraint-Release" Threshold: For customers with high limits (e.g., $10,000), increasing utilization from 30% to 50% cuts attrition risk by half. However, for low-limit customers (e.g., $2,000), the same utilization increase actually boosts attrition risk by 18%.

The model achieved strong performance with AUC = 0.869 and balanced accuracy = 0.78, confirming it is well-calibrated (Hosmer Lemeshow p = .31).

Model C: Random Forest for Early Churn Detection
The Random Forest model achieved excellent performance with OOB AUC = 1.00, recall = 1.00, and precision = 0.91 (at a 0.20 churn-probability cut-off).

Key Predictors: transaction count, transaction amount, and months inactive were identified as the strongest signals for churn.

Business Value: By contacting only the top 10% of customers flagged by the model, the bank could reach over 60% of all future churners, tripling the efficiency of a random campaign.

## Recommendations
The integrated "size–timing–target" decision framework derived from these models offers a powerful playbook for optimizing credit card portfolio value:

Precision Credit Policy:

Growth Tier: For customers with limits ≥$3,000 and <80% utilization, strategically increase limits by $3,000. This is projected to generate an additional $126 in annual spend with minimal risk increase.

Relief Tier: For customers with limits <$3,000 and >80% utilization, offer balance transfers or installment plans instead of simple limit increases, as higher limits in this segment can accelerate churn.

Early Disengagement Intervention:

Deploy personalized 30-day dormancy triggers (e.g., combining account summaries with time-limited cashback incentives) for inactive customers. This is justified by the finding that one month of inactivity costs $103 in spend and increases churn odds by 60%.

Premium Segment Retention:

Replace universal fee waivers with targeted value enhancement. Waive fees only for inactive (>1 month) or low-utilization (<20%) premium customers.

Offer experiential benefits to high-engagement premium segments to restore perceived value and reciprocity. This addresses the insight that premium cards, while adding spend, also increase churn odds seven-fold.

Gender-Optimized Outreach (Optional/Contextual):

Consider channel differentiation: high-touch outreach for high-risk women, and low-cost digital engagement for men. (Note: The model performs similarly without gender, allowing for a pivot to churn-score targeting if preferred for fairness/compliance reasons).

## Implementation Framework
The proposed framework integrates these models into a real-time decision engine. A 10% holdout pilot is recommended to validate effectiveness (targeting $8/account lift). Governance includes randomized tests, monthly model retraining (monitoring AUC drift), data enrichment, and bias audits. Conservative estimates project $6-8 million in additional annual margin and a 33% reduction in preventable churn without loosening credit-risk tolerance.
