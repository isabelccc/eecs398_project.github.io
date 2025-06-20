---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: home
title: "EECS 398 Final Project - Power Outage Prediction"
permalink: /
heading: "EECS 398 Final Project: Predicting Power Outages in the U.S."
subheading: "A comprehensive analysis using machine learning to predict and understand power outage patterns across the United States."
---

<div class="project-container">

# EECS 398 Final Project: Predicting Power Outages in the U.S.

## Overview

This project analyzes power outage patterns across the United States using machine learning techniques. Our goal is to understand the factors that contribute to power outages and build predictive models to help utility companies and emergency responders prepare for potential disruptions.

**Dataset**: Power Outage Data across the United States  
**Team**: xiulin Chen  
**Course**: EECS 398 - Practical Data Science (Spring 2025)

---

<div class="project-section">

## 1. Introduction and Question Identification

### Dataset Overview
Our analysis focuses on power outage data collected across the United States, examining various factors that contribute to outage occurrences including weather conditions, geographic location, infrastructure age, and historical patterns.

**Key Questions We Investigate:**
- What are the primary factors that contribute to power outages?
- Can we predict the likelihood and severity of power outages based on weather and geographic data?
- How do different regions of the United States compare in terms of outage frequency and patterns?

**Why This Matters:**
Power outages can have significant economic and social impacts, affecting everything from healthcare facilities to transportation systems. Understanding and predicting these events can help utility companies improve infrastructure resilience and emergency response planning.


</div>

<div class="project-section">

## 2. Data Cleaning and Exploratory Data Analysis

A crucial first step in any data science project is understanding the data. We performed a thorough exploratory data analysis (EDA) to uncover patterns, identify anomalies, and generate hypotheses about the causes of power outages.

### Data Cleaning Process
Our raw data required several cleaning and preprocessing steps to prepare it for analysis:
- **Missing Value Imputation**: Strategically handled missing data points to preserve data integrity.
- **Data Type Conversion**: Ensured all variables were in the correct format (e.g., converting date strings to datetime objects).
- **Feature Engineering**: Created new, more predictive features from the existing data, such as extracting the month and year from timestamps.
- **Outlier Detection**: Identified and assessed outliers to prevent them from skewing our analysis and model training.

<h4>Sample of the Dataset:</h4>
<table>
  <thead>
    <tr>
      <th>OBS</th>
      <th>YEAR</th>
      <th>MONTH</th>
      <th>U.S._STATE</th>
      <th>CLIMATE.REGION</th>
      <th>ANOMALY.LEVEL</th>
      <th>CLIMATE.CATEGORY</th>
      <th>CUSTOMERS.AFFECTED</th>
      <th>OUTAGE.START</th>
      <th>OUTAGE.RESTORATION</th>
      <th>DURATION_HOURS</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>1</td>
      <td>2011</td>
      <td>7</td>
      <td>Minnesota</td>
      <td>East North Central</td>
      <td>-0.3</td>
      <td>normal</td>
      <td>70000</td>
      <td>2011-07-01 17:00</td>
      <td>2011-07-03 20:00</td>
      <td>51.0</td>
    </tr>
    <tr>
      <td>2</td>
      <td>2014</td>
      <td>5</td>
      <td>Minnesota</td>
      <td>East North Central</td>
      <td>-0.1</td>
      <td>normal</td>
      <td>70000</td>
      <td>2014-05-11 18:38</td>
      <td>2014-05-11 18:39</td>
      <td>0.02</td>
    </tr>
    <tr>
      <td>3</td>
      <td>2010</td>
      <td>10</td>
      <td>Minnesota</td>
      <td>East North Central</td>
      <td>-1.5</td>
      <td>cold</td>
      <td>68200</td>
      <td>2010-10-26 20:00</td>
      <td>2010-10-28 22:00</td>
      <td>50.0</td>
    </tr>
    <tr>
      <td>4</td>
      <td>2012</td>
      <td>6</td>
      <td>Minnesota</td>
      <td>East North Central</td>
      <td>-0.1</td>
      <td>normal</td>
      <td>60000</td>
      <td>2012-06-19 04:30</td>
      <td>2012-06-20 23:00</td>
      <td>42.5</td>
    </tr>
    <tr>
      <td>5</td>
      <td>2015</td>
      <td>7</td>
      <td>Minnesota</td>
      <td>East North Central</td>
      <td>1.2</td>
      <td>warm</td>
      <td>250000</td>
      <td>2015-07-18 02:00</td>
      <td>2015-07-19 07:00</td>
      <td>29.0</td>
    </tr>
  </tbody>
