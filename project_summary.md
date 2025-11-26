# Project Summary: Housing Price Prediction Using Regression Analysis

## Problem Definition (Front Matter)

- **Project Title:** Housing Price Prediction Using Regression Analysis
- **Author/Alias:** Adrianna Webb
- **Brief description of the business or analytical problem:** Predicting residential housing sale prices in Ames, Iowa to help real estate professionals, home buyers, and sellers make informed pricing decisions
- **What decision or action the model is intended to support:** Support pricing decisions for home sales, property valuations, and real estate investment analysis
- **Who would use this model:** Real estate agents, home appraisers, property investors, home buyers and sellers

---

## 1. Identify the Target Variable

- **What is the exact name of the target variable:** SalePrice
- **Confirm the target is continuous and numeric:** Yes, SalePrice is a continuous numerical variable representing the property's sale price in dollars
- **Confirm no unexpected values, missing values, or outliers that distort the target:** The original dataset had no missing values in SalePrice. Outliers were handled by removing extreme values using the IQR method (values beyond Q1 - 1.5*IQR and Q3 + 1.5*IQR)
- **Why is this target meaningful for prediction:** Sale price is the primary metric of interest in real estate transactions. Accurate price predictions help buyers make competitive offers, sellers set realistic asking prices, and agents provide valuable guidance to clients

---

## 2. Review Feature Variables

- **What features are available:** The dataset contains 79 features including property characteristics (square footage, lot size, rooms), quality ratings, temporal features (year built, remodeled, sold), location (neighborhood), and categorical features (materials, amenities)
- **Are they numerical, categorical, or mixed:** Mixed - numerical (square footage, years, counts) and categorical (neighborhood, heating quality, foundation type)
- **Are any features irrelevant or redundant:** Yes. Dropped Alley, PoolQC, Fence, MiscFeature (>50% missing). Id is irrelevant. Redundancy exists between FullBath/TotalBath and floor square footage/TotalSF
- **Are any features ethically or legally restricted:** No. All features are standard property characteristics publicly disclosed in real estate transactions
- **Could any features cause leakage:** YrSold and MoSold could leak information if predicting future sales, but are valid for this historical analysis
- **Provide at least one example of possible leakage in this domain:** Including "AppraisedValue" would be leakage since appraisals reference sale prices. Similarly, "ListPrice" could leak information as it's often informed by expected sale price

---

## 3. Understand Relationships and Distributions

- **What patterns or correlations appear between features and the target:** Strong positive correlations exist between SalePrice and OverallQual, GrLivArea, TotalBsmtSF, and GarageArea. Quality ratings show the strongest relationship. YearBuilt and YearRemodAdd show moderate positive correlations
- **Are any features strongly correlated with each other (multicollinearity):** Yes. GrLivArea and TotalSF both measure living space. TotalBsmtSF correlates with 1stFlrSF. FullBath and TotalBath overlap. GarageArea and GarageCars measure garage capacity. Addressed through feature engineering and selection
- **Are any transformations needed:** StandardScaler was applied in pipelines. Log transformation of SalePrice could improve performance but was not implemented
- **Are there noticeable outliers:** Yes. LotArea had extremely large lots (up to 215,245 sq ft), GrLivArea had properties over 4,000 sq ft, and SalePrice had homes above $400,000
- **How were outliers handled:** (1) Removed properties with GrLivArea > 4,000 sq ft, (2) removed top 1% of LotArea, (3) removed SalePrice outliers beyond 1.5 * IQR

---

## 4. Select the Regression Model

- **What baseline model did you choose:** Linear Regression
- **Why is it appropriate for this dataset:** Linear Regression is appropriate because it: (1) works well with continuous targets, (2) provides interpretable coefficients, (3) requires no hyperparameter tuning, (4) establishes performance benchmarks, (5) is computationally efficient
- **What assumptions does this model make:** (1) Linear relationship between features and target, (2) independence of observations, (3) homoscedasticity (constant variance), (4) normality of residuals, (5) no perfect multicollinearity
- **Are those assumptions approximately met:** Reasonably met but not perfectly. Linear relationships exist for many features though some non-linearity likely present. Independence is satisfied. Homoscedasticity and normality not formally tested but likely somewhat violated. Multicollinearity exists but not severe enough to break the model

