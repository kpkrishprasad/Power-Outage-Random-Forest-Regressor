# Power-Outage-Random-Forest-Regressor

by Krish Prasad (krprasad@ucsd.edu)

---

## Introduction


The dataset at hand is a comprehensive collection of data relating to major power outages across various U.S. states from the year 2000 to 2016. It spans 1525 entries, each documenting a unique power outage event with detailed attributes such as the year, month, state affected, NERC (North American Electric Reliability Corporation) region, climate information, the cause of the outage, its duration, impact on customers, and various economic factors.
Major outages, as defined by the Department of Energy, are those impacting at least 50,000 customers or causing an unplanned firm load loss of at least 300 MW. This rich dataset is composed of 55 variables, encapsulating not only the outage events themselves but also an array of associated regional characteristics, such as climatic conditions, land use, electricity consumption patterns, and economic factors of the states affected by these outages.

Compiled using publicly available datasets from reputable sources like the DOE's Office of Electricity Delivery and Energy Reliability, the U.S. Energy Information Administration (EIA), the National Oceanic and Atmospheric Administration (NOAA), the U.S. Census Bureau, and the U.S. Department of Labor's Bureau of Labor Statistics, the dataset is a comprehensive resource for understanding the multifaceted nature of power outages.

Our focused research question is: "How have characteristics of major power outages changed over time? Is there a clear trend?" To address this, we will analyze historical trends and patterns, assess risk predictors, and understand the vulnerabilities and resilience of power systems.

Significant columns that pertain to our central research question "How have characteristics of major power outages changed over time? Is there a clear trend?" include:

**YEAR and MONTH**: Essential for identifying time-based trends in outage occurrences.

**U.S._STATE** and POSTAL.CODE: Geographic identifiers to examine state-wise impacts and patterns.

**NERC.REGION and CLIMATE.REGION**: Regional classifications for analyzing outages in the context of regulatory entities and climatic zones.

**OUTAGE.START.DATE and OUTAGE.RESTORATION.DATE**: Markers for the duration and recovery from outages.

**CAUSE.CATEGORY and CAUSE.CATEGORY.DETAIL:**
Categorizations that could reveal the most frequent and impactful causes of outages.

**OUTAGE.DURATION, DEMAND.LOSS.MW, and CUSTOMERS.AFFECTED:** Quantitative measures of each outage's severity and reach.

**CLIMATE.CATEGORY and ANOMALY.LEVEL**: Indicators of climatic conditions during the outage, which might correlate with severe weather events.

**RES.PRICE, COM.PRICE, IND.PRICE, and TOTAL.PRICE**: Data points to correlate economic factors with outages.

**RES.SALES, COM.SALES, IND.SALES, TOTAL.SALES:**
Consumption patterns that might influence or be influenced by outages.

**POPULATION and related demographic variables:** Insight into the population dynamics at the time of the outages.


These columns, among others, will be leveraged to answer our central question. Understanding how the characteristics of power outages have changed over time is critical for multiple stakeholders. Policymakers and power companies can better prepare for future events by identifying trends and patterns in the causes and effects of outages. Researchers and the general public also stand to gain valuable insights into the resilience of the power grid and how climate, technological advancements, and urban development may be influencing outage characteristics.

Our analysis aims to not only chart the historical trends but also to forecast future vulnerabilities, guiding investments in infrastructure resilience and disaster response planning. This dataset has direct implications for the reliability of their electricity supply and the robustness of the national power grid against an array of threats and natural challenges. ​​

---
## Data Cleaning and Exploratory Data Analysis


**Data Clearning**

An examination of missingness across the dataset reveals significant variability. A notable observation is that the HURRICANE.NAMES column has a very high proportion of missing values at 95.31%, suggesting hurricanes are not a common cause of power outages in the dataset. The DEMAND.LOSS.MW and CUSTOMERS.AFFECTED columns have moderate missingness, 45.96% and 28.88% respectively, indicating potential gaps in crucial impact data.