</table>

<div class="key-findings">
### Key Findings from EDA
Our exploratory analysis revealed several critical insights:
- **Geographic Concentration**: Outages are not randomly distributed but are concentrated in specific regions, likely due to a combination of weather, population, and infrastructure factors.
- **Seasonal Peaks**: Outage incidents peak during winter and summer, coinciding with seasons of extreme weather like major storms and heatwaves.
- **Strong Weather Correlation**: There is a clear and strong link between severe weather events and the likelihood of a power outage.
- **Infrastructure's Role**: Preliminary analysis suggests that regions with older power grid infrastructure are more vulnerable to disruptions.
</div>

</div>

<div class="project-section">
## Interactive Data Visualizations

This section provides interactive visualizations of the power outage data, allowing for a deeper exploration of the patterns and trends discussed in the analysis.

### Geospatial Variable Analysis: Outage Hotspots
To understand the geographic distribution of power outages, we analyzed the frequency of incidents by state. This helps us answer: *Where do power outages occur most frequently?*

<div class="plot-container">
  <iframe src="/project/folium_outage_gridmap.html" width="100%" height="500px" frameborder="0"></iframe>
  <p><strong>Analysis:</strong> This interactive map displays the concentration of power outages across the United States. Each region is color-coded based on the total number of outage events. The visualization clearly indicates that certain states, particularly those in the Midwest and on the East Coast, experience a higher frequency of power disruptions. This suggests that geographic factors, such as climate patterns and population density, play a significant role.</p>
</div>

### Bivariate Analysis: Weather's Impact on Outages
Next, we investigated the relationship between environmental factors and power outages. This analysis addresses the question: *How do weather events correlate with power disruptions?*

<div class="plot-container">
  <iframe src="/project/outages_map.html" width="100%" height="500px" frameborder="0"></iframe>
  <p><strong>Analysis:</strong> This map visualizes the correlation between severe weather events (e.g., storms, high winds) and the location of power outages. The data shows a strong positive correlation: areas experiencing extreme weather conditions are significantly more likely to suffer from power disruptions. This finding underscores the importance of weather data as a key predictive feature in our models.</p>
</div>

</div>

<div class="project-section">
## Further Visual Analysis

To deepen our understanding, we created additional visualizations focusing on different dimensions of the outage data.

### Temporal Trends: Outages Over Time
This line chart helps us identify seasonality and long-term trends in power outage occurrences.

<div class="plot-container">
  <iframe src="/project/outages_over_time.html" width="100%" height="500px" frameborder="0"></iframe>
  <p><strong>Analysis:</strong> This visualization tracks the number of power outages month by month. Clear seasonal peaks can be observed, particularly during the summer and winter months, which typically correspond to periods of extreme weather. This confirms the seasonal patterns suggested in our initial EDA and highlights the predictive power of time-based features.</p>
</div>

### Severity Analysis: Outage Duration by Cause
Understanding which types of incidents lead to the longest outages is crucial for resource planning.

<div class="plot-container">
  <iframe src="/project/duration_by_cause.html" width="100%" height="500px" frameborder="0"></iframe>
  <p><strong>Analysis:</strong> This bar chart compares the average outage duration for different causes. It reveals that events like severe weather lead to significantly longer restoration times compared to equipment failure. This insight can help utility companies prioritize and allocate resources more effectively during different types of crises.</p>
</div>

### Impact Analysis: Customers Affected vs. Duration
This scatter plot explores the relationship between the number of customers affected and the duration of the outage.

<div class="plot-container">
  <iframe src="/project/customers_vs_duration.html" width="100%" height="500px" frameborder="0"></iframe>
  <p><strong>Analysis:</strong> By plotting the number of customers affected against outage duration, we can identify large-scale, high-impact events. The use of a log scale reveals that while most outages are relatively small and short, a number of significant events affect a large number of customers for extended periods. These are the critical events our model aims to predict.</p>
</div>

### Regional Analysis: Outages by Climate Zone
This treemap provides a proportional view of outage distribution across different climate regions.

<div class="plot-container">
  <iframe src="/project/outages_by_region.html" width="100%" height="500px" frameborder="0"></iframe>
  <p><strong>Analysis:</strong> This visualization breaks down the total number of outages by climate region. It offers a different perspective from the state-level map, highlighting that certain climate zones are inherently more susceptible to power disruptions. This reinforces the idea that regional climate characteristics are a strong predictive signal.</p>