---

## 5. Evaluate Model Performance

**Common regression metrics include:**

| Abbrev | Full Name                    | Definition                                                                                      | When to Use                                                                     |
|--------|------------------------------|--------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------|
| MAE    | Mean Absolute Error          | Average size of prediction errors; treats all errors equally regardless of direction.           | A simple, easy-to-interpret metric when outliers should not dominate the score. |
| MSE    | Mean Squared Error           | Average of squared errors; penalizes larger errors more heavily.                                | When large mistakes are costly and should be penalized more.                    |
| RMSE   | Root Mean Squared Error      | Square root of MSE; returns error to original units, representing the typical prediction error. | To get error expressed in the same units as target and to penalize big errors. |
| R²     | Coefficient of Determination | Measures how much variation in the target variable the model explains (1=perfect, 0=mean model).| To measure overall model fit and how well features explain target variation. |

**Key questions:**

- **Which metrics did you use:** R², MAE, MSE, and RMSE
- **How were these metrics chosen:** These provide complementary perspectives: R² measures overall fit, MAE provides interpretable average error, RMSE penalizes large errors (important for expensive homes)
- **What is the model's performance:** 
  - Baseline Linear Regression: Test R² = 0.787, Test RMSE = $24,093, Test MAE = $17,336
  - Pipeline 1 (Ridge): Test R² = 0.787, Test RMSE = $24,090, Test MAE = $17,332
  - Pipeline 2 (Polynomial): Test R² = 0.337, Test RMSE = $42,536, Test MAE = $22,740
- **Is the performance reasonable for this application:** Yes for baseline and Ridge. R² of 0.787 means 79% of variance explained. RMSE of $24,093 represents ~13% error, acceptable for initial pricing guidance
- **What errors matter more to stakeholders and why:** Both matter but differently. Overestimating leads to unsold homes. Underestimating costs sellers money. Agents likely prefer slight underestimations for successful sales

---

## 6. Improve the Model

There are several ways to improve a regression model. These techniques help address nonlinearity, poor scaling, irrelevant features, overfitting, or general lack of predictive power.

**Use Polynomial Features**
- Adds curved relationships between features and the target.
- Useful when the baseline linear model underfits or when scatterplots show curved patterns.

**Add Scaling / Normalization**
- Ensures features are on comparable scales.
- Especially important for models sensitive to feature magnitude (Linear Regression with regularization, Polynomial Regression).

**Improve Feature Selection (may require engineering additional features)**
- Removing irrelevant variables can reduce noise.
- Creating useful new combinations or transformations can reveal hidden structure (e.g. Body Mass Index from height and weight)

**Add Regularization (Ridge, Lasso)**
- Helps prevent overfitting by shrinking coefficients or setting them exactly to zero.
- Useful when there are many correlated features or unstable weights.

**Enhance Pipelines**
- Combine preprocessing and modeling into one repeatable process.
- Improves reproducibility and reduces human error.

**Key questions:**

- **What alternate models or improvements were tried:** (1) Baseline Linear Regression, (2) Ridge Regression with StandardScaler, (3) Polynomial Features (degree=3) with StandardScaler
- **Why did you try each improvement:** Ridge for L2 regularization to reduce overfitting. StandardScaler because Ridge is scale-sensitive. Polynomial Features to capture non-linear relationships
- **Which changes improved performance:** Ridge performed identically to baseline (Test R² = 0.787, RMSE = $24,090), showing minimal improvement
- **Why do you think those changes helped:** Ridge didn't help significantly because baseline already generalizes well with minimal overfitting (train R² 0.844 vs test R² 0.787)
- **Which changes did NOT improve performance:** Polynomial Features (degree=3) dramatically worsened performance (test R² = 0.337, RMSE = $42,536), showing severe overfitting
- **Why might those approaches have failed:** Polynomial degree=3 created hundreds of interaction terms, allowing the model to memorize training data (R² = 0.922) rather than learn generalizable patterns
- **If you had more time, which additional improvements might you try next:** Test polynomial degree=2, tune Ridge alpha values, try ElasticNet, use cross-validation, apply log transformation to SalePrice, create targeted interaction terms, implement feature selection, test tree-based models

