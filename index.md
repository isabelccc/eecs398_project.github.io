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

<h1>EECS 398 Final Project: Predicting Power Outages in the U.S.</h1>

<h2>Overview</h2>

<p>This project analyzes power outage patterns across the United States using machine learning techniques. Our goal is to understand the factors that contribute to power outages and build predictive models to help utility companies and emergency responders prepare for potential disruptions.</p>
<ul>
  <li><strong>Dataset:</strong> Power Outage Data across the United States</li>
  <li><strong>Team:</strong> Xiulin Chen</li>
  <li><strong>Course:</strong> EECS 398 - Practical Data Science (Spring 2025)</li>
</ul>

<div class="project-section">

<h2>1. Introduction and Question Identification</h2>

<h3>Dataset Overview</h3>
<p>Our analysis focuses on power outage data collected across the United States, examining various factors that contribute to outage occurrences including weather conditions, geographic location, infrastructure age, and historical patterns.</p>

<p>
  Our project leverages a comprehensive dataset detailing major power outage events across the United States, spanning from the year 2000 to 2016.
  Each row in the dataset represents a single outage incident, enriched with contextual features such as climate conditions, regional infrastructure metrics, population data, and outage metadata.
  The dataset originates from U.S. government and energy infrastructure reports.
</p>

<p><strong>Total Rows:</strong> ~1535</p>

<h4>Key Columns for Analysis:</h4>
<ul>
  <li><strong>YEAR, MONTH:</strong> When the outage happened</li>
  <li><strong>U.S._STATE, CLIMATE.REGION:</strong> Where it occurred</li>
  <li><strong>CLIMATE.CATEGORY, ANOMALY.LEVEL:</strong> Climate conditions</li>
  <li><strong>CUSTOMERS.AFFECTED:</strong> Impact</li>
  <li><strong>OUTAGE.START, OUTAGE.RESTORATION:</strong> Timing</li>
  <li><strong>CAUSE.CATEGORY:</strong> What caused the outage</li>
</ul>

<h4>Key Questions We Investigate:</h4>
<ul>
  <li>What are the primary factors that contribute to power outages?</li>
  <li>Can we predict the likelihood and severity of power outages based on weather and geographic data?</li>
  <li>How do different regions of the United States compare in terms of outage frequency and patterns?</li>
  <li>What are the characteristics of major power outages with higher severity?</li>
</ul>

<h4>Why This Matters:</h4>
<p>Power outages can have significant economic and social impacts, affecting everything from healthcare facilities to transportation systems. Understanding and predicting these events can help utility companies improve infrastructure resilience and emergency response planning.</p>

</div>


<div class="project-section">
  <h2>2. Data Cleaning and Exploratory Data Analysis</h2>

  <p>
    A crucial first step in any data science project is understanding the data. We performed a thorough exploratory data analysis (EDA) to uncover patterns, identify anomalies, and generate hypotheses about the causes of power outages.
  </p>

  <h3>Data Cleaning Process</h3>
  <p>We performed several critical data cleaning operations to ensure the dataset was analysis-ready and aligned with the true underlying data generating process. Each step addressed structural issues, semantic inconsistencies, or outliers present in the raw data, improving both data quality and model performance.</p>

