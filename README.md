# Hypertension Treatment Efficacy & Risk Prediction

## Executive Summary
This project analyzes clinical data from 351 patients at a Hypertension Cardiology Center. The objective is twofold: to predict 3-month blood pressure (BP) control failure at the point of prescription, and to identify the most efficacious antihypertensive drug classes for patients with specific comorbidities. 

By engineering a class-balanced **Extreme Gradient Boosting (XGBoost)** model and utilizing **Explainable AI (SHAP)**, this analysis provides highly actionable clinical insights. The predictive model achieved a **91.43% accuracy**, identifying Baseline Systolic and Diastolic BP as the overwhelming primary risk factors. Furthermore, efficacy analysis reveals distinct, data-driven pathways for precision prescribing based on patient comorbidities.

---

## Problem 1: Predicting BP Control Failure
**Objective:** *What predicts BP control failure at 3 months, and can we identify high-risk patients at the point of prescription?*

### Methodology
To prevent "data leakage," all future-state variables (e.g., Medication Adherence, Follow-up BP) were removed. An XGBoost Classifier was trained strictly on Day-1 patient profiles. Due to a severe class imbalance (~90% treatment failure rate), `scale_pos_weight` and strict tree-depth regularization were applied to ensure the algorithm learned true physiological thresholds.

### Key Findings
*   **Predictive Power:** The model successfully predicts 3-month treatment failure with an overall accuracy of **91.43%**, achieving an **86% recall** on the minority class (patients who succeeded) and a **92% recall** on high-risk failures.
*   **The Predictors (SHAP Analysis):** 
    1. **Baseline Systolic & Diastolic BP** are the absolute strongest predictors of treatment failure.
    2. Prescribed **Antihypertensive Class** and patient **Gender** act as moderate secondary predictors.
    3. Specific metabolic comorbidities (Obesity, Dyslipidemia, Diabetes) had minimal predictive weight on their own regarding short-term failure compared to raw baseline BP metrics.

*(Note: View the SHAP feature importance plot in the repository files).*

---

## Problem 2: Drug Efficacy by Comorbidity
**Objective:** *Which antihypertensive drug class delivers the best outcomes for patients with specific comorbidity profiles?*

### Methodology
Patients were segmented into four primary comorbidity cohorts: Chronic Kidney Disease (CKD), Diabetes Mellitus, Dyslipidemia, and Obesity. Within each cohort, the 3-month clinical success rate (achieving controlled BP) was calculated across all prescribed drug classes.

### Key Findings
1.  **Dominance of Combination Therapy:** For metabolic disorders, monotherapy is clinically insufficient. **Combination Therapy** aggressively outperforms all standalone drugs, delivering success rates of **25.9%** (Dyslipidemia), **25.0%** (Diabetes), and **18.4%** (Obesity).
2.  **The CKD Exception:** For patients suffering from Chronic Kidney Disease, Combination Therapy was less effective (11.5%). Instead, **Thiazide Diuretics** proved superior, achieving a peak success rate of **17.6%**.
3.  **The Failure of ACE Inhibitors:** Strikingly, ACE Inhibitors recorded a **0.0% success rate** across all four comorbid cohorts, indicating they are ineffective as a first-line monotherapy for complex patients at this center.

*(Note: View the complete Treatment Success Rate Heatmap in the repository files).*

---

## Actionable Clinical Recommendations
Based on the data, the following operational adjustments are recommended to optimize patient outcomes:

1.  **EHR "Red Flag" Triage Protocol:** Implement an automated alert in the Electronic Health Record (EHR) system for patients presenting with extreme Baseline BP. Schedule these patients for a mandatory 2-week check-in rather than waiting the standard 3 months.
2.  **Standardize First-Line Prescriptions:** Update clinical guidelines to prescribe **Combination Therapy** as the default first-line treatment for hypertensive patients presenting with Diabetes, Dyslipidemia, or Obesity.
3.  **Specialized CKD Routing:** Ensure hypertensive patients with Chronic Kidney Disease are preferentially prescribed **Thiazide Diuretics**.
4.  **Review ACE Inhibitor Usage:** Conduct an immediate clinical review regarding the continued use of standalone ACE Inhibitors for complex patients.