---

## 7. Validate and Compare Models

- **How were models compared:** Side-by-side comparison using all four metrics (R², MAE, MSE, RMSE) on train and test sets. Results shown in comparison table and bar charts
- **Which metrics were compared:** R², MAE, MSE, and RMSE across all three models
- **Which model performed best:** Baseline Linear Regression and Ridge Regression tied for best (Test R² ≈ 0.787, RMSE ≈ $24,090)
- **How do you know:** Test metrics clearly show Ridge and baseline are tied, while Polynomial's test R² of 0.337 and RMSE of $42,536 make it significantly worse
- **Does the best-performing model generalize well:** Yes. The small gap between training R² (0.844) and test R² (0.787) indicates good generalization
- **Discuss:** Simpler is often better. Ridge provided no improvement over baseline because the baseline already generalizes well. Polynomial Features dramatically demonstrated that unjustified complexity leads to overfitting. For deployment, either baseline or Ridge would work, with Ridge potentially preferred if expanding features in the future

---

## 8. Real-World Interpretation

- **What does the model tell us about how the features influence the target:** 
  - OverallQual: Strongest impact ($15,582 per quality point) - material/finish quality drives value
  - TotalBath: Strong positive ($10,871 per bathroom) - bathroom count highly valued
  - TotalSF: Positive ($39 per sq ft) - size matters
  - HouseAge: Negative (-$271 per year) - depreciation over time
  - BedroomAbvGr: Negative (-$4,502) - more bedrooms means smaller rooms when controlling for total size
  - Neighborhood, Electrical: Modest positive impacts

- **What practical insights can stakeholders use:**
  - Sellers: Highlight quality ratings and bathroom count. Consider upgrades before selling
  - Buyers: Evaluate quality ratings and bathroom counts for fair pricing
  - Appraisers: Use as baseline ($24k error) for valuations
  - Agents: Provide objective estimates and justify prices using feature coefficients

- **Is the model stable and reliable enough for real decisions:** Moderately reliable for supporting decisions but not as sole factor. With 13% error ($24k), it's useful for initial pricing but significant for individual transactions. Best as starting point refined with market knowledge

- **How do you know:** Consistent train/test performance shows stability. Reasonable error metrics relative to average prices. Interpretable coefficients align with domain knowledge. However, 13% error could mean $20-30k mistakes on individual homes

- **Any limitations or caveats:**
  - Geographic: Trained only on Ames, Iowa
  - Temporal: 2006-2010 data may not reflect current market
  - Label encoding may lose information
  - 21% unexplained variance (aesthetic appeal, market timing, etc.)
  - Some counterintuitive coefficients need investigation
  - No confidence intervals provided

---

## 9. Final Notes

- **Key limitations:**
  - 2006-2010 data may not reflect current market
  - Ames, Iowa specific - not generalizable
  - Label encoding may lose categorical information
  - 13% average error significant for individual transactions
  - Some counterintuitive coefficients need investigation

- **Future improvements:**
  - Implement one-hot encoding for categoricals
  - Test polynomial degree=2
  - Apply log transformation to SalePrice
  - Create targeted interaction terms
  - Test tree-based models

- **Any open questions or assumptions:**
  - **Assumption:** Label encoding preserves sufficient information
  - **Assumption:** Linear relationships adequately capture feature-price dynamics
  - **Question:** Why does BedroomAbvGr have negative coefficient?
  - **Question:** Would one-hot encoding for Neighborhood improve predictions?
  - **Question:** How much would log transformation improve performance?
  - **Question:** What omitted variables explain remaining 21% variance?