For columns with low to moderate missingness, strategies such as mean or median imputation for numerical columns, and mode imputation or predictive modeling for categorical columns are considered. High missingness columns like HURRICANE.NAMES may be dropped if deemed non-essential, or filled with domain-specific knowledge if crucial. For key columns like DEMAND.LOSS.MW and CUSTOMERS.AFFECTED, more complex imputation methods such as regression or machine learning could be employed, given their importance in assessing the outage impact.

Temporal columns such as OUTAGE.START and OUTAGE.RESTORATION demand specific strategies like interpolation, reflecting the necessity for precise time-related data in outage analysis. Complete case analysis might be an option for certain studies where the integrity of each data point is critical, and the missingness is not systematic.

In the cleanup process, the dataset undergoes preprocessing where redundant or extraneous rows and columns are removed, such as the HURRICANE.NAMES column due to its high missingness. The original datetime columns are consolidated into OUTAGE.START and OUTAGE.RESTORATION using appropriate datetime formats for analysis. Subsequently, the cleaned dataset is saved as 'outage.csv' for further use.

Below is head to the cleaned dataset with the most important columns shown

<iframe src="assets/cleaned_dataset_head.png" width=1600 height=600 frameBorder=0></iframe>


---

**Univariate Analysis**

To understand the data's distribution, univariate plots are created for OUTAGE.DURATION using histograms. This visualization reveals the spread and central tendencies of these key variables. 

<iframe src="assets/univariate_histogram.html" width=1600 height=600 frameBorder=0></iframe>


**Bivariate Analysis**

Bivariate analyses involve creating scatter plots to explore relationships between variables like OUTAGE.DURATION and CUSTOMERS.AFFECTED. This plot are instrumental in identifying trends and correlations between the severity of an outage and its impact on customers and demand loss.

To examine the effect of climate on power outages, the average OUTAGE.DURATION is plotted against CLIMATE.CATEGORY, offering insights into how different climate conditions may influence outage lengths. Additionally, scatter plots with trendlines are employed to dissect the interaction between outage duration, customers affected, and climate categories.

<iframe src="assets/bivariate_histogram.html" width=1600 height=600 frameBorder=0></iframe>


**Interesting Aggregates**

Finally, a pivot table analys is conducted to summarize average OUTAGE.DURATION by CLIMATE.CATEGORY, total CUSTOMERS.AFFECTED by NERC.REGION, and to create a comprehensive pivot table that cross-references the CLIMATE.CATEGORY and U.S._STATE with average OUTAGE.DURATION and total CUSTOMERS.AFFECTED. This multidimensional view is essential to contextualize the outages within various regional and climatic parameters.

<iframe src="assets/pivot_table.png" width=1200 height=600 frameBorder=0></iframe>


The missingness analysis component of the code is pivotal for understanding data completeness and integrity, directing subsequent data cleaning, imputation, and analysis efforts, and ensuring the reliability of conclusions drawn from this significant dataset on power outages.







---




## Assessment of Missingness

Based on preliminary analysis, I believe the "CAUSE.CATEGORY.DETAIL" column in our dataset may exhibit characteristics of being NMAR, or Not Missing At Random. The pattern of missingness seems to depend on the 
unobserved data itself, suggesting that details are more frequently omitted when the cause is less clear or more
complex to diagnose. To better understand this missingness and potentially move it towards MAR (Missing At Random), additional contextual information regarding each incident's circumstances and the data collection process would be invaluable. Interviews with the data entry personnel or field technicians could provide insights into whether certain types of cause details are systematically harder to ascertain, which may lead to their absence in the dataset. If it is found that the likelihood of a "CAUSE.CATEGORY.DETAIL" being missing is independent of its actual content after obtaining further information, we could then consider the missingness to be MAR rather than NMAR.



**Permutation Test Description for CAUSE.CATEGORY.DETAIL Missingness depedent on CAUSE.CATEGORY**

I performed a permutation test to investigate the dependency of missing data 
in the "CAUSE.CATEGORY.DETAIL" column on other columns in the dataset. It starts by creating a
binary column to represent the missingness status of the "CAUSE.CATEGORY.DETAIL" entries. Then, 
it uses a chi-square test to examine the initial association between the categories of cause and
the missingness of the details.