<ul>
  <li>
    <strong>Date-Time Reconstruction:</strong>  
    The raw dataset recorded outage start and restoration information in two separate columns: a date (e.g., <code>OUTAGE.START.DATE</code>) and a time (e.g., <code>OUTAGE.START.TIME</code>). These were originally stored as strings or datetime objects in inconsistent formats. We combined the date and time fields into unified datetime columns (<code>OUTAGE.START</code> and <code>OUTAGE.RESTORATION</code>) using <code>pd.to_datetime</code> with error coercion. This allowed us to calculate meaningful features like outage duration.
  </li>
    <li>
    <strong>Duration Calculation:</strong>  
    From the parsed datetime fields, we computed <code>DURATION_HOURS</code>, representing the total length of an outage in hours. This transformation is crucial as outage length is a strong indicator of outage severity and infrastructure resilience.
  </li>
    <li>
    <strong>Invalid Value Removal:</strong>  
    Rows with invalid or missing datetime values (due to parsing failures or malformed entries) were dropped using <code>dropna()</code> to avoid introducing noise in time-based analyses. We also ensured that numeric columns such as <code>CUSTOMERS.AFFECTED</code> and <code>ANOMALY.LEVEL</code> were properly coerced into numeric types using <code>pd.to_numeric(errors='coerce')</code>, converting malformed or corrupted values to NaN.
  </li>
    <li>
    <strong>Outlier Filtering:</strong>  
    We examined the distribution of outage durations and number of customers affected. Values above the 99th percentile for <code>DURATION_HOURS</code> (approximately 389 hours) were removed, as these were likely data-entry errors or events not representative of typical outage behavior. Similarly, any event affecting more than 10 million customers was excluded. These steps helped prevent skewed model learning and provided a more realistic representation of outage conditions.
  </li>
    <li>
    <strong>Data Type Conversions and Cleanup:</strong>  
    We cast relevant fields to appropriate types for model compatibility (e.g., integer for <code>YEAR</code>, float for <code>ANOMALY.LEVEL</code>, datetime for timestamps). This ensured pipeline compatibility for one-hot encoding, scaling, and modeling downstream.
  </li>





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
      <tr><td>1</td><td>2011</td><td>7</td><td>Minnesota</td><td>East North Central</td><td>-0.3</td><td>normal</td><td>70000</td><td>2011-07-01 17:00</td><td>2011-07-03 20:00</td><td>51.0</td></tr>
      <tr><td>2</td><td>2014</td><td>5</td><td>Minnesota</td><td>East North Central</td><td>-0.1</td><td>normal</td><td>70000</td><td>2014-05-11 18:38</td><td>2014-05-11 18:39</td><td>0.02</td></tr>
      <tr><td>3</td><td>2010</td><td>10</td><td>Minnesota</td><td>East North Central</td><td>-1.5</td><td>cold</td><td>68200</td><td>2010-10-26 20:00</td><td>2010-10-28 22:00</td><td>50.0</td></tr>
      <tr><td>4</td><td>2012</td><td>6</td><td>Minnesota</td><td>East North Central</td><td>-0.1</td><td>normal</td><td>60000</td><td>2012-06-19 04:30</td><td>2012-06-20 23:00</td><td>42.5</td></tr>
      <tr><td>5</td><td>2015</td><td>7</td><td>Minnesota</td><td>East North Central</td><td>1.2</td><td>warm</td><td>250000</td><td>2015-07-18 02:00</td><td>2015-07-19 07:00</td><td>29.0</td></tr>
    </tbody>
  </table>

  <h3>Key Findings from EDA</h3>
  <ul>
    <li><strong>Geographic Concentration</strong>: Outages are not randomly distributed but are concentrated in specific regions, likely due to a combination of weather, population, and infrastructure factors.</li>
    <li><strong>Seasonal Peaks</strong>: Outage incidents peak during winter and summer, coinciding with seasons of extreme weather like major storms and heatwaves.</li>
    <li><strong>Strong Weather Correlation</strong>: There is a clear and strong link between severe weather events and the likelihood of a power outage.</li>
    <li><strong>Infrastructure's Role</strong>: Preliminary analysis suggests that regions with older power grid infrastructure are more vulnerable to disruptions.</li>
  </ul>

  <h2>Interactive Data Visualizations</h2>
  <p>This section provides interactive visualizations of the power outage data, allowing for a deeper exploration of the patterns and trends discussed in the analysis.</p>
  <h3>Univariate Analysis : Outage Duration (Box Plot)</h3>
  <iframe src="{{ site.baseurl }}/univariate_duration_box.html" width="100%" height="500px" frameborder="0"></iframe>
  <p><strong>Analysis:</strong>
  This box plot illustrates the distribution of outage durations in hours. The median outage duration is approximately 21 hours, with most outages lasting under 60 hours. However, there are several extreme events exceeding 140 hours, which are visualized as outliers. These long outages may be due to severe weather or infrastructure failures and are rare but impactful. This insight supports our focus on duration as a key indicator of severity.</p>
<h3>Univariate Analysis: Customers Affected (Histogram)</h3>
<iframe src="{{ site.baseurl }}/univariate_customers.html" width="100%" height="500px" frameborder="0"></iframe>
<p><strong>Analysis:</strong>
This histogram displays the distribution of the number of customers affected by each outage. The distribution is highly right-skewed — most outages affect fewer than 250,000 customers, while a few extreme cases impact over 1 million. These rare but massive events are important for understanding system vulnerability and planning mitigation efforts. The presence of such a long tail highlights the need for careful handling of this variable during model training, potentially via transformation or robust scaling.</p>
  
  <h3>Bivariate Analysis: Weather's Impact on Outages</h3>
  
  <iframe src="{{ site.baseurl }}/outages_map.html" width="100%" height="500px" frameborder="0"></iframe>

  <p><strong>Analysis:</strong> 
