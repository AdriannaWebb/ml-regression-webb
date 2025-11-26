# Final Project: Regression Analysis – Housing Price Prediction

**Author:** Adrianna Webb  
**Date:** November 25, 2025  

## Project Overview

This project demonstrates regression modeling techniques applied to predicting residential housing sale prices in Ames, Iowa. Using a dataset of 1,460 homes with 79 features, the analysis explores how property characteristics such as overall quality, square footage, age, and location influence sale prices.

The project compares multiple regression approaches including baseline Linear Regression, Ridge Regression with regularization, and Polynomial Regression to identify the optimal model for price prediction.

## Project Links

- **Jupyter Notebook:** [ml_regression_webb.ipynb](ml_regression_webb.ipynb)
- **Peer Review:** [peer_review.md](peer_review.md)
- **Project Summary:** [project_summary.md](project_summary.md)

## Dataset

The Ames Housing dataset contains residential home sales data with features including:
- Property characteristics (square footage, lot size, rooms)
- Quality ratings (overall quality, condition)
- Temporal features (year built, year remodeled, year sold)
- Location (neighborhood)
- Categorical features (materials, amenities, property types)

**Target Variable:** SalePrice (property sale price in dollars)

## Key Findings

- **Best Model:** Linear Regression and Ridge Regression both achieved **R² = 0.787** on test data with **RMSE ≈ $24,093**
- **Key Predictors:** Overall quality, total square footage, and bathroom count were the strongest drivers of home value
- **Feature Engineering:** Created meaningful composite features (TotalSF, HouseAge, TotalBath, RemodAge) that improved model interpretability
- **Model Complexity:** Polynomial Regression (degree=3) suffered from severe overfitting, demonstrating that simpler models often generalize better

## Repository Structure

```
├── data/
│   ├── train.csv                  # Training dataset
│   └── test.csv                   # Test dataset
├── ml_regression_webb.ipynb       # Main analysis notebook
├── peer_review.md                 # Peer review of classmate's work
├── project_summary.md             # Detailed project documentation
├── README.md                      # This file
├── pyproject.toml                 # Project dependencies
└── .gitignore                     # Git ignore file
```

## Setup Instructions

### Prerequisites

- Python 3.12 or higher
- UV package manager (recommended) or pip

### Installation Steps

#### Using UV 

1. **Clone the repository:**
   ```bash
   git clone https://github.com/[your-username]/ml_regression_webb.git
   cd ml_regression_webb
   ```

2. **Install UV** (if not already installed):
   ```bash
   curl -LsSf https://astral.sh/uv/install.sh | sh
   ```

3. **Create virtual environment and install dependencies:**
   ```bash
   uv venv
   source .venv/bin/activate  # On Windows: .venv\Scripts\activate
   uv pip install -e .
   ```

4. **Launch Jupyter Notebook:**
   ```bash
   jupyter notebook ml_regression_webb.ipynb
   ```

### Running the Notebook

Once Jupyter opens in your browser:
1. Navigate to `ml_regression_webb.ipynb`
2. Select the kernel (should match your virtual environment)
3. Run all cells

## Dependencies

Key Python packages used in this project:
- **pandas** - Data manipulation and analysis
- **numpy** - Numerical computing
- **matplotlib** - Data visualization
- **seaborn** - Statistical data visualization
- **scikit-learn** - Machine learning library
  - Linear models (LinearRegression, Ridge, ElasticNet)
  - Preprocessing (StandardScaler, PolynomialFeatures, SimpleImputer)
  - Model evaluation metrics (r2_score, mean_absolute_error, root_mean_squared_error)
  - Pipeline utilities

## Results Summary

| Model | Test R² | Test RMSE | Test MAE |
|-------|---------|-----------|----------|
| Linear Regression (Baseline) | 0.787 | $24,093 | $17,336 |
| Ridge Regression (Pipeline 1) | 0.787 | $24,090 | $17,332 |
| Polynomial Regression (Pipeline 2) | 0.337 | $42,536 | $22,740 |

The baseline Linear Regression and Ridge Regression models both explain approximately 79% of the variance in housing prices with an average prediction error of about $24,000.

## Skills Demonstrated

- **Data Preprocessing:** Handling missing values, outlier detection and removal, categorical encoding
- **Feature Engineering:** Creating composite features (TotalSF, HouseAge, TotalBath, RemodAge)
- **Model Development:** Training and evaluating multiple regression models
- **Pipeline Implementation:** Building scikit-learn pipelines with preprocessing and modeling steps
- **Model Evaluation:** Using R², MAE, RMSE metrics and visual diagnostics
- **Data Visualization:** Creating informative plots for exploratory analysis and model evaluation


## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

- Dataset: Ames Housing Dataset (Dean De Cock)
- Course: Applied Machine Learning
- Institution: Northwest Missouri State University