Following this, the code runs a permutation test, which involves shuffling the missingness column
and recalculating the chi-square statistic multiple times to create a distribution of chi-square 
values under the null hypothesis (that there's no association). By comparing the original chi-square 
statistic to this distribution, it computes a permutation p-value, giving a sense of how extreme the
observed statistic is.

The permutation test results indicate a statistically significant dependency of missingness in the 
"CAUSE.CATEGORY.DETAIL" column on the "CAUSE.CATEGORY," implying an NMAR scenario. With an observed 
chi-square statistic of 442.4872 and a permutation p-value of 0.0, the evidence strongly suggests that
the likelihood of details being missing is not random but tied to the specific cause categories. This 
pattern of missingness denotes that certain cause categories might inherently prompt more omissions in 
the corresponding detail field. To transition our understanding from NMAR to MAR, additional investigation 
into the data collection process and potential biases related to cause categories is necessary. This would 
enable us to determine if the missingness can be attributed to observable variables rather than the missing data itself.



**Permutation Test Summary for CAUSE.CATEGORY.DETAIL Missingness dependent OUTAGE.DURATION Analysis**


The analysis explored the relationship between the duration of power outages and the missingness of 
detailed cause categories in a dataset of power outages. Approximately 30.8% of the "CAUSE.CATEGORY.DETAIL" 
entries are missing, while about 3.2% of the "OUTAGE.DURATION" entries are missing. A permutation test was 
conducted to determine if the missingness of the "CAUSE.CATEGORY.DETAIL" is associated with the "OUTAGE.DURATION".

The observed mean difference in outage duration between outages with missing and non-missing "CAUSE.CATEGORY.DETAIL"
was approximately -9.85 hours, indicating that outages with missing detail information tend to be shorter.
The permutation test yielded a p-value of 0.087. This p-value suggests that the observed association between
outage duration and the missingness of cause category detail could occur by chance about 8.7% of the time 
under the null hypothesis of no association.

<iframe src="assets/Cause_Category_Missingness.html" width=1200 height=600 frameBorder=0></iframe>

In the context of missing data mechanisms, the findings do not strongly indicate a Non-Missing At Random (NMAR)
situation where the probability of missingness depends on the unobserved data itself. Instead, the evidence 
towards NMAR is weak, given the relatively high p-value (above the conventional threshold of 0.05), and 
suggests that further investigation or data might be needed to conclusively determine the nature of the missingness.
 


---
## Hypothesis Testing

For the hypothesis testing component of our analysis, we formulated two hypotheses concerning the relationship between
economic characteristics and the severity of power outages. The null hypothesis (H0) stated that there is no significant
association between a state's per capita real gross state product (PC.REALGSP.STATE) and the duration of power outages
(OUTAGE.DURATION). Conversely, the alternative hypothesis (H1) posited that a significant association does exist.

The Pearson correlation coefficient served as our test statistic due to its suitability in measuring the strength of a 
linear relationship between two continuous variables. We selected a permutation test for our analysis, a non-parametric 
method that makes no assumptions about the distribution of the data, thus offering flexibility and robustness against 
non-normal data distributions. The significance level (alpha) was set at the conventional threshold of 0.05, offering a 
balance between sensitivity to detect true associations and caution against false positives.

Upon conducting the permutation test, with 10,000 replicates to ensure the stability of our p-value estimate, we calculated
an observed correlation coefficient of approximately -0.0064. This value indicates a negligible inverse relationship 
between economic characteristics and outage duration. The permutation test yielded a p-value of 0.5643, significantly 
above our alpha threshold, suggesting that the observed correlation could well have occurred by chance under the null hypothesis.

Consequently, we do not reject the null hypothesis. Our results suggest that there is no evidence of a significant association
between the economic characteristic of per capita real gross state product and the severity of power outages, as measured 
by their duration, within the dataset.

Our choices were appropriate for the question at hand because they allowed us to directly measure the relationship between 
two key variables of interest using a reliable statistical method. Moreover, by using a permutation test, we avoided the 
potential pitfalls of parametric assumptions that may not have been satisfied.

It's crucial to note that the conclusion is not absolute; while we found no evidence to support the alternative hypothesis, this 
does not prove that no relationship exists. Other economic characteristics not analyzed here may still influence outage
severity, or the relationship may be non-linear or moderated by other factors. Our conclusion is based on the dataset and methods
used, and it's always possible that different data or methods could yield different results.

---

## Framing a Prediction Problem

The task of forecasting the duration of major power outages is a regression problem. The response variable
in the model is 'OUTAGE.DURATION', chosen because it quantitatively captures the impact of an outage, providing a clear measure
of severity. To evaluate the model's performance, we used the mean squared error (MSE) and the R-squared score. MSE was selected 
because it reflects the average squared difference between the estimated values and the actual value, which is crucial for
assessing the prediction accuracy in terms of the same unit as the response variable. The R squared
score was used to understand the proportion of variance in the outage duration that our model could explain.

Those metrics were favored over others, like mean absolute error, because MSE punishes larger errors more severely, which is
appropriate in the context of power outages where severe underestimations could be more costly. The R squared score is a
standardized indicator, making it easier to communicate the model's performance.

It's important to note that the features selected for the model are based on data that would be available at the time of
prediction. These include the anomaly level to represent weather conditions, the state's per capita real gross state product
to account for economic status, and the percentage of the urban population to reflect the density of the potentially
 affected area. We avoided using data that would not be available before an outage, such as the exact cause, since our aim
was to make predictions before an event occurs.

The current model serves as a starting point, and based on our analysis, further improvements are necessary. In future iterations, we
will explore the inclusion of more nuanced features that could provide a better prediction, such as infrastructure age or historical
outage data, always considering the information available prior to an outage event for our predictions.



---

## Baseline Model

The linear regression model in question is designed to predict the duration of power outages, utilizing three specific features. These features are 'ANOMALY.LEVEL', which is likely an ordinal variable indicating the level of anomaly in a systematic order; 'PC.REALGSP.STATE', which appears to be a quantitative variable that could denote the per capita real Gross State Product, reflective of economic data on a state level; and 'POPPCT_URBAN', another quantitative variable representing the percentage of the population residing in urban settings.

In the model's preparation phase, the dataset is cleansed of any instances with missing target values to ensure that the model trains on a complete dataset. The target variable, 'OUTAGE.DURATION', being a continuous one, lends itself well to the regression analysis conducted by the model.

A split of the data is performed, segregating it into a training set, which constitutes 80% of the data, and a test set, which makes up the remaining 20%. This split is essential for the model's validation, allowing for performance assessment on data that was not encountered during the training phase.

Upon training the linear regression model with the training dataset, predictions are made on the test set. The model's performance is subsequently evaluated using two metrics: the mean squared error (MSE) and the coefficient of determination, commonly referred to as R-squared (R²). The MSE provides insight into the average of the squares of the errors, essentially measuring the average squared difference between the estimated values and the actual value. A low MSE indicates a high level of precision in the model's predictions. R², on the other hand, quantifies the percentage of the variance in the dependent variable that is explained by the independent variables in the model. An R² value close to 1 implies that the model explains a large portion of the variance in the target variable.

It is also critical to note that the reliability of the model hinges on the assumption that the relationship between the independent variables and the dependent variable is linear. Any deviations from this assumption, such as non-linear relationships, outliers, or high leverage points, could potentially undermine the model's performance.


The negative R squared score of -0.003 indicates that the model's predictive ability is worse than a simple linear regression model that would
predict the mean outage duration for all observations. This implies that the three features selected—temperature anomaly level,
per capita real gross state product, and the percentage of urban population—are not sufficient to capture the complex factors 
that influence outage duration.

The large MSE further confirms that the predictions are, on average, far from the actual outage durations, indicating high error
variance. This suggests that outage duration may be influenced by factors not included in the model, such as specific details about
the infrastructure, the precise nature of the weather events, or other socio-economic factors.

To improve the model's performance, additional features that could better explain the variance in the outage duration should be
considered. Moreover, exploring more sophisticated modeling techniques or data transformations could help in capturing the non-linear 
relationships that a simple linear regression model may miss. The current model serves as a baseline from which more refined and 
accurate predictive models could be developed.


---

## Final Model

In the enhanced model, features such as 'ANOMALY.LEVEL', 'POPPCT_URBAN', 'CUSTOMERS.AFFECTED', 'COM.PERCEN', and 'IND.CUSTOMERS' are included because they are believed to capture the complexity and the impact of power outages. 'ANOMALY.LEVEL' likely signifies the severity of an anomaly which could correlate with the length of the outage. Since urban populations ('POPPCT_URBAN') may have more infrastructure but also potentially more efficient repair services, this feature could impact outage duration. The number of 'CUSTOMERS.AFFECTED' reflects the scale of an outage and might influence the urgency and resources allocated for repairs. 'COM.PERCEN' and 'IND.CUSTOMERS' could serve as proxies for the commercial importance and industrial demand within an affected area, affecting how quickly services are restored.

For these features, specific transformations are applied to normalize their distributions. 'ANOMALY.LEVEL' undergoes a quantile transformation to a normal distribution, potentially stabilizing variance and making the model less sensitive to outliers. 'CUSTOMERS.AFFECTED' is log-transformed to address its likely right-skewed distribution, which helps in reducing the influence of extreme values.

The model chosen for the prediction task is the RandomForestRegressor, known for handling non-linear relationships and interactions between features without requiring extensive feature engineering. It operates by building a multitude of decision trees at training time and outputting the mean prediction of the individual trees. The hyperparameters were not specified in the provided code; however, tuning these parameters is critical in improving model performance. Methods like grid search or random search could have been employed to find the optimal set of hyperparameters.

The final model achieved a significant improvement with an R-squared value of 0.464107046741818 compared to the previous R-squared of -0.003. This improvement suggests that the RandomForestRegressor, along with the feature transformations and selections made, resulted in a model that better captures the variance in the data, providing a more accurate and robust prediction of outage durations. The mean squared error is not provided, but a higher R-squared value alone indicates a better fit to the data.

---

## Fairness Analysis
---

Choice of Group X and Group Y:
In the analysis, Group X comprises data points representing populations less than 10 million. In contrast, Group Y includes populations equal to or exceeding the 10 million mark. This division is grounded in the hypothesis that the size of a population may influence the model's prediction of outage durations due to factors like infrastructure complexity and response capabilities.

Evaluation Metric:
Root Mean Squared Error (RMSE) has been chosen as the metric for evaluation. RMSE provides a clear indication of the model's accuracy by measuring the magnitude of the error in the model's predictions. It is particularly useful for regression problems and offers a straightforward interpretation of model performance.

Null and Alternative Hypotheses:
The Null Hypothesis (H0) posits that the model does not discriminate between large and small populations regarding outage duration predictions. Specifically, it suggests that the model's prediction errors, as quantified by RMSE, are consistent between Groups X and Y, and any observed variation is attributable to random fluctuations rather than systematic bias.

The Alternative Hypothesis (H1), however, proposes that the model exhibits a bias. It claims there is a statistically significant disparity in RMSE when comparing predictions for smaller populations (Group X) with larger ones (Group Y), implying that the model might be unfairly penalizing or favoring one group over the other.

Test Statistic:
The chosen test statistic is the absolute difference in RMSE between Group X and Group Y. This value is indicative of the disparity in prediction errors and serves as a key indicator in the fairness analysis.

Significance Level:
Although not explicitly stated in the code, the analysis generally adheres to a conventional significance level of α = 0.05. This threshold is a commonly accepted benchmark for statistical tests, below which the null hypothesis would be rejected in favor of the alternative.

Resulting p-value:
The permutation test yields a p-value, which in this case stands at 0.685. This p-value measures the probability of observing the computed difference in RMSE (or a more extreme one) under the assumption that the null hypothesis is true.

Conclusion Based on p-value:
Given the obtained p-value of 0.685, which is substantially higher than the conventional significance level of 0.05, there is insufficient evidence to reject the null hypothesis. We therefore conclude that the model does not show a statistically significant difference in performance when predicting outage durations for populations below and above 10 million. This suggests that the model is fair in this respect.