This interactive map visualizes the geographic distribution and severity of power outages across the United States, with color-coded markers reflecting weather-related conditions. Larger red circles indicate higher outage severity and frequency, often due to extreme weather.

The coastal regions—particularly the Southeast (e.g., Florida, North Carolina), Gulf Coast (e.g., Texas, Louisiana), and the West Coast (e.g., California)—show a higher concentration of severe outages. These areas are more prone to hurricanes, wildfires, and heavy storms, aligning with known climate vulnerabilities. In contrast, many inland regions experience fewer or less severe outages, indicated by smaller green and orange markers. This spatial pattern highlights the importance of incorporating regional climate risk into outage prediction models and utility infrastructure planning.
</p>





  <h2>Further Visual Analysis</h2>

  <p>To deepen our understanding, we created additional visualizations focusing on different dimensions of the outage data.</p>

  <h3>Temporal Trends: Outages Over Time</h3>
  <p>This line chart helps us identify seasonality and long-term trends in power outage occurrences.</p>

  <iframe src="{{ site.baseurl }}/outages_over_time.html" width="100%" height="500px" frameborder="0"></iframe>

  <p><strong>Analysis:</strong> This visualization tracks the number of power outages month by month. Clear seasonal peaks can be observed, particularly during the summer and winter months, which typically correspond to periods of extreme weather. This confirms the seasonal patterns suggested in our initial EDA and highlights the predictive power of time-based features.</p>

  <h3>Severity Analysis: Outage Duration by Cause</h3>
  <p>Understanding which types of incidents lead to the longest outages is crucial for resource planning.</p>

  <iframe src="{{ site.baseurl }}/duration_by_cause.html" width="100%" height="500px" frameborder="0"></iframe>

  <p><strong>Analysis:</strong> This bar chart compares the average outage duration for different causes. It reveals that events like severe weather lead to significantly longer restoration times compared to equipment failure. This insight can help utility companies prioritize and allocate resources more effectively during different types of crises.</p>



 <h3>Regional Analysis: Outages by Climate Zone</h3>
  <p>
    This treemap provides a proportional view of outage distribution across different climate regions.
  </p>

  <iframe src="{{ site.baseurl }}/outages_by_region.html" width="100%" height="500px" frameborder="0"></iframe>

  <p><strong>Analysis:</strong>
    This treemap shows the proportion of major U.S. power outages across different climate regions. Larger boxes indicate a higher number of recorded outages in that region. The Northeast, West, and Central zones exhibit the largest shares, highlighting their relative vulnerability to power disruptions, likely due to population density, infrastructure, and exposure to extreme weather.
  </p>
</div>

<div class="project-section">

  <h2>3. Framing a Prediction Problem</h2>

  <h3>Prediction Problem Statement</h3>
  <p>
    We aim to predict the <strong>cause of a major power outage</strong> in the United States using relevant environmental, regional, and temporal data available at the time of the outage.
    The goal is to assist utility companies and policy-makers in proactively identifying risk factors and preparing mitigation strategies.
  </p>

  <ul>
    <li><strong>Problem Type:</strong> Multiclass classification</li>
    <li><strong>Response Variable:</strong> <code>CAUSE.CATEGORY</code> — this column represents the high-level cause of the power outage (e.g., intentional attack, equipment failure, public appeal, etc.). We chose this variable because understanding the cause of an outage has practical value for prevention and planning.</li>
    <li><strong>Evaluation Metric:</strong> Macro-averaged F1-score — this metric is ideal for imbalanced classes as it gives equal weight to all categories, including rare but critical causes.</li>
  </ul>

  <h3>Model Approach</h3>
  <p>We're building a <strong>predictive model</strong> (not inferential), meaning we only use features that would be known at the time of the outage, such as:</p>
  <ul>
    <li>Weather forecasts</li>
    <li>Historical outage patterns</li>
    <li>Geographic and infrastructure characteristics</li>
    <li>Seasonal and temporal features</li>
  </ul>
<h3>Variables Used in the Model</h3>

