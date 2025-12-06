# A/B Test Results Analysis

# Evaluating User Conversion Outcomes for an E-Commerce Website

**Project Overview:**

This project analyzes the results of an A/B test conducted by an e-commerce company that aimed to increase customer conversions through a redesigned landing page. The business question is straightforward:

Should the company implement the new page, keep the old one, or continue running the experiment?

To answer this, we apply statistical analysis, probability testing, and practical decision-making frameworks commonly used in product analytics and growth experimentation.

**Objectives:**

•	Assess the effectiveness of the new landing page vs. the old page.
•	Determine whether any observed difference in conversion rates is statistically significant.
•	Provide a recommendation backed by data:
•	Adopt the new page
•	Retain the old page
•	Run the experiment longer

**Dataset Description:**

Two datasets are used:

1. ab_data.csv contains the A/B test event log:
•	user_id
•	timestamp
•	group (control or treatment)
•	landing_page (old_page or new_page)
•	converted (1 or 0)

2. countries.csv contains:
•	user_id
•	country (CA, UK, US)

These datasets are merged to analyze whether **country-specific behavior** affects conversion.


**Key Steps in the Analysis:**

**1. Data Exploration**
	•	Inspect dataset structure
	•	Validate experiment randomization
	•	Check for inconsistencies or sampling issues

**2. Probability & Statistical Tests**
	•	Compute conversion rates for both groups
	•	Perform hypothesis testing, typically:
	•	Z-test for difference in proportions
	•	Confidence interval estimation
	•	Examine p-values and statistical significance

**3. Practical & Business Interpretation**
	•	Assess whether the statistical results translate into real business value
	•	Consider effect size, risk, and uncertainty
	•	Recommend final action to stakeholders

**4. Tools & Technologies**
	•	Python (Pandas, NumPy, SciPy)
	•	Statistical Testing (Z-tests, bootstrapping, probability modeling)
	•	Data Visualization (Matplotlib, Seaborn)
	•	Jupyter Notebook

This project delivers:
•	A clear comparison of conversion performance between the A and B groups
•	Evidence-based conclusions about the impact of the new design
•	Actionable recommendations grounded in statistical rigor

**Part I — Data Cleaning & Probability**

Key cleaning steps:
•	Removed rows where group and landing_page did not match: control ≠ new_page AND treatment ≠ old_page
•	Removed duplicate user IDs to avoid counting the same user twice.
•	Verified that the dataset contained no missing values.

Metrics:
Overall conversion rate = 11.96%
Control (old page) conversion = 12.04%
Treatment (new page) conversion = 11.88%
Probability of receiving new page = 50%

The new page **converts slightly less often** than the old page, but the difference is small.

**Part II — A/B Testing**

1. Hypotheses:

• We test whether the new page converts better than the old page

• difference: pnew - pold = -0.00158. The difference is slightly negative.

2. Null Simulation (10,000 iterations):

• We simulated a sampling distribution assuming both pages convert at the same rate.
• The observed difference fell near the center of this null distribution.
• Simulated p-value ≈ 0.90

3. Z-test

Using a two-sample z-test:
•	z ≈ 1.31
•	p ≈ 0.905
•	Critical z (α = 0.05) = 1.645

Conclusion from A/B test
•	p-value ≫ 0.05
•	No statistical evidence that the new page performs better
•	The observed difference is fully consistent with random noise

The **A/B test** suggests the **new page should not replace the old page**.

**Part III — Regression Modeling**

We fit several logistic regression models with:
•	converted as the dependent variable
•	Predictors: page type, country, interactions

1. Model 1 — Page only
•	ab_page coefficient ≈ –0.015
•	p ≈ 0.19 (not significant)
•	Odds ratio ≈ 0.985

Users on the new page have 1.5% lower odds of converting, but this is not statistically significant.

2. Model 2 — Page + Country

Added dummy variables: US, UK (baseline = CA)
•	US p-value ≈ 0.13
•	UK p-value ≈ 0.07
•	ab_page still not significant

Country differences exist numerically but are not reliable enough to be considered statistically significant.

3. Model 3 — Interactions (Page × Country)

Added:
•	US_ab_page
•	UK_ab_page

All interaction terms had p-values > 0.16
Pseudo R² ≈ 0.000

Neither country nor page nor their interaction explain conversion outcome.

**Results Summary**

Across all methods:

A/B Test
•	New page conversion slightly lower than old page
•	p-value ≈ 0.90 → no statistical evidence of improvement

Z-test
•	z = 1.31, p = 0.905
•	Fail to reject the null

Logistic Regression
•	ab_page not significant (p ≈ 0.19)
•	Adding country predictors did not change results
•	No significant interaction between page and country

**Business Recommendation**

Do NOT launch the new page.

Based on all evidence:
•	The new page does not improve conversions
•	May slightly reduce conversions
•	No country segment benefits meaningfully from the new page
•	Statistical tests strongly indicate maintaining the old page

**Recommended action:**

- Continue using the old page
- Consider A/B testing alternative designs
  Run longer tests or segment by user behavior, device, or session duration 
