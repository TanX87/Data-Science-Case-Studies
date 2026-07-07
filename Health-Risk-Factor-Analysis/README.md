# Health Profile Analysis Using Principal Component Analysis

## Overview

This project applies Principal Component Analysis (PCA) to identify hidden patterns and reduce dimensionality in health-related data.

The objective is to transform multiple lifestyle and demographic variables into a smaller set of interpretable components that capture the main sources of variation in individual health profiles.

## Methodology

The analysis follows a dimensionality reduction approach, including:

- Data preprocessing and variable transformation.
- Feature standardization.
- Categorical variable encoding through dummy variables.
- Correlation analysis between health-related variables.
- Principal Component Analysis (PCA).
- Analysis of explained variance.
- Interpretation of component loadings to identify latent health factors.

## Key Findings

The PCA analysis allowed the identification of relevant underlying dimensions within the health dataset.

The main components captured combinations of variables related to:

- Nutrition and lifestyle patterns.
- Age and alcohol consumption behavior.
- Body composition and health-related characteristics.

The resulting components provide a simplified representation of the original variables while preserving the main information contained in the dataset.

## Technical Stack

- Python
- Pandas
- NumPy
- Scikit-learn
- Matplotlib
- Seaborn

## Analytical Perspective

This project demonstrates the use of multivariate statistical techniques for extracting meaningful patterns from complex datasets.

Dimensionality reduction methods such as PCA are valuable in risk analytics, exploratory modeling, and predictive frameworks by improving interpretability and identifying relevant latent factors behind observed behaviors.
