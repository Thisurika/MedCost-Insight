
<div align="center">

# 🏥 MedCost Insight
### Medical Insurance Cost Analysis Using Statistical Modelling

**IT3081 – Statistical Modelling | Academic Consultancy Project**

*From simulated healthcare expenditure data to transparent, evidence-based insurance decision support.*

![Status](https://img.shields.io/badge/Status-Academic%20Prototype-blue)
![Data](https://img.shields.io/badge/Dataset-Synthetic-orange)
![Best Model](https://img.shields.io/badge/Selected%20Model-LASSO-teal)
![Language](https://img.shields.io/badge/Statistical%20Analysis-R-276DC3?logo=r&logoColor=white)

</div>

---

## 📌 Project Overview

**MedCost Insight** is an academic statistical consultancy project that examines factors associated with annual medical expenditure and evaluates methods for cost prediction and decision support.

Using **7,500 calibrated synthetic records** representing a Sri Lankan insurance-cost scenario, the project explores healthcare expenditure patterns, tests group differences, compares regression models, and evaluates PCA, Bayesian analysis, experimental designs, and the feasibility of time-series forecasting.

The proposed MedCost Insight dashboard is intended to support **human analysts** with understandable results, warnings, and comparisons—not automate insurance premiums, eligibility, or coverage decisions.

> [!IMPORTANT]
> All main modelling outcomes are based on **simulated data**. Reported results do **not** establish actual Sri Lankan healthcare costs, real insurer-paid claims, future-year performance, causal effects, or production readiness.

## 🎯 Objectives

- Explore annual medical expenditure distributions and potential cost-associated factors.
- Apply statistical hypothesis testing to compare population groups in the simulated data.
- Compare interpretable regression methods for annual expenditure estimation.
- Assess whether PCA improves prediction while reducing dimensionality.
- Use Bayesian methods to study coefficient and prediction uncertainty.
- Evaluate a Naïve Bayes classifier for identifying high-cost cases.
- Examine experimental design and future time-series forecasting requirements.
- Translate statistical findings into responsible business recommendations.

## 📊 Dataset

| Property | Description |
|---|---|
| Main dataset | Calibrated synthetic Sri Lankan medical expenditure data |
| Records | 7,500 |
| Original variables | 22 |
| Prepared columns | 24 |
| Encoded columns used in Task 07 PCA | 47 |
| Target | `annual_medical_cost_lkr` |
| Mean annual expenditure | LKR 329,316 (approximately) |
| Median annual expenditure | LKR 282,095 (approximately) |
| Distribution | Right-skewed; skewness ≈ 1.04 |

**Predictor groups** include demographic and family characteristics, smoking and exercise, health conditions, stress, insurance plans, blood pressure, and other available attributes.

A separate **Spanish real-world health insurance portfolio** (228,711 insured-year records, 42 original variables, 2017–2019) was explored for domain context. It was **not** used to independently validate the synthetic Sri Lankan cost model.

## 🔬 Statistical Modelling Workflow

```text
Dataset understanding & quality checks
                ↓
Exploratory data analysis
                ↓
Hypothesis testing
                ↓
Leakage-aware preprocessing & train/test split
                ↓
Regression model comparison (5-fold CV)
                ↓
Selected LASSO held-out test
                ↓
Supplementary PCA & Bayesian evaluations
                ↓
Business interpretation & prototype recommendations
```

For the main regression comparison, **6,000 records** were used for training and **1,500** for the independent held-out test. Missing-value imputation, categorical encoding, and numerical scaling were handled within the training pipeline. Identifier/target-derived variables and same-year healthcare-utilization information were excluded from the pre-year prediction workflow to reduce leakage risk.

## 🧩 Project Tasks

| Task | Area | Summary |
|---|---|---|
| 01 | Problem definition | Insurance-cost analysis, stakeholders, and objectives |
| 02 | Research landscape | Review of medical expenditure modelling and statistical approaches |
| 03 | Data analysis | Dataset selection, data quality, descriptive statistics, and cost patterns |
| 04 | Hypothesis testing | Smoking, insurance plans, diabetes, region, and gender comparisons |
| 05 | Predictive modelling | Mean baseline, OLS, Ridge, LASSO, Elastic Net, and supplementary models |
| 06 | Experimental design | Hypothetical CRD and RCBD treatment comparison |
| 07 | PCA | Dimensionality reduction and regression comparison |
| 08 | Bayesian methods | Bayesian linear regression, credible/predictive intervals, and Naïve Bayes |
| 09 | Time series | Feasibility assessment and future ARIMA/SARIMA framework |
| 10 | Innovation | MedCost Insight dashboard/decision-support concept |
| 11 | Review feedback | Documented review of the report and dashboard |
| 12 | Recommendations | Evidence-led business and governance action plan |

## 📈 Results

### 1. Descriptive findings and statistical tests

- Mean annual simulated expenditure: **LKR 329,316**.
- **165** observations (**2.20%**) were flagged as high-cost IQR outliers and retained for analysis.
- Smoking and insurance-plan groups showed statistically significant **unadjusted** differences in mean costs.
- Diabetes showed a significant difference, but a relatively small standardized effect.
- The region and gender comparisons did **not** show statistically significant mean differences at the chosen threshold.

These are **associations**, not evidence that a characteristic causes medical expenses to rise or fall.

### 2. Regression model comparison

Initial **5-fold training cross-validation**:

| Model | CV RMSE (LKR) | CV MAE (LKR) | CV R² |
|---|---:|---:|---:|
| **LASSO** | **70,556** | **54,634** | **0.8493** |
| Elastic Net | 70,582 | 54,682 | 0.8493 |
| OLS | 70,721 | 54,839 | 0.8485 |
| Ridge | 71,191 | 55,215 | 0.8476 |
| Mean-only baseline | 181,657 | 144,014 | 0.0000 |

**Selected model: LASSO**, because it delivered strong cross-validated results with coefficient shrinkage and feature selection. Differences among some leading models were small.

#### Independent held-out test: selected LASSO

| Metric | Value |
|---|---:|
| **R²** | **0.8345** |
| **MAE** | **LKR 56,765** |
| **RMSE** | **LKR 74,073** |
| Test records | 1,500 |

An R² of 0.8345 indicates approximately **83.45% of held-out simulated expenditure variation explained**, relative to the test-set mean baseline. It is **not** "83.45% accuracy" and is **not** a measured real-world claims result.

### 3. PCA evaluation

Task 07 evaluated PCA *separately using training-only 5-fold CV*. After encoding, there were **47 columns**.

| Pipeline | Dimensions | CV RMSE (LKR) | CV R² |
|---|---:|---:|---:|
| **LASSO without PCA** | **47 encoded columns** | **71,025** | **0.8484** |
| PCA 95% + Ridge | 34 components | 82,380 | 0.7960 |
| PCA 90% + Ridge | 32 components | 83,473 | 0.7906 |
| PCA 80% + Ridge | 27 components | 85,762 | 0.7789 |

**Finding:** PCA reduced dimensionality but increased prediction error. The best evaluated PCA pipeline had approximately **16% higher RMSE** than the non-PCA LASSO comparator. PCA remained exploratory.

*These Task 07 cross-validation values are not the Task 05 independent held-out test results.*

### 4. Bayesian linear regression

Bayesian regression was explored to quantify uncertainty around model estimates and predictions.

| Model | Validation RMSE (LKR) | MAE (LKR) | R² |
|---|---:|---:|---:|
| Bayesian linear regression | 94,544.63 | 73,089.81 | 0.7285 |
| OLS (same validation) | 94,553.53 | 73,093.31 | 0.7285 |

The Bayesian model reduced RMSE by only **LKR 8.90**, so it did not demonstrate a meaningful improvement in point prediction over OLS on that validation sample.

- Used an **internal validation split** of 4,800 fitting and 1,200 validation observations, from the original training set.
- **95% posterior predictive intervals** covered **94.17%** of the observed validation costs.
- Posterior coefficient credible intervals communicated uncertainty in the estimated associations.

A *credible interval for a coefficient* and a *predictive interval for a person's expenditure* represent different types of uncertainty. Neither validates a causal interpretation.

### 5. Naïve Bayes high-cost classification

High-cost cases were defined using the fitting data's 75th-percentile threshold of approximately **LKR 428,822**.

| Metric | Result |
|---|---:|
| Accuracy | 78.67% |
| Sensitivity (high-cost recall) | 44.63% |
| Specificity (lower-cost recall) | 89.91% |
| Actual high-cost cases missed | **165** |

**Confusion matrix** (1,200 internal validation observations):

| Predicted class | Actual lower-cost | Actual high-cost |
|---|---:|---:|
| Lower-cost | 811 (TN) | **165 (FN)** |
| High-cost | 91 (FP) | 133 (TP) |

**Finding:** Despite reasonable overall accuracy, the classifier missed **165 of 298 actual high-cost cases**. It is **not suitable for production use without further improvement, calibration, and independent validation**.

### 6. Experimental design and time series

**Experimental design (Task 06):** Demonstrated Completely Randomized Design (CRD) and Randomized Complete Block Design (RCBD) on **hypothetically generated treatment assignments and outcomes**. No observed intervention benefit or real-world causal cost reduction was established.

**Time series (Task 09):** The main dataset did **not** contain authentic consecutive monthly insurer expenditure measurements. Therefore **ARIMA/SARIMA were proposed as future methods, not fitted or validated on the project data**. Collecting appropriate dated records is required before reporting trends, seasonality, or forecasts.

## 💡 MedCost Insight Decision-Support Concept

The proposed dashboard/analyst workflow brings together:

- Medical expenditure summaries, distributions, and outlier monitoring.
- Descriptive group comparisons, including smoking and insurance-plan segments.
- LASSO-based estimated cost views accompanied by model-error cautions.
- Potential uncertainty warnings and validation checks before any real-world use.
- Human review, privacy controls, and fairness monitoring.

**Proposed architecture:**

```text
Authorized insurance records (future pilot)
           ↓
Governed preprocessing and quality checks
           ↓
Validated, versioned LASSO candidate
           ↓
Cost patterns + prediction errors + warnings
           ↓
Human analyst review and documented decisions
```

A dashboard was shared for review during the academic project. Positive written feedback described the presentation of cost patterns, associated factors, and management-useful insights; this **does not demonstrate independent model validation, deployment feasibility, operational adoption, or financial savings**.

## 🏢 Business Recommendations

1. **Monitor expensive cases** using medians, cost distributions, outliers, and subgroup errors, not averages alone.
2. **Investigate smoking- and plan-related cost differences** without treating them as causal estimates or grounds for automatic premium decisions.
3. **Retain LASSO as the initial interpretable candidate** rather than replacing it with the evaluated PCA pipelines.
4. **Use Bayesian uncertainty only as a validated supplement**, with attention to prediction-interval width and high-cost underestimation.
5. **Improve high-cost identification** before considering use of the Naïve Bayes classifier.
6. **Collect legitimate dated expenditure records** before developing ARIMA/SARIMA forecasting.
7. **Require real-data evaluation, privacy safeguards, subgroup fairness checks, and human approval** before any organizational pilot.

## ⚠️ Limitations & Responsible Use

- The principal dataset contains **synthetic**, not verified insurer-paid claims, data.
- Model metrics are based on internal simulation and should not be generalized to Sri Lankan insurance operations.
- Statistical associations do not establish causation.
- Experimental intervention outcomes were entirely hypothetical.
- No real-world time-series forecasting was performed.
- Independent validation, calibration, fairness checks, authorization, and security are required before operational use.
- This project must **not** be used for automated coverage denial, premium changes, or decisions affecting individuals.

## 🚀 Future Work

- Obtain lawful, de-identified, representative insurance records and clarify the business target.
- Test the model independently on real, prospectively collected data.
- Evaluate high-cost prediction errors and performance across relevant subgroups.
- Validate and improve predictive-interval calibration.
- Develop secure dashboard functionality with auditable human review.
- Consider a properly approved prospective randomized trial to evaluate interventions.
- Add time-series models **only when suitable monthly historical data become available**.

## 📚 Academic Context

**Module:** IT3081 – Statistical Modelling  
**Project:** MedCost Insight – Medical Insurance Cost Analysis  
**Type:** Group academic statistical consultancy project

For detailed hypothesis test outputs, research references, methodology, charts, and supporting evidence, consult the accompanying project presentation and reports when included in this repository.

## 🤝 Contributors

Developed as a **group academic project**.

---

<div align="center">

**MedCost Insight · Statistical evidence for responsible decision support**

*Academic demonstration only · Not a deployed insurance decision system*

</div>
