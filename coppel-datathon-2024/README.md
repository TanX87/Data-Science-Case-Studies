# Customer Waiting Time Analytics

### Developed during the Coppel Datathon 2025

---

## Overview

This project focuses on analyzing customer waiting times across different service segments to identify operational bottlenecks and propose data-driven strategies for improving service efficiency.

Using descriptive statistics and a Gamma Generalized Linear Model (GLM), the project evaluates how variables such as service segment, state, arrival time, and date influence waiting times. The results support operational decision-making by identifying the locations with the greatest improvement opportunities.

## Business Problem

Long customer waiting times reduce operational efficiency and negatively affect customer satisfaction.

However, waiting times are not homogeneous across all locations. Understanding which operational factors explain these differences allows organizations to allocate resources more efficiently and improve customer experience.

This project aims to identify these factors and quantify their impact using statistical modeling.

## Dataset

The analysis was conducted using operational customer service records provided for the Coppel Datathon.

The dataset contains information such as:

- Service segment
- State
- Store
- Arrival time
- Call time
- Departure time
- Customer status

*The original dataset is proprietary and cannot be publicly shared.*

## Statistical Methods

- Exploratory Data Analysis (EDA)
- Stratified Sampling
- Distribution Fitting
- Gamma Generalized Linear Model (GLM)
- Model Interpretation
- Scenario Evaluation

## Key Findings

- Retail represented approximately 54% of the customer portfolio.

- Average waiting time differed considerably across service segments.

- Nine states consistently presented the highest waiting times.

- Operational characteristics varied significantly between states, suggesting that resource allocation strategies should not be uniform.

- The Gamma GLM successfully quantified the influence of operational variables on customer waiting times.
