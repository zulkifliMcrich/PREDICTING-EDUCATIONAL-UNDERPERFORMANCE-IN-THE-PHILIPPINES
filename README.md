<h1 align="center">Predicting Educational Underperformance in the Philippines</h1>

<p align="center">
  A Machine Learning Approach to NAT 2023–2024 Regional Disparities
</p>

<p align="center">
  Mindanao State University — College of Information and Computing Sciences<br>
  Department of Information Systems
</p>

---

## Overview

Filipino learners consistently fall below national proficiency targets in the National Achievement Test (NAT). In PISA 2022, only 16% of Filipino students reached Level 2 math proficiency compared to the 69% OECD average. Persistent regional inequalities are often masked by aggregate national statistics, making it difficult to target interventions effectively.

This project applies supervised machine learning to school-level NAT 2023–2024 data to predict school performance tiers and identify the subject areas and regions most contributing to educational underperformance — an approach that remains largely unexplored in the Philippine context.

---

## Research Questions

1. What are the current patterns and regional disparities in school performance tiers based on the 2023–2024 NAT?
2. Which subject MPS scores are the strongest predictors of educational underperformance?
3. How accurately can machine learning models (Logistic Regression, Decision Tree, Random Forest) predict school performance tiers?

---

## Dataset

| Detail | Value |
|--------|-------|
| Source | DepEd Open Data Repository — NAT Results SY 2023–2024 |
| Records | 6,636 school-level observations |
| Regions | Region I, Region III, Region VIII, Region IX, CAR |
| School Divisions | 44 |
| Train / Test Split | 80/20 stratified — 5,308 training / 1,328 testing |

**Key Variables:** Subject MPS (Filipino, Mathematics, English, Science, Araling Panlipunan), number of test takers, regional and division identifiers

**Target Variable:** Performance Tier derived from DepEd's MPS framework

| Tier | MPS Range |
|------|-----------|
| Poor | 0 – 25% |
| Lower Average | 26 – 50% |
| Upper Average | 51 – 75% |
| Superior | 76 – 100% |

---

## Methodology

### Exploratory Data Analysis
- Descriptive statistics and visualizations of subject MPS distributions
- Performance tier distribution analysis
- Regional mean MPS comparison
- Pearson Correlation analysis between each subject MPS and overall performance tier
- All five subjects showed strong upward linear trends (r = 0.747–0.814), with Mathematics recording the highest correlation (r = 0.814, p < 0.001)

### Data Preprocessing
- Removed non-predictive columns: `school_id` and `overall_mps` (to prevent data leakage)
- Corrected UTF-8 encoding error in "Science City of Muñoz"
- Applied one-hot encoding to region and division variables → expanded to 54 input features
- CAR treated as implicit reference/baseline category
- 693 outlier rows in `n_test_takers` retained as valid large-school observations
- No feature scaling applied (to preserve coefficient interpretability)

### Models Used

| Model | Reason for Selection |
|-------|---------------------|
| Logistic Regression | Interpretable baseline; shows coefficient direction and strength |
| Decision Tree | Reveals explicit score thresholds; explainable to stakeholders |
| Random Forest | High accuracy, robust to noise, generates feature importance scores |

---

## Results

### Model Performance (Test Set)

| Model | Accuracy | Precision | Recall | F1-Score | Cohen's Kappa |
|-------|----------|-----------|--------|----------|---------------|
| Decision Tree | 1.0000 | 1.0000 | 1.0000 | 1.0000 | 1.0000 |
| Logistic Regression | 1.0000 | 1.0000 | 1.0000 | 1.0000 | 1.0000 |
| Random Forest | 0.9992 | 0.9993 | 0.9167 | 0.9496 | 0.9987 |

- Decision Tree and Logistic Regression achieved perfect classification on all metrics
- Random Forest had only **1 misclassification** (a Poor-tier school predicted as Lower Average) out of 1,328 test records
- High accuracy reflects the near-deterministic relationship between subject MPS and DepEd-defined tier thresholds

### Feature Importance (Random Forest)

| Feature | Importance Score |
|---------|-----------------|
| Mathematics MPS | 0.28 |
| English MPS | 0.22 |
| Science MPS | 0.15 |
| Araling Panlipunan MPS | 0.11 |
| Filipino MPS | 0.09 |
| Number of Test Takers | 0.05 |
| Region Features | 0.04 |

Mathematics, English, and Science collectively account for **65% of total predictive weight.**

---

## Key Insights

- **Underperformance is systemic, not subject-isolated** — cross-subject interventions are more effective than single-subject fixes (Math–Science inter-subject correlation: r = 0.781)
- **Region IX** is a priority region requiring urgent policy attention and resource reallocation (mean MPS: 53.79% vs. Region VIII's 66.32%)
- The **15 schools in the Poor tier**, while numerically small, represent severely at-risk institutions needing immediate targeted support
- The ML framework can serve as a **decision-support tool** for DepEd analysts to monitor and flag at-risk schools in real time

### Policy Implications
- DepEd should prioritize Mathematics and Science instruction quality in low-performing regions
- Regional disparities signal a need for geographically targeted funding and teacher deployment
- Data-driven evidence can strengthen DepEd's public accountability mandate under DepEd Order No. 55, s. 2016

---

## Limitations

- Dataset covers only 5 of the Philippines' 17 regions — findings are not nationally generalizable
- Near-perfect accuracy is partly due to the circular relationship between predictors (subject MPS) and the target (performance tier derived from the same MPS values)
- School-level data excludes individual-level factors such as socioeconomic status, teacher quality, and classroom resources
- Severe class imbalance in the Poor tier (only 15 schools total; 3 in test set) — per-class metrics for this tier are statistically unreliable

---

## Future Work

- Expand dataset to all Philippine regions and include multiple school years for longitudinal analysis
- Integrate contextual variables: teacher-to-student ratios, school funding, and poverty indices
- Apply SMOTE or other oversampling techniques to address class imbalance in the Poor tier
- Deploy the best-performing model as an interactive DepEd decision-support dashboard for real-time school monitoring and intervention planning

---

## Authors

| Name | Institution |
|------|-------------|
| Mohammad Zulkifli S. Macadato | Mindanao State University — BSIS |
| Amber Miguel B. Malinis | Mindanao State University — BSIS |

---

<p align="center">
  Mindanao State University — College of Information and Computing Sciences<br>
  Department of Information Systems
</p>