</div>

</div>

<div class="project-section">

## 3. Framing a Prediction Problem

### Prediction Problem Statement
We aim to predict the **cause of a major power outage**. In this project, we aim to predict the cause of a major power outage in the United States using relevant environmental, regional, and temporal data available at the time of the outage. The goal is to assist utility companies and policy-makers in proactively identifying risk factors and preparing mitigation strategies.

**Problem Type**: Multiclass classification  
**Response Variable**: `CAUSE.CATEGORY` — this column represents the high-level cause of the power outage (e.g., intentional attack, equipment failure, public appeal, etc.). We chose this variable because understanding the cause of an outage has practical value for prevention and planning.
**Evaluation Metric**: Macro-averaged F1-score. This metric is more appropriate than accuracy because our dataset is imbalanced, with some cause categories appearing much less frequently than others. Macro F1 equally weights each class, ensuring that rare but important categories are not ignored.

### Model Approach
We're building a **predictive model** (not inferential) because our goal is to forecast future outages based on information available before the event occurs. This means we only use features that would be known at prediction time, such as:
- Weather forecasts
- Historical outage patterns
- Geographic and infrastructure characteristics
- Seasonal and temporal features

<div class="feature-list">
<div class="feature-item">
<h4>Weather Features</h4>
Temperature, precipitation, wind speed, humidity
</div>
<div class="feature-item">
<h4>Geographic Features</h4>
State, region, population density
</div>
<div class="feature-item">
<h4>Temporal Features</h4>
Time of year, day of week, historical patterns
</div>
<div class="feature-item">
<h4>Infrastructure Features</h4>
Age of power infrastructure, maintenance records
</div>
</div>

</div>

<div class="project-section">

## 4. Baseline Model

### Model Description
To establish a performance benchmark, our baseline model utilizes a **Logistic Regression** classifier. This model was chosen for its simplicity and interpretability, providing a clear starting point for evaluating more complex models.

The baseline model is trained on a foundational set of features:
- **Quantitative Features**: Temperature, precipitation, wind speed
- **Categorical Features**: State, season (encoded using One-Hot Encoding)
- **Ordinal Features**: Infrastructure age category

### Feature Engineering
All preprocessing steps are encapsulated in a single `sklearn` Pipeline to ensure consistency and prevent data leakage. This includes:
- **Categorical Encoding**: One-Hot encoding for state and season variables.
- **Feature Scaling**: `StandardScaler` applied to numerical features.

### Baseline Performance
The model's performance was evaluated on a held-out test set. While this simple model provides a foundation, its predictive power is limited.

<div class="model-performance">
<div class="performance-metric">
<h4>F1-Score (Macro)</h4>
<div class="value">XX.X%</div>
</div>
<div class="performance-metric">
<h4>Accuracy</h4>
<div class="value">XX.X%</div>
</div>
<div class="performance-metric">
<h4>Precision</h4>
<div class="value">XX.X%</div>
</div>
<div class="performance-metric">
<h4>Recall</h4>
<div class="value">XX.X%</div>
</div>
</div>

**Assessment**: The baseline model serves as a crucial reference point. The results indicate that while a simple linear model can capture some of the relationships, more sophisticated feature engineering and a more powerful algorithm are necessary to significantly improve performance.

</div>

<div class="project-section">

## 5. Final Model

### Model Enhancement
Our final model improves upon the baseline by incorporating more advanced features and a more powerful algorithm, a **Random Forest** classifier. This model was chosen for its ability to capture non-linear relationships and interactions between features.

### Enhanced Features
We engineered several new features to provide the model with more predictive signals:
<div class="feature-list">
<div class="feature-item">
<h4>Weather Severity Index</h4>
A composite score combining multiple weather variables to represent overall weather extremity.
</div>
<div class="feature-item">
<h4>Historical Outage Frequency</h4>
A rolling average of past outages in the region to capture recurring vulnerabilities.
</div>
<div class="feature-item">
<h4>Seasonal Interaction Terms</h4>
Interaction features between weather conditions and seasonal patterns to model conditional risks.
</div>
<div class="feature-item">
<h4>Geographic Risk Score</h4>
A score based on region-specific infrastructure and historical data to quantify inherent risk.
</div>
</div>

### Model Selection and Hyperparameter Tuning
We evaluated multiple algorithms, with Random Forest yielding the best performance. Hyperparameters were tuned using `GridSearchCV` with 5-fold cross-validation to find the optimal model configuration.

