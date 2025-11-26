# Peer Review: Medical Cost Regression Analysis

**Reviewer:** Adrianna Webb  
**Date:** November 25, 2025  
**Notebook Reviewed:** [regression_aaron.ipynb](https://github.com/hrawp/applied-ml-hrawp/blob/main/notebooks/final/regression_aaron.ipynbb)

---

## 1. Clarity & Organization

**Strengths:**
- Clear, logical structure with well-defined sections
- Good progression from data loading through modeling to conclusions
- Reflections appropriately placed after each major section

**Areas for Improvement:**
- Section 2.1 exists but no 2.2 or 2.3 - either remove sub-numbering or add missing subsections
- Some code cells lack explanatory text (e.g., correlation matrix appears without context)
- Some plots missing titles or axis labels

**Actionable Suggestions:**
1. Add brief markdown before major visualizations explaining their purpose
2. Ensure all plots have titles and labeled axes
3. Add markdown cells explaining what the correlation matrix reveals about feature relationships

---

## 2. Feature Selection & Justification

**Strengths:**
- Excellent feature engineering with **3ptindex** (smoker_e × age × bmi) - this shows creative thinking
- Good use of one-hot encoding for `region` categorical variable
- Smart comparison: Case 1 (all features) vs Case 2 (engineered feature only)
- Correlation matrix provides data-driven support for feature relationships

**Areas for Improvement:**
- No explanation of *why* these three factors or this specific formula for 3ptindex
- Missing discussion of which original features in Case 1 contribute most
- No discussion of potential multicollinearity issues
- Region encoding choice not explained (why one-hot for region but binary for sex/smoker?)

**Actionable Suggestions:**
1. Add justification for the 3ptindex formula
2. Print and discuss Case 1 feature coefficients: `print(dict(zip(features, lr_model.coef_)))` to show which features matter most
3. Explain encoding

---

## 3. Model Performance & Comparisons

**Strengths:**
- Comprehensive testing: Linear Regression, Ridge, ElasticNet, and Polynomial (degree 3)
- Testing on two feature sets (Case 1 vs Case 2) provides valuable comparison
- Consistent metrics reported (R², RMSE, MAE)
- Residual plots provide diagnostic information beyond metrics

**Areas for Improvement:**
- No comparison table - results scattered across cells make comparison difficult
- Missing interpretation: What does RMSE of $5,900 mean for stakeholders?
- No discussion of overfitting potential for Polynomial Cubic model
- No explanation of why Ridge/ElasticNet didn't improve results
- Minimal residual plot interpretation

**Actionable Suggestions:**
1. Create a comparison table
2. Add interpretation of RMSE
3. Check overfitting: Compare training vs test R² for Polynomial model to assess generalization

---

## 4. Reflection Quality

**Strengths:**
- Reflections present after each major section
- Honest about challenges (difficulty visualizing inputs, finding engineered variable)
- Recognition that 3ptindex was powerful

**Areas for Improvement:**
- Reflection 1 just states facts without insight ("no missing values")
- Missing "why" explanations
- No discussion of real-world implications or limitations

**Actionable Suggestions:**
1. Enhance Reflection 1
2. Add Reflection 2
3. Deepen Reflection 5 with hypothesis
4. Add limitations in Section 6

---

## Overall Assessment

**Summary:**
Solid regression analysis demonstrating good technical skills and creative feature engineering. The 3ptindex variable is a highlight showing domain understanding. With improved explanations and deeper reflections, this work would move from good to excellent.

**Key Strengths:**
- Creative and effective feature engineering (3ptindex)
- Comprehensive model comparison
- Good code organization

**Priority Improvements:**
1. Add model comparison summary table
2. Interpret metrics in practical terms
3. Deepen reflections with "why" explanations
4. Explain feature selection reasoning
5. Discuss model limitations