<table>
  <thead>
    <tr>
      <th>Variable Name</th>
      <th>Description</th>
      <th>Data Type</th>
      <th>Role in Model</th>
      <th>Source</th>
    </tr>
  </thead>
  <tbody>
    <tr><td colspan="5"><strong>Response Variable</strong></td></tr>
    <tr><td>CAUSE.CATEGORY</td><td>High-level cause of power outage</td><td>Categorical</td><td>Target</td><td>DOE Database</td></tr>
    
    <tr><td colspan="5"><strong>Predictor Variables</strong></td></tr>
    <tr><td>YEAR</td><td>Year of outage</td><td>Integer</td><td>Predictor</td><td>DOE Database</td></tr>
    <tr><td>MONTH</td><td>Month of outage</td><td>Integer</td><td>Predictor</td><td>DOE Database</td></tr>
    <tr><td>U.S._STATE</td><td>State where outage occurred</td><td>Categorical</td><td>Predictor</td><td>DOE Database</td></tr>
    <tr><td>CLIMATE.CATEGORY</td><td>Climate condition (normal, warm, cold)</td><td>Categorical</td><td>Predictor</td><td>Climate Data</td></tr>
    <tr><td>CUSTOMERS.AFFECTED</td><td>Number of customers affected</td><td>Integer</td><td>Predictor</td><td>DOE Database</td></tr>
    <tr><td>ANOMALY.LEVEL</td><td>Temperature anomaly level</td><td>Float</td><td>Predictor</td><td>Climate Data</td></tr>
    <tr><td>SEASON</td><td>Season derived from MONTH</td><td>Categorical</td><td>Engineered</td><td>Derived</td></tr>
  </tbody>
</table>

  <h4>Variable Importance and Categorization</h4>
  <ul>
    <li><strong>Primary Predictors:</strong> Weather and climate variables (e.g., <code>ANOMALY.LEVEL</code>, <code>CLIMATE.CATEGORY</code>), geographic indicators, and outage timing.</li>
    <li><strong>Secondary Predictors:</strong> Population characteristics and prior impact indicators (e.g., <code>CUSTOMERS.AFFECTED</code>).</li>
    <li><strong>Feature Engineering:</strong> We derived <code>DURATION_HOURS</code>, <code>LAT</code>, and <code>LON</code> for modeling purposes, and extracted seasonal indicators from <code>MONTH</code>.</li>
  </ul>

  <h4>Data Quality Considerations</h4>
  <ul>
    <li>Handled missing values using strategic imputation and filtering.</li>
    <li>Applied appropriate type conversions and validations.</li>
    <li>Encoded categorical variables using one-hot encoding.</li>
    <li>Scaled numerical variables to support model convergence.</li>
  </ul>

</div>


<div class="project-section">

  <h2>4. Baseline Model</h2>

  <h3>Model Description</h3>
  <p>
    To establish a performance benchmark, our baseline model utilizes a <strong>Logistic Regression</strong> classifier. This model was chosen for its simplicity and interpretability, providing a clear starting point for evaluating more complex models.
  </p>

  <h3>Features Used</h3>
  <ul>
    <li><strong>Quantitative:</strong> ANOMALY.LEVEL, DURATION_HOURS, CUSTOMERS.AFFECTED</li>
    <li><strong>Categorical:</strong> U.S._STATE, CLIMATE.CATEGORY, CLIMATE.REGION (One-Hot Encoded)</li>
    <li><strong>Temporal:</strong> MONTH, YEAR</li>
  </ul>

  <h3>Preprocessing</h3>
  <p>
    All preprocessing steps were encapsulated in a <code>sklearn</code> Pipeline to ensure consistency and prevent data leakage:
  </p>
  <ul>
    <li><strong>Categorical Encoding:</strong> One-Hot encoding for categorical variables.</li>
    <li><strong>Feature Scaling:</strong> StandardScaler applied to numerical features.</li>
    <li><strong>Train-Test Split:</strong> 80% training, 20% testing</li>
  </ul>


  <h3>Assessment</h3>
  <p>
    The baseline logistic regression model achieved an overall accuracy of <strong>80%</strong>, largely due to its performance on the most frequent class (<strong>severe weather</strong>), which dominates the dataset. However, the <strong>macro F1-score of 0.35</strong> highlights poor performance on rare but critical outage causes like <em>fuel supply emergency</em> and <em>public appeal</em>. This motivates the need for more sophisticated models that can better handle class imbalance and nonlinear relationships.
  </p>
  </div>

