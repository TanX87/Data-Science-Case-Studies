# Customer Waiting Time Analytics

### Developed during the Coppel Datathon 2024

---

## Overview

This project analyzes customer waiting times across different service segments to identify operational bottlenecks and propose data-driven strategies for improving service efficiency.

Using descriptive statistics and a Gamma Generalized Linear Model (GLM), the project evaluates how variables such as service segment, state, arrival time, and date influence waiting times. The results provide actionable insights that support operational decision-making by identifying the locations with the greatest opportunities for service improvement.

---

## Business Problem

Long customer waiting times reduce operational efficiency and negatively affect customer satisfaction.

However, waiting times are not homogeneous across all locations. Understanding which operational factors explain these differences enables organizations to allocate resources more efficiently, optimize operations, and improve the customer experience.

This project aims to identify these factors and quantify their impact using statistical modeling.

---

## Dataset

The analysis was conducted using operational customer service records provided for the Coppel Datathon.

The dataset includes variables such as:

- Service Segment
- State
- Store
- Arrival Time
- Call Time
- Departure Time
- Customer Status

> **Note:** The original dataset is proprietary and cannot be publicly shared.

---

## Methodology

The project followed the analytical workflow below:

1. Data preprocessing and cleaning
2. Feature engineering
3. Exploratory Data Analysis (EDA)
4. Stratified sampling
5. Distribution fitting
6. Gamma Generalized Linear Model (GLM)
7. Model interpretation
8. Scenario evaluation
9. Business recommendations

---

## Statistical Methods

- Exploratory Data Analysis (EDA)
- Descriptive Statistics
- Stratified Sampling
- Distribution Fitting
- Gamma Generalized Linear Model (GLM)
- Model Interpretation

---

## Key Findings

- Retail represented approximately **54%** of the customer portfolio.
- Waiting times differed considerably across service segments.
- Nine states consistently presented the highest waiting times.
- Operational characteristics varied significantly between states, suggesting that resource allocation strategies should not be uniform.
- The Gamma GLM successfully quantified the influence of operational variables on customer waiting times.
- Scenario analysis suggested that targeted operational improvements could reduce waiting times by approximately **2–5%**, depending on the implemented strategy.

---

## Business Recommendations

Based on the model results, several improvement opportunities were identified for the states with the highest waiting times:

- Reallocate personnel according to operational demand.
- Strengthen employee training focused on service efficiency.
- Optimize staffing schedules during peak hours.
- Redistribute customer demand across available service points.
- Continuously monitor operational performance to evaluate implemented strategies.

These recommendations provide a data-driven framework for improving customer experience while increasing operational efficiency.

---

## Technologies

- R
- dplyr
- ggplot2
- Generalized Linear Models (GLM)
- Statistical Modeling
- Data Visualization

---

## What I Learned

This project strengthened my ability to work with large operational datasets, apply statistical models to real business problems, and translate quantitative findings into actionable business recommendations.

Beyond developing a predictive model, it reinforced the importance of combining statistical analysis with business understanding to support evidence-based decision-making.

---

## 📄 Additional Resources

- Final presentation available in `/presentation`
- Technical report available in `/report`
- Complete R implementation available in `/code`