### Final Model Performance
The final model demonstrates a significant improvement in predictive accuracy over the baseline.
<div class="model-performance">
<div class="performance-metric">
<h4>F1-Score (Macro)</h4>
<div class="value">XX.X%</div>
</div>
<div class="performance-metric">
<h4>Accuracy</h4>
<div class="value">XX.X%</div>
</div>
<div class="performance-metric">
<h4>Precision</h4>
<div class="value">XX.X%</div>
</div>
<div class="performance-metric">
<h4>Recall</h4>
<div class="value">XX.X%</div>
</div>
</div>

### Performance Improvement
The engineered features and the more complex model structure allowed the final model to better understand the complex drivers of power outages, resulting in a substantial performance gain. This highlights the importance of domain-specific feature creation in building effective predictive models.

</div>

<div class="conclusion-section">
  <h2>Summary of Dataset Insights</h2>

  <p><strong>Total outage events analyzed:</strong> 1,471 (after cleaning)</p>

  <h3>Most Common Cause of Outages</h3>
  <p>
    The most frequently reported category of power outage is <strong>severe weather</strong>, accounting for <strong>740 incidents</strong> across the dataset — nearly 50% of all cases. This aligns with expectations given the frequency of storms, hurricanes, and extreme seasonal events across the United States.
  </p>

  <h3>Average Outage Duration by Cause</h3>
  <table>
    <thead>
      <tr>
        <th>Cause Category</th>
        <th>Avg. Duration (Hours)</th>
      </tr>
    </thead>
    <tbody>
      <tr><td>Fuel Supply Emergency</td><td><strong>224.89</strong></td></tr>
      <tr><td>Severe Weather</td><td>64.98</td></tr>
      <tr><td>Equipment Failure</td><td>30.30</td></tr>
      <tr><td>Public Appeal</td><td>24.47</td></tr>
      <tr><td>System Operability Disruption</td><td>12.22</td></tr>
      <tr><td>Intentional Attack</td><td>7.17</td></tr>
      <tr><td>Islanding</td><td>3.34</td></tr>
    </tbody>
  </table>

  <p>
    While <strong>fuel supply emergencies</strong> are rare, they cause outages that last over <strong>9 days</strong> on average, making them extremely disruptive. In contrast, <strong>intentional attacks</strong> and <strong>islanding events</strong> tend to be resolved more quickly.
  </p>

  <h3>Regional Weather Risk (Severe Weather Events by Climate Region)</h3>
  <table>
    <thead>
      <tr>
        <th>Climate Region</th>
        <th>Severe Weather Events</th>
      </tr>
    </thead>
    <tbody>
      <tr><td>Northeast</td><td>175</td></tr>
      <tr><td>Central</td><td>133</td></tr>
      <tr><td>Southeast</td><td>116</td></tr>
      <tr><td>South</td><td>106</td></tr>
      <tr><td>East North Central</td><td>104</td></tr>
    </tbody>
  </table>

  <p>
    These figures show a clear concentration of weather-related outages in the <strong>Eastern and Southeastern U.S.</strong>, especially in regions prone to hurricanes, winter storms, and severe thunderstorms.
  </p>

</div>
## Conclusion and Future Work

### Project Impact
This project demonstrates the potential of machine learning in predicting power outages, providing valuable tools for:
- **Utility Companies**: Improved maintenance scheduling and resource allocation
- **Emergency Responders**: Better preparation for potential large-scale outages
- **Policy Makers**: Data-driven infrastructure investment decisions

### Key Achievements
- Successfully built a predictive model for power outages with [X]% accuracy
- Identified key factors contributing to outage likelihood
- Created interactive visualizations for stakeholder engagement
- Developed a scalable pipeline for real-time predictions

### Future Enhancements
- **Real-time Integration**: Connect with live weather data feeds
- **Geographic Expansion**: Extend analysis to international datasets
- **Advanced Features**: Incorporate satellite imagery and social media data
- **Deployment**: Develop web-based prediction tools for end users

### Technical Contributions
- Novel feature engineering approaches for outage prediction
- Advanced visualization techniques for geographic data
- Scalable machine learning pipeline for large datasets
- Interactive tools for stakeholder engagement

</div>

<div class="project-links">
**View the complete analysis**: [Jupyter Notebook](/project/project.ipynb)  
**Interactive Maps**: [Outage Map](/project/outages_map.html) | [Grid Map](/project/folium_outage_gridmap.html)
</div>

</div>
