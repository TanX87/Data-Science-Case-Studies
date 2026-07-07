# Health Score Prediction Using Machine Learning

## Overview

This project develops a predictive model to estimate an individual's health score using statistical analysis and machine learning techniques.

The objective is to identify the main lifestyle and demographic factors associated with health outcomes and build a predictive framework capable of estimating health scores based on personal characteristics.

## Methodology

The analysis follows a predictive modeling approach, including:

- Data preprocessing and variable transformation.
- Exploratory Data Analysis (EDA).
- Correlation analysis using Pearson, Spearman, and Kendall methods.
- Statistical analysis of categorical variables through ANOVA.
- Feature engineering through variable categorization.
- Development of a Random Forest Regression model.
- Hyperparameter optimization using Randomized Search.
- Model evaluation through validation performance.

## Key Findings

The exploratory analysis identified relevant relationships between health score and lifestyle factors.

The variables with stronger associations with Health Score included:

- Diet quality.
- Body Mass Index (BMI).
- Exercise frequency.
- Sleep quality.
- Alcohol consumption.

The optimized Random Forest model improved validation performance, increasing the coefficient of determination (R²) from 0.638 to 0.703, demonstrating better predictive capability after hyperparameter tuning.

## Technical Stack

- Python
- Pandas
- NumPy
- Scikit-learn
- Statsmodels
- Matplotlib
- Seaborn

## Risk Analytics Perspective

This project demonstrates the application of statistical analysis and machine learning techniques to health risk modeling, focusing on the identification of key factors associated with individual health outcomes.

The methodology can be extended to predictive analytics applications in areas such as health risk assessment, preventive strategies, and data-driven decision making.