<div class="project-section">
  <h2>5. Final Model</h2>

  <h3>Model Enhancement</h3>
  <p>
    Our final model improves upon the baseline by incorporating engineered features and a more powerful algorithm. We used a <strong>Random Forest Classifier</strong>, which captures complex nonlinear interactions between variables and is robust to noise and outliers.
  </p>

  <h3>Enhanced Features</h3>
  <ul>
    <li><strong>Log-Transformed Numerical Features</strong>: Applied <code>log1p</code> to <code>CUSTOMERS.AFFECTED</code> and <code>DURATION_HOURS</code> to reduce skewness and improve numerical stability.</li>
    <li><strong>Seasonal Feature</strong>: Extracted <code>SEASON</code> from the <code>MONTH</code> column using a custom transformer and one-hot encoded it.</li>
  </ul>

  <h3>Pipeline and Preprocessing</h3>
  <p>
    We used a <code>sklearn</code> <strong>Pipeline</strong> to ensure all transformations and modeling steps were cleanly encapsulated. The pipeline includes:
  </p>
  <ul>
    <li><strong>One-Hot Encoding</strong> for categorical variables: <code>U.S._STATE</code>, <code>CLIMATE.CATEGORY</code>, and derived <code>SEASON</code></li>
    <li><strong>Standard Scaling</strong> and <code>log1p</code> transformation for numerical features</li>
    <li><strong>Random Forest</strong> for final prediction</li>
  </ul>

  <h3>Hyperparameter Tuning</h3>
  <p>
    We performed a grid search over the following parameters:
  </p>
  <ul>
    <li><code>n_estimators</code>: 50, 100</li>
    <li><code>max_depth</code>: 5, 10, 15</li>
    <li><code>min_samples_split</code>: 2, 5</li>
  </ul>
  <p>
    <strong>Best Parameters:</strong> <code>max_depth=15</code>, <code>min_samples_split=2</code>, <code>n_estimators=50</code>
  </p>

  <h3>Final Model Performance</h3>
  <div class="model-performance">
    <div class="performance-metric">
      <h4>F1-Score (Macro)</h4>
      <div class="value">0.40</div>
    </div>
    <div class="performance-metric">
      <h4>Accuracy</h4>
      <div class="value">86%</div>
    </div>
    <div class="performance-metric">
      <h4>Precision</h4>
      <div class="value">0.80</div>
    </div>
    <div class="performance-metric">
      <h4>Recall</h4>
      <div class="value">0.86</div>
    </div>
  </div>

  <h3>Classification Breakdown</h3>
  <p>The table below summarizes precision, recall, and F1-score by cause category:</p>
  <table>
    <thead>
      <tr>
        <th>Cause Category</th>
        <th>Precision</th>
        <th>Recall</th>
        <th>F1-score</th>
        <th>Support</th>
      </tr>
    </thead>
    <tbody>
      <tr><td>Equipment Failure</td><td>0.00</td><td>0.00</td><td>0.00</td><td>5</td></tr>
      <tr><td>Fuel Supply Emergency</td><td>0.00</td><td>0.00</td><td>0.00</td><td>1</td></tr>
      <tr><td>Intentional Attack</td><td>0.93</td><td>0.95</td><td>0.94</td><td>39</td></tr>
      <tr><td>Islanding</td><td>0.80</td><td>0.57</td><td>0.67</td><td>7</td></tr>
      <tr><td>Public Appeal</td><td>0.00</td><td>0.00</td><td>0.00</td><td>4</td></tr>
      <tr><td>Severe Weather</td><td>0.87</td><td>0.98</td><td>0.92</td><td>140</td></tr>
      <tr><td>System Operability Disruption</td><td>0.40</td><td>0.25</td><td>0.31</td><td>16</td></tr>
    </tbody>
  </table>

  <p>
    While the model performed very well on the most frequent class (<strong>severe weather</strong>) and <strong>intentional attack</strong>, it still struggles on underrepresented categories like <strong>fuel supply emergency</strong> and <strong>public appeal</strong>. Further improvements could involve upsampling, class weighting, or advanced models like XGBoost or ensemble methods.
  </p>
</div>
<div class="conclusion-section">
 <h3>Performance Summary</h3>
  <p>
    We compared two models to predict the cause of power outages:
  </p>
  <ul>
    <li><strong>Baseline Model:</strong> Logistic Regression</li>
    <li><strong>Final Model:</strong> Random Forest Classifier</li>
  </ul>
   <h3>Key Takeaways</h3>
  <ul>
    <li>The Random Forest model improved classification accuracy by 6 percentage points over the baseline.</li>
    <li>Macro F1-score improved by 0.05, showing better balance across all cause categories—including rare ones.</li>
    <li>This performance gain reflects the benefits of using engineered features (like season) and non-linear modeling.</li>
  </ul>
  
</div>